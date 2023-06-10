# Environmental typing rules

For clarity, we describe the typing rule for some expressions in English rather than through mathematical symbols.

## Instantiate expressions

In the following rules, the precise syntax of the \\( \mathsf{data} \\) and \\( \mathsf{prop} \\) intro rules is shortened for brevity.

\\[ \frac{\Pi, Q = \mathsf{data}\\ u\\ \dots \vdash 0\Gamma\\ \mathsf{ctx}}{\Pi, Q = \mathsf{data}\\ u\\ \dots \mid 0\Gamma \vdash \mathsf{inst}\\ Q \overset 0 : \mathsf{Type}\\ u} \\]

This rule ascribes meaning to the act of placing a datatype in the environment, by allowing it to be used as a type in an appropriate context.
In particular, the universe \\( u \\) defines the sort in which the datatype resides.

\\[ \frac{\Pi, Q = \mathsf{prop}\\ \dots \vdash 0\Gamma\\ \mathsf{ctx}}{\Pi, Q = \mathsf{prop}\\ \dots \mid 0\Gamma \vdash \mathsf{inst}\\ Q \overset 0 : \mathsf{Prop}} \\]

This rule is like the previous one, except that propositions always live in the sort \\( \mathsf{Prop} \\).

\\[ \frac{\Pi, Q = \mathsf{def}\\ a \overset\sigma : \alpha \vdash 0\Gamma\\ \mathsf{ctx}}{\Pi, Q = \mathsf{def}\\ a \overset\sigma : \alpha \mid 0\Gamma \vdash \mathsf{inst}\\ Q \overset{\sigma\sigma'} : \alpha} \\]

When instantiating a definition, we can choose to instantiate it with the usage it was defined, or erase it.

## Intro expressions

Suppose the environment \\( \Pi \\) contains
\\[ \mathsf{data}\\ u\\ (x_1 : \alpha_1), \dots, (x_r : \alpha_r) \Rightarrow y_1 \mapsto V_1; \dots; y_n \mapsto V_n \\]
assigned to the name \\( Q \\), and we wish to assign a type to the term
\\[ t ::= \mathsf{intro}\\ Q\\ a_1\\ \dots\\ a_r\\ \mathsf{variant}\\ y\\ \mathsf{with}\\ z_1 \mapsto c_1, \dots, z_m \mapsto c_m \\]
We assume that we wish to construct \\( t \\) with usage \\( \sigma \\).
First, we check that for each \\( i = 1, \dots, r \\), we have
\\[ \Pi \mid 0\Gamma \vdash a_i \overset 0 : \alpha_i[a_1/x_1, \dots, a_{i-1}/x_{i-1}] \\]
This ensures that the global parameters for the datatype are type correct.
We then check that \\( y \\) is one of the variants \\( y_1, \dots, y_n \\).
Suppose \\( y = y_i \\), and \\( V_i = z_1 \overset{\sigma_1} : \gamma_1, \dots, z_m \overset{\sigma_m} : \gamma_m \\).
We implicitly assume in this statement that the names of the fields in \\( V_i \\) agree with the arguments of the \\( \mathsf{intro} \\) expression.
Then for each \\( j = 1, \dots, m \\), we check that
\\[ \Pi \mid \Gamma \vdash c_j \overset {\sigma\sigma_j} : \gamma_j[a_1/x_1, \dots, a_n/x_n, c_1/z_1, \dots, c_{j-1}/z_{j-1}] \\]
If all of these constraints can be satisfied concurrently (using the sum of the relevant contexts \\( \Gamma \\)), we conclude
\\[ t \overset\sigma : Q\\ a_1\\ \dots\\ a_r \\]
Suppose instead that \\( \Pi \\) assigns the proposition type
\\[ \begin{aligned}
    \mathsf{prop}\\ &(x_1 : \alpha_1), \dots, (x_r : \alpha_r) \mid \beta_1, \dots, \beta_s \Rightarrow \\\\
    & y_1 \mapsto V^P_1 : b_{11}, \dots, b_{1s}; \dots; y_n \mapsto V^P_n : b_{n1}, \dots, b_{ns}
\end{aligned} \\]
to the name \\( Q \\).
We first check that all of the global parameters are correctly typed as above, and similarly ensure that \\( y = y_i \\) for some \\( i \\).
For each field \\( j \\) in the propositional variant \\( V_i^P = z_1 : \gamma_1, \dots, z_m : \gamma_m \\), we verify that
\\[ \Pi \mid 0\Gamma \vdash c_j \overset 0 : \gamma_j[a_1/x_1, \dots, a_n/x_n, c_1/z_1, \dots, c_{j-1}/z_{j-1}] \\]
Then, we conclude
\\[ t \overset 0 : Q\\ a_1\\ \dots\\ a_r\\ (b_{i1}\\ \dots\\ b_{is})[a_1/x_1, \dots, a_r/x_r, c_1/z_1, \dots, c_m/z_m] \\]

## Match expressions

<!-- TODO: How do we do Fn vs FnOnce in the current type system? Do we really need to have \to and \implies be different? I think this is the cleanest way. -->

Suppose we wish to typecheck
\\[ t ::= \mathsf{match}\\ a\\ \mathsf{return}\\ \alpha\\ \mathsf{with}\\ y_1 \mapsto b_1, \dots, y_n \mapsto b_n \\]
First, suppose \\( \Pi \\) contains
\\[ \mathsf{data}\\ u\\ (x_1 : \alpha_1), \dots, (x_r : \alpha_r) \Rightarrow y_1 \mapsto V_1; \dots; y_n \mapsto V_n \\]
assigned to the name \\( Q \\), and we check \\( a \overset \sigma : Q\\ a_1\\ \dots\\ a_r \\).
Implicitly, we are assuming that the names of the variants of \\( Q \\) match with the arguments of the \\( \mathsf{match} \\) expression.
We then check that, for some \\( u \\),
\\[ \Pi \mid 0\Gamma \vdash \alpha \overset 0 : (x \overset 0 : Q\\ a_1\\ \dots\\ a_r) \to \mathsf{Sort}\\ u \\]
We now verify that each \\( b_i \\) is well-typed.
Specifically, if \\( V_i = z_1 \overset{\sigma_1} : \gamma_1, \dots, z_m \overset{\sigma_m} : \gamma_m \\), we check that
\\[ \begin{aligned}
    \Pi \mid \Gamma \vdash\\, &b_i \overset\sigma : (z_1 \overset{\sigma_1} : \gamma_1) \to \dots \to (z_m \overset{\sigma_m} : \gamma_m) \to \\\\
    &\alpha\\ (\mathsf{intro}\\ Q\\ a_1\\ \dots\\ a_r\\ \mathsf{variant}\\ y_i\\ \mathsf{with}\\ z_1 \mapsto z_1, \dots, z_m \mapsto z_m)
\end{aligned} \\]
If all of these checks pass, we conclude
\\[ \Pi \mid \Gamma \vdash t \overset \sigma : \alpha\\ a \\]
Now, suppose \\( \Pi \\) binds the name \\( Q \\) to the proposition
\\[ \begin{aligned}
    \mathsf{prop}\\ &(x_1 : \alpha_1), \dots, (x_r : \alpha_r) \mid \beta_1, \dots, \beta_s \Rightarrow \\\\
    & y_1 \mapsto V^P_1 : b_{11}, \dots, b_{1s}; \dots; y_n \mapsto V^P_n : b_{n1}, \dots, b_{ns}
\end{aligned} \\]
In this case, we require \\( \sigma = 0 \\).
We check \\( a \overset 0 : Q\\ a_1\\ \dots\\ a_r\\ a_1'\\ \dots\\ a_s' \\).
Then, we verify that
\\[ \Pi \mid 0\Gamma \vdash \alpha \overset 0 : (x_1' \overset 0 : \beta_1) \to \dots \to (x_s' \overset 0 : \beta_s) \to (x \overset 0 : Q\\ a_1\\ \dots\\ a_r\\ x_1'\\ \dots\\ x_s') \to \mathsf{Sort}\\ u \\]
At this stage, we ensure that \\( u \\) is a valid universe to eliminate into for this particular proposition.
We perform the following sequence of checks.

- If \\( Q \\) has no variants, \\( u \\) may be any universe.
- If \\( Q \\) has at least two variants, \\( u \\) must be \\( 0 \\); we can only eliminate into \\( \mathsf{Prop} \\).
- Suppose \\( Q \\) has one variant \\( V^P \\). If every field satisfies either

  1. its type lies in \\( \mathsf{Prop} \\); or
  2. it occurs in one of the index parameters \\( b_{11}, \dots, b_{1s} \\),

  then \\( u \\) may be any universe.
  If any field fails to satisfy both requirements, \\( u \\) must be \\( 0 \\).

We now check that each \\( b_i \\) is well-typed.
Suppose we are considering the variant \\( V^P_i = (z_1 : \gamma_1, \dots, z_m : \gamma_m) : b_{i1}, \dots, b_{is} \\).
Then, we check
\\[ \begin{aligned}
    \Pi \mid \Gamma \vdash\\, &b_i \overset 0 : (z_1 \overset 0 : \gamma_1) \to \dots \to (z_m \overset 0 : \gamma_m) \to \\\\
    &\alpha\\ b_{i1}\\ \dots\\ b_{is}\\ (\mathsf{intro}\\ Q\\ a_1\\ \dots\\ a_r\\ \mathsf{variant}\\ y_i\\ \mathsf{with}\\ z_1 \mapsto z_1, \dots, z_m \mapsto z_m)
\end{aligned} \\]
We finally conclude
\\[ \Pi \mid \Gamma \vdash t \overset 0 : \alpha\\ a_1'\\ \dots\\ a_s'\\ a \\]

> TODO: Can we get away with \\( \sigma \\) usage in some cases, such as to perform casts between propositionally equal types?

## Fix expressions

Finally, suppose we wish to typecheck
\\[ \mathsf{fix}\\ (x \overset\sigma : \alpha)\\ \mathsf{return}\\ \beta\\ \mathsf{with}\\ y.\\, a \\]
First, we ensure that \\( \alpha \\) is an algebraic datatype or a propositional type.
If \\( \alpha \\) is a propositional type, we assume \\( \sigma = 0 \\).
We now check that
\\[ \Pi \mid 0\Gamma \vdash \beta \overset 0 : (x \overset 0 : \alpha) \to \mathsf{Sort}\\ u \\]
and
\\[ \Pi \mid \Gamma, x \overset\sigma : \alpha, y \overset\sigma : (x \overset\sigma : \alpha) \Rightarrow \beta\\ x \vdash a \overset\sigma : \beta\\ x \\]
Now, we check that each use of \\( y \\) in \\( a \\) occurs as a function in an application expression, where the argument is one of the bound variables from a match expression on \\( x \\), possibly with some arguments applied to it.
This restriction ensures that the fixpoint function we describe will terminate on all inputs.
