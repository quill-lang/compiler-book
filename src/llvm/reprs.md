# Representations

## Introduction

A _representation_ of a type is a way of encoding a value of that type into binary. A representation is defined recursively.

- The empty set \\( \varnothing \\) is a representation.
- The 'integer representation' \\( i_n \\) for \\( n \in \mathbb N, n > 0 \\) is a representation.
- The 'pointer representation' \\( p \\) is a representation.
- The product type \\( (R_1 \times R_2 \times \dots \times R_n) \\) is a representation, where the \\( R_i \\) are representations. This represents a tuple; its values are of type \\( R_1, R_2, \dots, R_n \\).
- The sum type \\( (R_1 + R_2 + \dots + R_n) \\) is a representation, where the \\( R_i \\) are representations. This represents a tagged union; its value may be of type \\( R_1, R_2, \dots, R_n \\).

We define the symbol \\( \rho \\) to give the representation of any Quill type. For instance, we define \\( \rho(\texttt{Unit}) = \varnothing \\). This section of the book essentially defines the mechanics of \\( \rho \\).

## Values of representations

A representation \\( R \\) is defined on a set of values \\( V \\). We say a value \\( v \in V \\) has the representation \\( \rho(v) \\). Note the distinction between the representation of a type and the representation of a value.

\\[ V = \\{ 0, 1, 2, 3, 4, 5, 6, 7 \\} \\]

## Zero size

We say that a representation has _zero size_ if requires no bits of data to encode it in binary.

- \\( \varnothing \\) has zero size.
- \\( p \\) has non-zero size.
- \\( i_n \\) has non-zero size.
- (TODO: this is inaccurate with the new definition of a representation) The set \\( \\{ (n_1, R_1), (n_2, R_2), \dots, (n_k, R_k) \\} \\) has zero size if and only if all \\( R_i \\) have zero size.

In the compiler, if a type's representation has zero size, we sometimes say that the type has _no representation_.

## LLVM conversion

To convert a representation to an LLVM type, we first consider if the representation has zero size. If this is true, there exists no LLVM type to represent this object. So the variable must be completely deleted, along with any accesses from and assignments to it.

If it has a size, we construct an LLVM type as follows:

- The representation of \\( i_n \\) is `i<n>`.
- The representation of \\( p \\) is `i8*`, since _a priori_ we do not know the type of the value behind the pointer.
