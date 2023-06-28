# Borrowing typing rules

We assume \\( \Pi \\) contains the definition \\( ¬ : \mathsf{Prop} \to \mathsf{Prop} \\).

For each \\( A : \mathsf{Sort}\\ u \\), we assume \\( \Pi \\) contains the type \\( =_A \\).

- The global parameters are \\( A : \mathsf{Sort}\\ u \\) and \\( a : A \\).
- There is one index parameter, \\( b : A \\).
- There is one variant, named \\( \mathsf{refl} \\), with no fields, and index parameter assignment \\( a \\).

## Reference types

\\[ \frac{\Pi \mid 0\Gamma \vdash \alpha \overset 0 : \mathsf{Sort}\\ u}{\Pi \mid 0\Gamma \vdash \mathsf{ref}\\ \alpha \overset 0 : \mathsf{Sort}\\ u} \\]

## Dereference expressions

\\[ \frac{\Pi \mid 0\Gamma \vdash a \overset 0 : \mathsf{ref}\\ \alpha}{\Pi \mid 0\Gamma \vdash *a \overset 0 : \alpha} \\]

## Loan expressions

\\[ \frac{\Pi \mid \Gamma, x \overset 0 : \alpha, y \overset 1 : \mathsf{ref}\\ \alpha, z \overset 0 : x =_{\mathsf{ref}\\ \alpha} *y \vdash b \overset 1 : \beta}{\Pi \mid \Gamma, x \overset 1 : \alpha \vdash \mathsf{loan}\\ x\\ \mathsf{as}\\ y\\ \mathsf{with}\\ z;\\; b \overset 1 : \beta} \\]

We add the additional constraint that, in every code path in \\( b \\), there is a unique \\( \mathsf{take}\\ x \\) expression, not enclosed in any abstraction.

## Take expressions

\\[ \frac{\Pi \mid \Gamma, x \overset 1 : \alpha \vdash b \overset 1 : \beta \quad \Pi \mid 0\Gamma, x \overset 0 : \alpha \vdash a_1 \overset 0 : ¬(x\\ \mathsf{in}\\ y_1) \\ \dots \\ \Pi \mid 0\Gamma, x \overset 0 : \alpha \vdash a_n \overset 0 : ¬(x\\ \mathsf{in}\\ y_n)}{\Pi \mid \Gamma, x \overset 0 : \alpha \vdash \mathsf{take}\\ x\\ \mathsf{with}\\ y_1 \mapsto a_1, \dots, y_n \mapsto a_n;\\; b \overset 1 : \beta} \\]

We add the constraint that this \\( \mathsf{take} \\) expression occurs in the body of a \\( \mathsf{loan}\\ x \\) expression.
We also stipulate that the names \\( y_1, \dots, y_n \\) correspond to the variables bound between the loan and take expressions.

## In expressions

\\[ \frac{\Pi \mid 0\Gamma \vdash a \overset 0 : \mathsf{ref}\\ \alpha \quad \Pi \mid 0\Gamma \vdash b \overset 0 : \beta}{\Pi \mid 0\Gamma \vdash a\\ \mathsf{in}\\ b \overset 0 : \mathsf{Prop}} \\]
