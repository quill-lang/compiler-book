# Functional typing rules

We present typing rules for each of the functional expression types.

## Local variables

We can obtain a local variable from a context.
\\[ \frac{\Pi \vdash 0\Gamma, x \overset\sigma : \alpha \\ \mathsf{ctx}}{\Pi \mid 0\Gamma, x \overset\sigma : \alpha \vdash x \overset\sigma : \alpha} \\]
The use of \\( 0\Gamma \\) implies that the resources in the context are not used in the construction of the term \\( x \\).

## Applications

\\[ \frac{\Pi \mid \Gamma_1 \vdash a \overset{\sigma_1} : (x \overset{\sigma_2} : \alpha) \to \beta \quad \Pi \mid \Gamma_2 \vdash b \overset{\sigma_1 \sigma_2} : \alpha \quad 0\Gamma_1 = 0\Gamma_2}{\Pi \mid \Gamma_1 + \Gamma_2 \vdash a\\ b \overset{\sigma_1} : \beta[b/x]} \\]

> Note that unless \\( \sigma_1 = \sigma_2 = 1 \\), we used no resources to construct \\( b \\), due to the "zero needs nothing" rule.
> (TODO: Describe this rule.)
> The context in the conclusion of this rule can therefore be reduced from the standard QTT form of \\( \Gamma_1 + \sigma_2 \Gamma_2 \\) to just \\( \Gamma_1 + \Gamma_2 \\).
> This observation simplifies our presentation of the function application rule somewhat.
> In quantitative type theory more generally, the usage \\( \sigma_2 \\) could be any element of an arbitrary semiring \\( R \\), so the same modification cannot be made there.

We describe the interpretations of each instance of this rule.

- \\( \sigma_1 = 0, \sigma_2 = 0 \\).
  We consume and produce no resources.
  This is a function application rule that ignores usage labels.
- \\( \sigma_1 = 0, \sigma_2 = 1 \\).
  The function expects that its parameter is a resource.
  But since the function is erased, the computation is not executed at runtime, so the resource is not needed.
  The conclusion of the judgment is therefore \\( \Pi \mid \Gamma_1 + 0\Gamma_2 \vdash a\\ b \overset 0 : \beta[b/x] \\).
- \\( \sigma_1 = 1, \sigma_2 = 0 \\).
  In this case, the function is a resource, and its argument is erased.
  We can evaluate the function on an erased argument.
- \\( \sigma_1 = 1, \sigma_2 = 1 \\).
  This is the standard function application rule from linear logic.
  The function and its argument are used exactly once.

## Abstractions

\\[ \frac{\Pi \mid \Gamma, x \overset{\sigma_1\sigma_2} : \alpha \vdash b \overset{\sigma_1} : \beta}{\Pi \mid \Gamma \vdash \lambda(x \overset{\sigma_2} : \alpha).\\,b \overset{\sigma_1} : (x \overset{\sigma_2} : \alpha) \to \beta} \\]

## Function types

\\[ \frac{\Pi \mid 0\Gamma \vdash \alpha \overset 0 : \mathsf{Sort}\\, u \quad \Pi \mid 0\Gamma, x \overset 0 : \alpha \vdash \beta \overset 0 : \mathsf{Sort}\\, v}{\Pi \mid 0\Gamma \vdash (x \overset \sigma : \alpha) \to \beta \overset 0 : \mathsf{Sort}\\, \mathsf{imax}(u, v)} \\]

## Let expressions

\\[ \frac{\Pi \mid \Gamma_1 \vdash a \overset{\sigma_1\sigma_2} : \alpha \quad \Pi \mid \Gamma_2, x \overset{\sigma_1\sigma_2} : \alpha \vdash b \overset{\sigma_2} : \beta \quad 0\Gamma_1 = 0\Gamma_2}{\Pi \mid \Gamma_1 + \Gamma_2 \vdash \mathsf{let}_{\sigma_1\sigma_2}\\ x = a;\\, b \overset{\sigma_2} : \beta} \\]

This rule admits three forms.

- \\( \sigma_1 \\) arbitrary, \\( \sigma_2 = 0 \\).
  The ambient context is erased, so the let-binding is also erased.
- \\( \sigma_1 = 0, \sigma_2 = 1 \\).
  The ambient context exists at runtime, but we are constructing an erased value.
- \\( \sigma_1 = 1, \sigma_2 = 1 \\).
  The standard \\( \mathsf{let} \\) rule from linear logic.

## Sort expressions

\\[ \frac{\Pi \vdash 0\Gamma\\ \mathsf{ctx}}{\Pi \mid 0\Gamma \vdash \mathsf{Sort}\\ u \overset 0 : \mathsf{Sort}\\ (u + 1)} \\]
