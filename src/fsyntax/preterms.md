# Preterms

A *preterm*, denoted \\( a, b, c, \alpha, \beta, \gamma \\), is given by one of the following syntactic introduction rules.

## Functional rules

The basic introduction rules of functional programming are:

- a *local variable* \\( x \\);
- an *application* \\( a\ b \\);
- an *abstraction* (or \\( \lambda \\)-abstraction) \\( \lambda (x \overset\pi : \alpha).\\, a \\);
- a *function type* (or \\( \Pi \\)-type) \\( (x \overset\pi : \alpha) \to \beta \\);
- a *let expression* \\( \mathsf{let}\\ x = a;\\; b \\);
- a *sort* \\( \mathsf{Sort}\\ u \\).

We write \\( \mathsf{Prop} = \mathsf{Sort}\\ 0 \\) and \\( \mathsf{Type}\\ u = \mathsf{Sort}\\ (u + 1) \\).

## Environmental rules

The introduction rules relating to declarations in the environment, such as algebraic types, are:

- an *instantiate expression* (or *inst expression*) \\( \mathsf{inst}\\ Q \\);
- an *intro expression* \\( \mathsf{intro}\\ Q\\ \mathsf{with}\\ y_1 \mapsto a_1, \dots, y_n \mapsto a_n \\);
- a *match expression* \\( \mathsf{match}\\ a\\ \mathsf{return}\\ \alpha\\ \mathsf{with}\\ y_1 \mapsto a_1, \dots, y_n \mapsto a_n \\);
- a *fix expression* \\( \mathsf{fix}\\ (x \overset\pi : \alpha).\\, a\\).

In a match expression, since the type we return could depend on the value of the major premise \\( a \\), we explicitly specify the type \\( \alpha \\) to return.

## Borrowing rules

The introduction rules relating to borrowing are:

- a *reference type* or *borrowed type* \\( \mathsf{ref}\\ \alpha \\);
- a *dereference expression* \\( *a \\);
- a *loan expression* \\( \mathsf{loan}\\ x\\ \mathsf{as}\\ y\\ \mathsf{with}\\ z;\\; b \\);
- a *take expression* \\( \mathsf{take}\\ x\\ \mathsf{with}\\ y_1 \mapsto a_1, \dots, y_n \mapsto a_n;\\; b \\);
- an *in expression* \\( a\\ \mathsf{in}\\ b \\);
- an *in-self expression* \\( \mathsf{inself}\\ a \\).

In a loan expression, \\( x \\) is a local variable to be loaned, \\( y \\) is the name to assign to the borrow, and \\( z \\) is a local variable which will store a proof of the proposition \\( *y = x \\).
In the surface-level Quill syntax, a new name is not bound when loaning a variable, but it can be addressed using the syntax \\( \\&x \\).

In a take expression, \\( x \\) is the name of a local variable that was loaned earlier; this expression should be in some subexpression of the right hand side of a \\( \mathsf{loan} \\) expression.
Each \\( y_i \\) is intended to be the name of a local variable in the current context, but not in the context at the time of loaning, and the \\( a_i \\) is a proof that nothing depending on the memory containing \\( x \\) occurs in the value assigned to \\( y_i \\).
