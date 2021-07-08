# Function objects

## Introduction

A function object in Quill is an object that can perform dynamically dispatched function invocation. In LLVM, a function object is a data structure that contains three fields, which will be discussed in detail in the following sections. In rough C-like syntax:

```c
struct fobj {
    void* fptr;
    void* copy;
    void* drop;
};
```

Function objects are expected to be allocated on the heap, and passed around by pointer. The reason for this is explored later.

## `fptr`

The `fptr` field of a `fobj` is a pointer to the function which will be called. If the Quill type of this function object is `A -> B`, the real type signature of `fptr` should be (again in C-like syntax):

```c
B_t fptr(fobj* f, A_t a);
```

where `A_t` and `B_t` are the LLVM representations of `A` and `B`. To call the function behind a function object, invoke the `fptr` (appropriately cast to the correct function signature, given the known Quill type), where the first parameter is the `fobj*`.

## Function objects with extra data

Sometimes, we want to add extra data to a function object. For instance, the function object given by `\a -> x + a` (where `x` is a local integer variable) has an extra field it must keep track of, the value of `x`. We cannot simply represent this as a `fobj`, but we can create a new larger type to hold this data.

```c
struct fobj_lambda {
    void* fptr;
    void* copy;
    void* drop;
    int x;
};
```

This `fobj_lambda` type has all the same fields as `fobj*` does, but contains the extra `int`. The function referred to by the function object would be something like

```c
int fptr_lambda(fobj* f, int a) {
    fobj_lambda* f_cast = (fobj_lambda*) f;
    return f_cast->x + a;
}
```

By storing `x` in the function object, we can access it later in `fptr_lambda`. Now, note that by adding `x` as the _final_ field of `fobj_lambda`, we can safely cast a `fobj_lambda*` to `fobj*`, effectively forgetting that we had an extra field - although that extra field remains on the heap. When the specific `fptr` is invoked, we know that the `fobj*` must in fact be a `fobj_lambda*`, so we can cast it back and recover that extra field that we stored earlier. This is the advantage of always storing function objects as pointers, since they can carry arbitrary extra data.

Any `fptr` should only be invoked once for a given `fobj*`, since it is free to destructively use the contents of the `fobj*`. In particular, the `fptr` should dispose of the contents of the `fobj*` and free all of its memory.

## Copy and drop functions

The copy and drop functions should have signatures

```c
fobj* copy(fobj*);
void drop(fobj*);
```

These functions are automatically generated and cannot be overridden in Quill code. The `copy` function should return a semantically equivalent copy of the function object, and `drop` should free its memory without invoking the function (or causing any side effects).
