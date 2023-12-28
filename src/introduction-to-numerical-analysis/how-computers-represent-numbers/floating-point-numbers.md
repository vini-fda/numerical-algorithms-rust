# Floating Point Numbers

*Floating-point numbers* are the main kind of numbers that are used in numerical computing, because (as you'll soon see)
they provide a great solution to the *extremely difficult* problem of representing a wide range of numbers with finite
memory.

You can think of these numbers as approximations to the set of real numbers \\(\mathbb{R}\\), so whenever
you calculate a function such as `y = sin(x)` you will probably use a floating point datatype to represent
both `x` and `y`.

> #### Rust floating-point datatypes
> [In Rust](https://doc.rust-lang.org/book/ch03-02-data-types.html#floating-point-types), the default type to represent a floating-point number is `f64`, a 64-bit representation
> that is often called "double precision". There's also the `f32` type, which is often called "single precision".
> We often abbreviate "floating-point number" to just "float", following C and C++'s naming for such types.

Rust, just like many other programming languages, follows the [IEEE 754 standard for floating-point arithmetic](https://ieeexplore.ieee.org/document/8766229),
which guarantees some behavior that is standardized throughout hardware vendors, such as Intel, AMD and IBM.
This section will discuss FP arithmetic in light of this standard, and we'll not discuss non-binary floating-point (i.e. with a radix that isn't 2),
as well as different encodings for FP numbers.

## The floating point format

In Rust, floats are stored according to a binary IEEE 754 format.
This format comprises:

- Finite numbers, which can be described by three integers:
  - \\(s\\), a *sign* (+1 or -1)
  - \\(c\\), a *mantissa*, significand or coefficient, with a precision of \\(p\\) bits
  - \\(e\\), an *exponent*
- Two infinities: \\(+\infty\\) and \\(-\infty\\).
- Two kinds of NaN (not-a-number): a quiet NaN (qNaN) and a signaling NaN (sNaN).

The formula to represent a finite number \\(x\\) under this format is:

\\[
    x = s \cdot c \cdot 2^e
\\]

As you can see, only a subset of the rational numbers \\(\mathbb{Q}\\) can be represented under this format. This
will have some consequences when we try to represent numbers that are not in this subset, as we'll see later.

> #### Note on the exponent
> The exponent is a signed integer. In the IEEE 754 standard, the exponent is stored as an unsigned integer, and a *bias* term is subtracted from it.
> Therefore, if it has \\(n\\) bits, the exponent can represent \\(2^n\\) values, but the range includes negative numbers because of the bias.
> For example, if \\(n = 8\\), then the exponent can represent \\(2^8 = 256\\) values, but the range is \\(-127\\) to \\(+128\\) because of the bias of 127.
>
> Therefore, in general, the range of the exponent is \\(e_{\text{min}} = -2^{n-1} + 1\\) to \\(e_{\text{max}} = +2^{n-1}\\).

### Encoding

The *encoding* of floating point numbers is the way that the format is mapped to the sequence of binary digits.
As a concrete example, the `f32` type is encoded as follows:

\\[
  \underbrace{\text{sign bit}}\_{1\text{ bit}} \quad \underbrace{\text{exponent}}\_{8\text{ bits}} \quad \underbrace{\text{mantissa}}\_{23\text{ bits}}
\\]

> #### Note on the leading bit of the mantissa
> For numbers with an exponent in the normal range (the exponent not being in the extremities \\(e_{\text{min}}\\) or \\(e_{\text{max}}\\), or equivalently, the exponent field being neither all ones nor all zeros), the leading bit of the mantissa will always be 1.
>
> Consequently, an "invisible" leading 1 can be implied rather than explicitly stored in memory, and under the standard the explicitly represented part of the significand will lie between 0 and 1.
>
> This rule is called *leading bit convention*, *implicit bit convention*, or *hidden bit convention*, and it allows the binary format to have an extra bit of precision. Pretty clever, huh?

Rust code can be used to inspect the encoding of a floating-point number:

```rust
let x: f32 = 1.125;
let bits = x.to_bits();
println!("{} is encoded as {:032b}", x, bits);
// Print s, e, c in encoded form
println!("Encoded form:");
println!(" sign bit: {}", (bits >> 31) & 1);
println!(" exponent: {:08b}", (bits >> 23) & 0xFF);
println!(" mantissa: {:023b}", bits & 0x7FFFFF);
// Print s, e, c in decoded form
let s = (bits >> 31) & 1;
let e = ((bits >> 23) & 0xFF) as i32 - 127;
let c = (bits & 0x7FFFFF) as i32;
println!("Decoded form:");
println!(" sign: {}", if s == 0 { "+1" } else { "-1" });
println!(" exponent: {}", e);
println!(" mantissa: {}", c);
```

In case you can't run the code, here's the output:

```text
1.125 is encoded as 00111111100100000000000000000000
Encoded form:
 sign bit: 0
 exponent: 01111111
 mantissa: 00100000000000000000000
Decoded form:
 sign: +1
 exponent: 0
 mantissa: 1048576
```

Let's break down the output. In the text above, we see

\\[
    \underbrace{0}\_{\text{sign bit}} \quad \underbrace{01111111}\_{\text{exponent bits}} \quad \underbrace{00100000000000000000000}\_{\text{mantissa bits}}
\\]

In the *encoded form*,

- The sign bit is 0, which means that the number is positive.
- The exponent bits represent 127, which means that the exponent is 0 (since we subtract 127 from it).
- The mantissa bits represent \\(2^{20}\\), which is equal to 1048576. Also, the exponent is in the normal range,
which means we must add the leading bit. Therefore the mantissa is \\(1 + \frac{1048576}{2^{23}}\\), or \\(1 + \frac{2^{20}}{2^{23}}\\).

In summary, the encoding of \\(1.125\\) is:

\\[
    \begin{align}
        \text{sign bit} &= 0 \implies s = +1 \\\\
        \text{exponent} &= 01111111 \implies e = 127 - 127 = 0 \\\\
        \text{mantissa} &= 00100000000000000000000 \implies c = 1 + 2^{20} \cdot 2^{-23} = 1 + 0.125 = 1.125
    \end{align}
\\]

And, as we can see, the formula for the floating point format holds:

\\[
    \begin{align}
        x &= s \cdot c \cdot 2^e \\\\
        &= (+1) \cdot 1.125 \cdot 2^0 \\\\
        &= 1.125
    \end{align}
\\]

> #### Rounding
> The keen reader might have noticed that we picked a number that is exactly representable in the `f32` format.
> But what about numbers that are not exactly representable, such as \\(0.1\\)? The IEEE 754 standard defines
> rounding rules for these cases, which we'll not discuss in much detail here.
> 
> The key takeaway is that when you write an expression such as `let x: f32 = 0.1;`, the number that is actually stored in memory
> is not exactly \\(0.1\\), but a number that is very close to it. This is true for all numbers
> that cannot exactly be represented in the binary floating point format.
> Let's run an example to see this in action:
>
> ```rust
> #// compute decimal string from ratio a/b
> #pub fn decimal_from_ratio(a: i32, b: i32) -> String {
> #    let mut a = a;
> #    let mut b = b;
> #    let mut result = String::new();
> #    if a < 0 {
> #        result.push('-');
> #        a = -a;
> #    }
> #    if b < 0 {
> #        result.push('-');
> #        b = -b;
> #    }
> #    result.push_str(&format!("{}", a / b));
> #    result.push('.');
> #    let mut r = a % b;
> #    while r != 0 {
> #        r *= 10;
> #        result.push_str(&format!("{}", r / b));
> #        r %= b;
> #    }
> #    result
> #}
> let x: f32 = 0.1;
> let bits = x.to_bits();
> println!("{} is encoded as {:032b}", x, bits);
> let e = ((bits >> 23) & 0xFF) as i32 - 127;
> let c = (bits & 0x7FFFFF) as i32;
> let dec = decimal_from_ratio((1 << 23) + c, 1 << (23 - e));
> println!("{} was stored in f32 format as {}", x, dec);
> ```
>
> So, for example, 0.1 is stored as 0.100000001490116119384765625 in the `f32` format. As you can see, there is an error on the order of \\(10^{-9}\\)
> because of rounding.

## Counter-intuitive consequences of the floating point format

### Zeroes are Not Always Zero

In floating-point arithmetic, you can encounter both positive zero (`+0`) and negative zero (`-0`), and they are considered equal even though they have different bit representations.

### Rounding Errors

Rounding introduces a lot of "weird" and unexpected behavior in FP arithmetic. Examples include:

- Non-associativity of addition and multiplication
- Loss of significance ("catastrophic cancellation")
- Machine epsilon and comparison issues
- Fluctuating precision at different scales

These will be discussed in more detail in the [Rounding Errors](rounding-errors.md) section.

### Infinite and NaN (Not a Number) Values

Operations like dividing by zero or taking the square root of a negative number in floating-point don't throw errors but produce special values like `Infinity` or `NaN`. Moreover, `NaN` is unique as it is not equal to itself (`NaN != NaN`). In fact, `NaN != x` for any `x`.

### Overflow and Underflow

Numbers too large to be represented result in overflow, often represented by `Infinity`. Conversely, numbers too small to be represented (closer to zero than the smallest representable number) result in underflow, leading to a loss of precision. This will be further discussed in the [Overflow and Underflow](overflow-and-underflow.md) section.

## Further reading

- The [Wikipedia page on floating-point arithmetic](https://en.wikipedia.org/wiki/Floating-point_arithmetic) is a great resource, with many links and explanations for anyone who wants to know more about both the technical aspects of floating-point arithmetic, as well as its history.
- [Rust's documentation on the std::f32 primitive](https://doc.rust-lang.org/std/primitive.f32.html) is also a great resource to learn more about the `f32` type, and how the IEEE 754 standard is implemented in Rust.
- The famous [What every computer scientist should know about floating-point arithmetic](https://dl.acm.org/doi/10.1145/103162.103163) article by David Goldberg is a must-read for anyone interested in the nitty-gritty details of the topic. For an easier introduction, you can read the [floating-point-gui.de](https://floating-point-gui.de/) website.
- [This stackoverflow answer](http://stackoverflow.com/questions/588004/is-javascripts-math-broken/588014) provides a great explanation of binary FP arithmetic.
- As always, [the Rust Book](https://doc.rust-lang.org/book/ch03-02-data-types.html#floating-point-types) provides the de-facto source of truth for Rust's numeric types, and includes proper explanations of their behavior.
