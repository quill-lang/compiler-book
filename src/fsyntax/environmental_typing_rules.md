# Environmental typing rules

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

We describe the typing rule for \\( \mathsf{intro} \\) expressions in English rather than through mathematical symbols.
Suppose the environment \\( \Pi \\) contains
\\[ \mathsf{data}\\ u\\ (x_1 : \alpha_1), \dots, (x_r : \alpha_r) \Rightarrow y_1 \mapsto V_1; \dots; y_n \mapsto V_n \\]
and we wish to assign a type to the term
\\[ t ::= \mathsf{intro}\\ Q\\ a_1\\ \dots\\ a_r\\ \mathsf{with}\\ y_1 \mapsto a_1, \dots, y_n \mapsto a_n \\]
First, we check that for each \\( i = 1, \dots, r \\), we have
\\[ \Pi \mid 0\Gamma \vdash a_i \overset 0 : \alpha_i[a_1/x_1, \dots, a_{i-1}/x_{i-1}] \\]
TODO: Choose variant in \\( \mathsf{intro} \\); check field types; check multiplicities.
