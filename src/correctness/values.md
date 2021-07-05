# Values of types

This section defines what values a given type can have. Each type \\( \tau \in \mathcal T \\) is associated with a set of values \\( V_\tau \\). We write \\( e \colon \tau \\) if \\( e \in V_\tau \\). Each value \\( e \\) is required to have only one type.

## Primitive types

The unit type \\( \top \\) has one value, \\( \varnothing \\).

The integral primitive types \\( \alpha_n \\) have values \\( \\{ \dots, (-1, n), (0, n), (1, n), (2, n), \dots \\} \\) where the largest value is \\( 2^{n-1} - 1 \\) and the smallest is \\( -2^{n-1} \\). The \\( n \\) is a tag to ensure that `1i32` and `1i64` are considered different.

The floating point primitive types have a set of values as defined in the IEEE-754-2008 specification, tagged with their specific type so that each value belongs to exactly one type. For instance, `1.0f32` and `1.0f64` are different since one has type \\( \delta_{32} \\) and the other has type \\( \delta_{64} \\).

## Product types

The product type \\( \tau_1 \times \dots \times \tau_n \\) has value set \\( V_{\tau_1} \times \dots \times V_{\tau_n} \\). For instance, the type \\( \alpha_1 \times \alpha_2 \\) has value set

\\[ \\{ ((0, 1), (0, 2)), ((1, 1), (0, 2)), ((0, 1), (1, 2)), ((1, 1), (1, 2)), \\\\ ((0, 1), (2, 2)), ((1, 1), (2, 2)), ((0, 1), (3, 2)), ((1, 1), (3, 2)) \\} \\]

## Sum types

The sum type \\( \tau = \tau_1 + \dots + \tau_n \\) has value set

\\[ \\{ (e, 1, \tau) \colon e \in V_{\tau_1} \\} \cup \dots \cup \\{ (e, n, \tau) \colon e \in V_{\tau_n} \\} \\]

This representation stores the specific enum value, and the variant that it came from. It further guarantees that each value \\( e \\) has only one type, by storing the specific type in the third entry of the tuple.

## Type constructors

Any type constructor \\( \tau \\) has a type of the form \\( \ast \to \mathcal T \\) where \\( \ast \\) is one of \\( \mathcal T, \mathcal E, \mathcal L \\). Their values are simply a pair \\( (\dagger, \sigma) \\), where \\( \dagger \in \ast \\), and \\( \sigma \in \mathcal T \\).
