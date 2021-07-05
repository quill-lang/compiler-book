# Composite types

## Product types

Given types \\( \tau_1, \tau_2, \dots, \tau_n \\), their product \\( \tau_1 \times \tau_2 \times \dots \times \tau_n \\) is a type, and is valid when all of the \\( \tau_i \\) are valid (the _meet_ of the lifetimes of the \\( \tau_i \\)). In a later section, we will define computation rules that allow us to construct and deconstruct values of this product type.

## Sum types

Given types \\( \tau_1, \tau_2, \dots, \tau_n \\), their sum \\( \tau_1 + \tau_2 + \dots + \tau_n \\) is a type, and is valid when all of the \\( \tau_i \\) are valid. This represents a quill `enum`, a tagged union. Again, we will define computation rules that allow us to construct and deconstruct values of this sum type.

## Type constructors

Type constructors allow us to parametrise using a type, effect or lifetime. We can make a type constructor out of any function, product, or sum type. A type constructor is written using the syntax `for [T]` in Quill, and \\( \forall \tau \\) here.

## Aspects

Aspects are not complicated; we can represent an impl of an aspect as simply a data type.
