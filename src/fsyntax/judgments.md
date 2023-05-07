# Judgments

We distinguish preterms from terms, precontexts from contexts, and pre-environments from environments.
In order to do this, we make use of several judgments expressed in the form of natural deduction.

1. The judgment \\( \Pi\\ \mathsf{env} \\) states that the pre-environment \\( \Pi \\) is an *environment*.
2. The judgment \\( \Pi \vdash \Gamma\\ \mathsf{ctx} \\) states that the precontext \\( \Gamma \\) is a *context* in the environment \\( \Pi \\).
3. The judgment \\( \Pi \mid \Gamma \vdash a \overset\pi : \alpha \\) states that the preterm \\( a \\) is a *term* of multiplicity \\( \pi \\) and type \\( \alpha \\) in the environment \\( \Pi \\) and context \\( \Gamma \\).
4. The judgment \\( \Pi \mid \Gamma \vdash a \equiv b \\) states that the preterms \\( a \\) and \\( b \\) are *definitionally equal*.

## Admissibility

We construct our theory in such a way that the following rules are admissible.
\\[ \frac{\Pi \mid \Gamma \vdash a \overset\pi : \alpha}{\Pi \vdash \Gamma\\ \mathsf{ctx}};\quad
    \frac{\Pi \mid \Gamma \vdash a \equiv b}{\Pi \vdash \Gamma\\ \mathsf{ctx}};\quad
    \frac{\Pi \vdash \Gamma\\ \mathsf{ctx}}{\Pi\\ \mathsf{env}} \\]
\\[ \frac{\Pi \vdash \Gamma\\ \mathsf{ctx} \quad 0\Gamma = 0\Gamma'}{\Pi \vdash \Gamma'\\ \mathsf{ctx}};\quad
    \frac{\Pi \mid \Gamma \vdash a \overset\pi : \alpha}{\Pi \mid 0\Gamma \vdash a \overset 0 : \alpha} \\]

## Contexts

We stipulate the rules
\\[ \frac{\Pi\\ \mathsf{env}}{\Pi \vdash \diamond\\ \mathsf{ctx}} \\]
