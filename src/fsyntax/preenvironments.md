# Pre-environments

## Pre-datatypes

Feather's algebraic types are defined as follows.

A *field* is of the form
\\[ F ::= z \overset\sigma : \gamma \\]
A *variant* is of the form
\\[ V ::= \varepsilon \mid F,V \\]
A *pre-datatype* is
\\[ T ::= \mathsf{data}\\ u\\ (x_1 : \alpha_1), \dots, (x_r : \alpha_r) \Rightarrow y_1 \mapsto V_1; \dots; y_n \mapsto V_n \\]

Fields represent the objects contained within an algebraic data type.
Each field is tagged with a (binary) usage \\( \sigma \\), describing whether the field is to be stored at runtime.

The variants of a datatype are interpreted as a partition of the data type into disjoint sets.
Each variant in a datatype is tagged with its name.

A datatype has a universe level \\( u \\) in which the type lives, then a list of parameters \\( \alpha_1, \dots, \alpha_r \\), and finally a list of possible variants.
No usage information is attached to the parameters, as no resources are required for type creation.
In this way, an algebraic datatype is a "sum of product types".

## Pre-propositions

Feather's type theory features an impredicative, proof-irrelevant universe of propositions.
We separate propositions and datatypes in order to simplify runtime code; propositions can be entirely erased, so the runtime does not need to know about the additional flexibility offered by proposition types.

A *propositional field* is of the form
\\[ F^P ::= z : \gamma \\]
A *propositional variant* is of the form
\\[ V^P ::= \varepsilon \mid F^P,V^P \\]
A *pre-proposition* is
\\[ \begin{aligned}
    P ::=\mathsf{prop}\\ &(x_1 : \alpha_1), \dots, (x_r : \alpha_r) \mid \beta_1, \dots, \beta_s \Rightarrow \\\\
    & y_1 \mapsto V^P_1 : b_{11}, \dots, b_{1s}; \dots; y_n \mapsto V^P_n : b_{n1}, \dots, b_{ns}
\end{aligned} \\]

Pre-propositions are like pre-datatypes, but have more expressive flexibility.
They always have multiplicity 0.
The \\( \alpha_i \\) are called the *global parameters*, and the \\( \beta_j \\) are called the *index parameters*.
Each variant \\( v \\) specifies the index parameters \\( b_{v1} : \beta_1, \dots, b_{vs} : \beta_s \\) associated to it.

## Pre-definitions

A *pre-definition* is of the form
\\[ D ::= \mathsf{def}\\ a \overset\sigma : \alpha \\]

## Pre-environments

Pre-environments store named datatypes, propositions, and definitions.
Formally, a *pre-environment* is of the form
\\[ \Pi ::= \diamond \mid \Pi, Q = T \mid \Pi, Q = P \mid \Pi, Q = D \\]
As with precontexts, we suppress the "\\( \diamond, \\)" syntax, and we consider two pre-environments identical if they agree up to reordering.
There is no scaling operation defined on pre-environments.
