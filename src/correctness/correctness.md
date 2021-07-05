# Correctness

We are interested in proving that the resultant program, when compiled, really does produce the intended result, without undefined behaviour. To do this, we must first define the "intended result", defined in terms of the HIR, the high-level intermediate representation, which has type information.

Precise algorithms and implementation details are specified whenever they would have an effect on the program's execution. The compiler is free to reject certain programs that conform to the HIR definition supplied here, for instance because borrow checking fails. The specific lifetimes of borrows are not considered to be a part of a program's correctness, only that the compiler must guarantee certain constraints (e.g. lifetimes never outlive the object) as specified later.

Notably, we do not specify specific algorithms for such tasks as type inference and borrow checking, since these do not contribute significantly to proving the correctness of a program at _run time_. It is therefore left as an implementation detail to provide a correct type inference algorithm and borrow checker, although proofs for certain cases may be supplied here eventually.
