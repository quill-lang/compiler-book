# Computation rules

## Introduction

Computation rules are logical statements that allow us to define the relationships between types, and exactly how we compute the values of such types. They are analogous to typing rules in type theory, but encapsulate the fact that we must actually compute the value.

## Computation context

A computation context \\( \Upsilon_{!E\ \`a} \\) (short for _υπολογίζω_) is similar to a typing context in type theory, but represents the abstract state of the computer at some point in time \\( t \in \`a \\) during the runtime of the program. At this point in time, \\( \Upsilon \\) is capable of "executing" effects \\( !E \\). This precise meaning will be explored later.

## Execution time

Any computation will take a positive amount of time \\( \Delta t \\) to execute. How do we know that a given input lifetime is valid for the entire duration of the function's execution? The key is that a lifetime can only ever be defined as a region of MIR instructions in the same function. But a function execution is one MIR instruction, so it cannot last for only part of a lifetime. So if a function is provided a lifetime parameter, that lifetime is guaranteed to outlive the function.

Since this deduction only follows from the way in which MIR is generated, we cannot use it in our ruleset that is based only in HIR. We instead take it as an axiom that all defined lifetimes must follow this rule; they must be defined as regions of expressions.

## Computation rules in general

Computation rules are written as follows.

\\[ \frac{\Upsilon \vdash e_1\colon \tau_1}{\Upsilon_{!E\ \`a} \vdash e_2\colon \tau_2} \\]

Informally, we might say: given that a computation context \\( \Upsilon \\) has produced a value \\( e_1 \\) of type \\( \tau_1 \\), the computation context \\( \Upsilon_{!E\ \`a} \\) is capable of computing a value \\( e_2 \\) of type \\( \tau_2 \\).

Notably, we do _not_ specify which effects and lifetimes went in to computing the value \\( e_1 \\); we just know that it has already been computed. This matches the intuitive semantics of Quill; regardless of how we obtain a value, we can use it in any function. For instance, some text that we obtain from the user (with some kind of IO effect) can be supplied to a pure function (which has no effects).

This computation rule does not evaluate a computation _per se_, it simply states that such a computation can be performed, if the given effect and lifetime constraints are satisfied.

## Function computation rule

A function type follows the following computation rule:

\\[ \frac{\sigma, \tau <: \`a \quad \Upsilon \vdash e_1 \colon \sigma \overset{!E\ \`a}\to \tau \quad \Upsilon \vdash e_2 \colon \sigma}{\Upsilon_{!E\ \`a} \vdash (e_1\ e_2) \colon \tau} \\]

Informally, this states that if we have a value \\(e_1\\) of type \\(\sigma \overset{!E\ \`a}\to \tau\\) and another value \\(e_2\\) of type \\(\sigma\\), then given a context \\( \Upsilon_{!E\ \`a} \\) we can apply the function \\(e_1\\) to \\(e_2\\), yielding a value of type \\(\tau\\). Note the \\( \sigma, \tau <: \`a \\) constraint: we can only perform this computation if the types \\( \sigma, \tau \\) are valid for the duration of the function, \\( \`a \\).
