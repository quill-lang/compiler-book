# Precontexts

A *usage*, to be interpreted as an element of the semiring \\( R_\omega \\), is of the form
\\[ \rho, \pi ::= 0 \mid 1 \mid \omega \\]
A *binary usage*, to be interpreted as an element of the semiring \\( R_1 \\), is of the form
\\[ \sigma ::= 0 \mid 1 \\]
A *precontext* is of the form
\\[ \Gamma ::= \diamond \mid \Gamma, x \overset\rho : \alpha \\]
We suppress the syntax "\\( \diamond, \\)" at the beginning of the expression for a precontext.
We identify precontexts that are equal up to reordering, so for example, \\( x \overset\rho : \alpha, y \overset\pi : \beta \\) is considered equal to \\( y \overset\pi : \beta, x \overset\rho : \alpha \\).
Formally, a precontext is given by the multiset of its entries.

We can define *scaling* of precontexts by
\\[ \pi(\diamond) = \diamond;\quad \pi(\Gamma, x \overset\rho : \alpha) = \pi(\Gamma), x \overset{\pi\rho} : \alpha \\]
If \\( 0\Gamma = 0\Gamma' \\), then \\( \Gamma \\) and \\( \Gamma' \\) contain the same variables with the same types, but possibly with different usage annotations.
In this case, we define *addition* of precontexts by
\\[ \diamond + \diamond = \diamond;\quad (\Gamma, x \overset\rho : \alpha) + (\Gamma', x \overset\pi : \alpha) = (\Gamma + \Gamma'), x \overset{\rho + \pi} : \alpha \\]
