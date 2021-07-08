# Function currying

## Curried vs non-curried

Function currying is an important part of Quill code; that is, partial application of functions. For instance, given a function `foo: A -> B -> C`, and some `a: A`, we can write `foo a` to produce a function of type `B -> C` which can be used wherever an explicitly-declared `B -> C` function can.

In order to make this possible, curried and non-curried functions are both treated (in principle) as function objects by the compiler. This means, for example, that invoking `foo a` will produce a function object of the type `B -> C`. By extension, simply writing `foo` produces a function object of the type `A -> B -> C`. Of course, the compiler is free to optimise function objects away when the exact control flow can be determined at compile time, but this general strategy is used in all other cases.

## Arity of functions

The arity of a function is defined in MIR. Intuitively, it is the number of arguments that must be supplied in order to enter the function's control flow. Because we can specify functions in a number of different ways in Quill, a function's arity can not be determined from its type. For instance, consider the function `add_ints: Int -> Int -> Int` defined in two ways:

```
add_ints a b = a + b
add_ints a = \a -> a + b
```

In the first instance, the arity is two, because we need two arguments before any control flow in the function is entered. In the second instance, the arity is one, because we only need one argument in order to know what to do (namely, create the lambda). Even though these two alternatives are semantically identical, they have different MIR and LLVM IR representations.

## Converting curried functions to function objects

For every Quill function defined in a program, the Quill compiler may generate multiple LLVM functions which can be used in different circumstances. For example, the Quill code `foo a b` actually calls three different LLVM functions:

- generating the function object `foo: A -> B -> C`
- calling `foo a` to produce some `B -> C`
- calling `(foo a) b` to produce some `C`

All three of these functions are associated with the same Quill function, but are used with different "curry steps". The list of "curry steps" associated with an LLVM function details how many more parameters are allowed to be supplied to this function. For example, `foo a` has curry steps `[1]`, since we are only allowed to supply one parameter `b`. `foo` has curry steps `[1, 1]`, since we are allowed to supply one parameter `a` (the first `1`), and then a second parameter `b` (the second `1`).

For optimisation purposes, the Quill compiler is allowed to provide a different list of curry steps, e.g. `foo` could have curry steps `[2]`. This would mean that `a` and `b` must be provided at the same time, to directly produce the resulting value without creating an intermediate function object. Note that in either case, the sum of the curry steps is the arity of the function, which in this case is 2.

## Direct vs indirect

Note that there is a difference between just writing `foo a` and calling `f a` for some unknown function `f`. In the first case we are explicitly telling Quill that we want to apply `a` to the function `foo` which is known at compile time. This is called a "direct" function call. Calling `f a` without the knowledge of `f` is called an "indirect" function call, since the function call is performed via a function object. We will now explore how direct and indirect function calls differ.

## Direct function calls

A direct function call is a function call where we

- know at compile time what function we are calling, and
- specify any number of the initial arguments of the function explicitly, up to the function's arity

For instance, we could call `foo a b` directly. The Quill compiler generates an LLVM function for this:

```c
C "foo/[]d"(A a, B b);
```

The name of this LLVM function is `foo/[]d` (the quotes are simply for grouping this name into one token). Broken into its component parts, this means:

- `foo`: the function was named `foo` in Quill
- `/`: a delimiter
- `[]`: the list of curry steps, which is empty since we are not doing any function currying
- `d`: the function is being called directly

We could alternatively call `foo a` directly, and choose not to provide the last parameter. In this case, the function we want to use would be

```c
fobj* "foo/[1]d"(A a);
```

This time, the curry steps are `[1]`: the output of this function is a function object which may accept one parameter (of type `B`). By extension, we also have

```c
fobj* "foo/[1, 1]d"();
```

which is a function that simply instances `foo` as a function object. Unless the compiler can perform optimisations, this is how all functions are created by default: they are first instanced into a function object by way of a direct function, and then the arguments to the function are applied subsequently to this new function object.

Note that all arguments before (but _not_ including) the given curry steps are given as function parameters. The return type is a function object with a list of curry steps equivalent to the list of curry steps of this function. If the list of curry steps is empty, the result is instead just the output of the function.

## Indirect function calls

An indirect function call is a function call where we

- do not necessarily know at compile time what function we are calling, and
- specify a given number of extra arguments to the function

While we might not know the exact function we are calling at compile time, we are guaranteed to know its exact list of curry steps at compile time. Given a function object, we cannot change its list of curry steps. The first entry on this list is the amount of extra arguments we are allowed to supply to this function. Given a function object `f` of type `A -> B -> C` with curry steps `[1, 1]`, we can provide a value of type `A` using a function of the following form.

```c
fobj* fptr_function(fobj* f, A a);
```

The input `fobj*` is just `f`, and the function we call is the `fptr` contained inside the `fobj*`. The rest of the parameters are the parameters required for the first curry step. The return value of this function is a function object of type `B -> C` with curry steps `[1]`, which is the original list of curry steps with the first entry removed. Note that this is simply a more in-depth re-statement of the rules for invoking a function object defined in the previous section of the book.

## Summary of function call semantics

Calling a direct function with curry steps `[steps]` allows you to create a function object with curry steps `[steps]`. By providing a smaller list of curry steps, you can pass in some of the initial values of the function. You must pass in `arity - sum(steps)` arguments, so that the original function will eventually receive `arity` arguments in total.

Calling an indirect function with curry steps `[a, tail]` allows you to pass in `a` extra arguments to a function object, to yield another function object with only `[tail]` curry steps.

Direct functions allow us to "jump inside the currying chain" - providing an amount of parameters, we can create a function object holding these parameters. We can think of indirect functions as "going one level down the currying chain", since they always consume and emit a function object (unless, of course, this is the last currying step - in which case the actual function is executed and its return type becomes the only return value).

## Function object definitions

Now that we know how to call direct and indirect functions, we can see how they are defined in LLVM IR. Direct functions are easier to explain, so we will start with those. Given a function `foo` and a (valid) list of curry steps `[steps]`, the Quill compiler can generate a direct function with the signature

```c
return_type "foo/[steps]d"(some_arguments_here);
```

If `[steps] = []`, then this contains the "real" function body, the code written in Quill. Otherwise, this contains some glue code creating a function object with `[steps]` curry steps. The type of this function object is named `o/foo/last_step`, where `last_step` is the last element of `[steps]`. If `[steps] = []`, then there is no `/last_step` suffix. `foo/[steps]d` returns a `o/foo/last_step*`, which can be freely cast to and from a `fobj*`.

The `o/foo/last_step` object allocates enough storage space in advance for all of the parameters to the function, excluding the last `last_step` parameters. This prevents reallocation of the `o/foo/last_step` object every time we supply an extra parameter. The parameters that have not yet been supplied are not initialised, so until the function is fully applied there may be garbage values in the function object. Note that the `last_step` in the name of the function object is (in principle) not enough to fully define the list of curry steps of the function object; this information is kept in MIR but not preserved in the name of the type in LLVM IR; the same `o/foo/last_step` object can be used for multiple kinds of function currying strategies.

The Quill compiler can generate an indirect function with the signature

```c
"o/foo/last_step"* "foo/[a, tail]i"("o/foo/last_step"* fobj, some_arguments_here);
```

where there are exactly `a` extra arguments supplied. Note that the return type is the same function object type - in fact, it reuses the same memory allocation. The function simply stores the supplied parameters inside the given function object, and then returns it.

If the list of curry steps has only one entry, then this LLVM function contains the real Quill function body. The first `arity - a` parameters are taken from the function object, and the last `a` parameters are the LLVM function's arguments. The return type is instead the real return type of the Quill function.

This all ensures efficient direct and indirect function calls, all while conforming to the most general kind of `fobj*` calling convention. This optimises function currying even when the compiler does not know in advance which functions are called with what parameters.
