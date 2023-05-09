# Precontexts

A *precontext* is of the form
\\[ \Gamma ::= \diamond \mid \Gamma, x \overset\pi : \alpha \\]
We suppress the syntax "\\( \diamond, \\)" at the beginning of the expression for a precontext.
We identify precontexts that are equal up to reordering, so for example, \\( x \overset{\pi_1} : \alpha, y \overset{\pi_2} : \beta \\) is considered equal to \\( y \overset{\pi_2} : \beta, x \overset{\pi_1} : \alpha \\).
Formally, a precontext is given by the multiset of its entries.

We can define *scaling* of precontexts by
\\[ \pi(\diamond) = \diamond;\quad \pi(\Gamma, x \overset{\pi'} : \alpha) = \pi(\Gamma), x \overset{\pi\pi'} : \alpha \\]
If \\( 0\Gamma_1 = 0\Gamma_2 \\), then \\( \Gamma_1 \\) and \\( \Gamma_2 \\) contain the same variables with the same types, but possibly with different usage annotations.
In this case, we define *addition* of precontexts by
\\[ \diamond + \diamond = \diamond;\quad (\Gamma_1, x \overset{\pi_1} : \alpha) + (\Gamma_2, x \overset{\pi_2} : \alpha) = (\Gamma_1 + \Gamma_2), x \overset{\pi_1 + \pi_2} : \alpha \\]
