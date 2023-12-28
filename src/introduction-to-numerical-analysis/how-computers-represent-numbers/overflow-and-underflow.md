# Overflow and Underflow

## Integer overflow

What happens when you try to add 1 to the largest possible `u8` value, i.e. `255`?
In C, using the equivalent `uint8_t` type, the result is overflow with *two's complement wrapping*, which means that the result is truncated to
the value `255 + 1` modulo 256, i.e. `0`. For signed integers, the result is, in principle, [undefined behavior](https://www.gnu.org/software/c-intro-and-ref/manual/html_node/Signed-Overflow.html).

But in Rust, the result depends if you're compiling in debug or release mode.
In debug mode, the compiler includes checks for integer overflow, and panics
if it happens. In release mode, the compiler does not include such checks,
and instead wraps around the value, so the result in the example above is also `0`. This is the same behavior as C.
[The integer overflow](https://doc.rust-lang.org/book/ch03-02-data-types.html#integer-overflow)
section of the Rust book has more details on this.

> #### Underflow
> *Underflow* is the opposite of overflow, and happens when you try to subtract 1 from the smallest possible value of a given integer type.
> In Rust, underflow is also checked in debug mode, and wraps around in release mode.

In Rust, relying on integer overflow wrapping is considered an *error*.
Instead, you should *explicitly* state what you want to happen in the case of overflow.
As explained in the [integer overflow](https://doc.rust-lang.org/book/ch03-02-data-types.html#integer-overflow)
section of the Rust book, there are several methods for integer arithmetic that
the standard library provides, which allow you to specify what you want to happen. These include:

- [`wrapping_*`](https://doc.rust-lang.org/std/primitive.u8.html#method.wrapping_add): wraps around the value instead of overflowing
- [`checked_*`](https://doc.rust-lang.org/std/primitive.u8.html#method.checked_add): returns `None` if overflow happens
- [`saturating_*`](https://doc.rust-lang.org/std/primitive.u8.html#method.saturating_add): saturates at the value's minimum or maximum instead of overflowing
- [`overflowing_*`](https://doc.rust-lang.org/std/primitive.u8.html#method.overflowing_add): returns a tuple with the result and a boolean indicating if overflow happened

## Floating-point overflow

Floating-point overflow happens when the result of an operation is too large to be represented by the floating-point type.
TODO