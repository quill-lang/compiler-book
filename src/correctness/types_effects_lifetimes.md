# Types, effects and lifetimes

## Lifetimes

A _region_ or _lifetime_ \\( \`a \\) is a set, which is defined to be the exact time it takes to perform some computation. The entire program's runtime is the region \\( \`\Omega \\). All regions defined in a program are implicitly subsets of \\( \`\Omega \\). A lifetime is often used to define a time period in which some action is permitted to be performed. Most commonly, this is used for borrowing data: the Quill code `&'a T` means "a borrow of some data of type `T` which may be dereferenced during the time period `'a` (without undefined behaviour)". Note that this is not time in the "wall clock" sense, but time in the sense of regions of code, or (in the case of the current Quill compiler implementation) a block of lines of MIR.

Two lifetimes \\( \`a, \`b \\) can be related by a partial order \\( <: \\) where we say \\( \`a <: \`b \\) if for all temporal instants \\( t \in \`b \\), we have \\( t \in \`a \\). We say that \\( \`a \\) _outlives_ \\( \`b \\), since whenever \\( \`b \\) is valid, \\( \`a \\) is valid. In other words, \\( \`a <: \`b \iff \`a \supseteq \`b \\).

The set-theoretic operator \\( \cup \\) may be applied to lifetimes. This allows us to create a lifetime which outlives both its operands.

Note that the lifetime \\( \`\Omega \\) outlives all others, and hence has the property that \\( \forall \`a . \`\Omega \cup \`a = \`\Omega\\).

## Primitive types

We will begin by rigorously defining a _type_ in Quill using (in part) type theory, building from System F. By convention, we will use lowercase Greek letters for names of types, and capital Greek letters for sets of types.

A type is defined to be some symbol \\( \tau \\), along with a lifetime \\( \`a \\) during which that type is defined to be _valid_. The exact properties a type has while it is "valid" are explored later.

We define the primitive types as follows.

- The symbol \\( \top \\) represents the unit type.
- Given \\( n \in \\{ 1, 2, \dots, 128 \\} \\), the symbol \\( \alpha_n \\) (short for _αριθμός_) represents the type of integers representable with \\( n \\) bits in two's complement binary representation. The set of such \\( \alpha_n \\) types is written \\( A \\) (capital \\( \alpha \\)).
- Similarly, given \\( n \in \\{ 16, 32, 64, 128 \\} \\), \\( \delta_n \\) (short for _δεκαδικός_) represents the type of floating-point numbers representable with \\( n \\) bits, as defined in the IEEE-754-2008 specification for `binary<n>`. The set of such \\( \delta_n \\) types is written \\( \Delta \\).

We now define the set of primitive types \\( \Pi = \\{ \top \\} \cup A \cup \Delta \\). All primitive types are defined to be valid during \\( \`\Omega \\), that is, for the entire program's execution.

## Function types

We will use the notion of an effect intuitively in this section, and it will be defined rigorously in the subsequent section once we have defined function types.

Given types \\( \sigma, \tau \\), a set of effects \\( !E \\), and a lifetime \\( \`a \\), the function type \\( \sigma \overset{!E\ \`a}\to \tau \\) is the type of functions whose domain is \\( \sigma \\) and whose codomain is \\( \tau \\), which may have effects \\( !E \\), and which is only valid during the lifetime \\( \`a \\). This behaviour will be specified formally later.

Likewise, we define the "unique function" type \\( \sigma \overset{!E\ \`a}\leadsto \tau \\) in the same way, with the restriction that the function is not copyable (which will be rigorously defined later).

The symbols \\( \to \\) and \\( \leadsto \\) are taken to be right-associative.

## Effects

An effect \\( !e \\) is a type \\( \epsilon \\) with lifetime \\( \`a \\), possibly with some set of parent effects \\( !F \\), together with a collection of functions which, when provided enough arguments, have the type \\( \epsilon \overset{!F\ \`b}\leadsto \epsilon \\), where \\( \`b <: \`a \\). More formally, the functions associated with an effect have types of the form

\\[ \sigma_1 \leadsto \sigma_2 \leadsto \dots \leadsto \epsilon \overset{!F\ \`b}\leadsto \epsilon \\]
