# Introduction

## Conventions

When defining a term for the first time, it will be displayed in *italics*.

## Basic ideas

- Expressive, safe, fast, in that order.
- Compile time "reference counting", leading to a borrow-checked system.
- Expression blocks. `loan` and `take`.
- Algebraic types. Inductive propositions. Subtypes.
- No universe polymorphism, but still a universe hierarchy.
- Quantitative type system. Usages correspond to the ideal storage mechanisms at runtime (given sufficient monomorphisation capabilities).
- Monomorphisation is optional. Types at runtime; no decidable properties (what about `sizeof`?), so fully erasable. Function objects.
