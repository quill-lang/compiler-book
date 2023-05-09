# Definitional equality

The rules used to derive definitional equality require no resources.

## Equality as an equivalence relation

The *reflexivity* rule is
\\[ \frac{\Pi \mid 0\Gamma \vdash a \overset 0 : \alpha}{\Pi \mid 0\Gamma \vdash a \equiv a} \\]
The *symmetry* rule is
\\[ \frac{\Pi \mid 0\Gamma \vdash a \equiv b}{\Pi \mid 0\Gamma \vdash b \equiv a} \\]
The *transitivity* rule is
\\[ \frac{\Pi \mid 0\Gamma \vdash a \equiv b \quad \Pi \mid 0\Gamma \vdash b \equiv c}{\Pi \mid 0\Gamma \vdash a \equiv c} \\]

## Type conversion

Terms can be reinterpreted as a definitionally equal type.
The *type conversion* rule is
\\[ \frac{\Pi \mid \Gamma \vdash a \overset\sigma : \alpha \quad \Pi \mid 0\Gamma \vdash \alpha \equiv \beta}{\Pi \mid \Gamma \vdash a \overset\sigma : \beta} \\]
