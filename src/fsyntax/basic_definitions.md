# Basic definitions

In the following, we assume familiarity with basic set theory.

## Common sets

We define

- \\( \mathbb B = \\{ \bot, \top \\} \\) is the two-element *Boolean* set, where \\( \top \\) represents truth and \\( \bot \\) represents falsity.
- \\( \mathbb N = \\{ 0, 1, 2, \dots \\} \\) is the set of *natural numbers*, including zero.

## Binary operations

A *binary operation* \\( \otimes \\) on a set \\( X \\) is a function \\( X \times X \to X \\).
Such a binary operation is called

- *associative*, if for all \\( x, y, z \in X \\), \\( (x \otimes y) \otimes z = x \otimes (y \otimes z) \\);
- *commutative*, if for all \\( x, y \in X \\), \\( x \otimes y = y \otimes x \\).

## Semirings

A *semiring* is a set \\( R \\) with elements \\( 0, 1 \in R \\) equipped with binary operations denoted \\( + \\) and \\( \cdot \\) such that

- \\( + \\) is associative and commutative;
- \\( \cdot \\) is associative and commutative;
- *additive identity*: for all \\( x \in R \\), \\( x + 0 = x \\);
- *multiplicative identity*: for all \\( x \in R \\), \\( x \cdot 0 = 0 \\) and \\( x \cdot 1 = x \\);
- *distributivity*: for all \\( x, y, z \in R \\), \\( (x + y) \cdot z = x \cdot z + y \cdot z \\).

We write \\( x y \\) for \\( x \cdot y \\).
We also assume that semirings satisfy the following two laws.

- *positivity*: for \\( x, y \in R \\), \\( x + y = 0 \\) implies \\( x = 0 \\) and \\( y = 0 \\).
- *integrality*: for \\( x, y \in R \\), \\( xy = 0 \\) implies either \\( x = 0 \\) or \\( y = 0 \\).

We define the semiring \\( R_\omega = \\{ 0, 1, \omega \\} \\) with operations given by

- \\( 1 + 1 = \omega \\);
- \\( 1 + \omega = \omega \\);
- \\( \omega + \omega = \omega \\);
- \\( \omega \cdot \omega = \omega \\).

One can check that this structure is indeed a semiring, and satisfies positivity and integrality.

## Grammars

Precontexts and preterms are defined as syntax adhering to a particular grammar.
See *Syntax and Semantics of Quantitative Type Theory* (Atkey, 2018) for more information.
Before defining these grammars, we make a few initial definitions.

### Usages

An *extended usage*, to be interpreted as an element of the semiring \\( R_\omega \\), is of the form
\\[ \pi ::= 0 \mid 1 \mid \omega \\]
A *(binary) usage*, to be interpreted as either the element \\( 0 \\) or \\( 1 \\) in \\( R_\omega \\), is of the form
\\[ \sigma ::= 0 \mid 1 \\]
In our theory, terms all have an annotated binary usage.
Variables in contexts, however, may be annotated with \\( 0, 1 \\) or \\( \omega \\) usage.
The usage \\( \omega \\) records the fact that a resource was used multiple times; a context in which this occurs cannot typically be used in a derivation for a closed term.
There is an exception, however, in the form of *copyable types*.
The copying rule, seen later, roughly means that if we can derive a judgment under the assumption \\( x \overset\omega : \alpha \\), and \\( \alpha \\) is a copyable type, then we can derive the same consequent under the weaker assumption \\( x \overset 1 : \alpha \\).

### Names

A *name*, denoted \\( x, y, z \\), is a nonempty sequence of Unicode code points in the following Unicode character categories:

- `L*` Letters
- `N*` Numbers
- `S*` Symbols

A *qualified name*, denoted \\( Q \\), is of the form
\\[ Q ::= x \mid Q :: x \\]

### Universes

A *universe*, interpreted as an element of the set of natural numbers \\( \mathbb N \\), is of the form
\\[ u, v, w ::= \\{0, 1, 2, \dots \\} \\]
We define the operations of *maximum* and *impredicative maximum* by
\\[ \mathsf{max}(u,v) =
    \begin{cases}
        u & \text{if } u > v \\\\
        v & \text{otherwise}
    \end{cases};\quad
    \mathsf{imax}(u,v) =
    \begin{cases}
        0 & \text{if } v = 0 \\\\
        \mathsf{max}(u,v) & \text{otherwise}
    \end{cases}
\\]
