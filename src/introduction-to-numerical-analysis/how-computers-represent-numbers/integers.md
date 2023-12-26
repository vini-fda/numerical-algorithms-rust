# Integers

## Unsigned Integers

As briefly discussed in the [previous section](README.md), unsigned integers are represented using the [unsigned binary number system](https://en.wikipedia.org/wiki/Binary_number#Unsigned_binary_numbers). In Rust, we can use the `u8`, `u16`, `u32`, `u64`, and `u128` types to represent unsigned integers with 8, 16, 32, 64, and 128 bits respectively.

## Signed Integers

Signed integers are represented using the [two's complement](https://en.wikipedia.org/wiki/Two%27s_complement) representation. In Rust, we can use the `i8`, `i16`, `i32`, `i64`, and `i128` types to represent signed integers with 8, 16, 32, 64, and 128 bits respectively.

To represent negative numbers, we use the most significant bit (MSB) as a sign bit. If the MSB is 0, then the number is positive. If the MSB is 1, then the number is negative. This can be understood by using the positional notation, and noting that in two's complement the MSB is the only negative digit. Thus, for \\(n\\) bits, we have:

\\[
    x = -a_{n-1} 2^{n-1} + \sum_{i=0}^{n-2} a_i 2^i
\\]

The remaining bits are used to shift the number up. For example, the number -42 is represented as `11010110` in binary using 8 bits. The MSB is 1, which means that the number is negative (i.e. shifted down by \\(2^7 = 128\\)). The remaining bits are `1010110`, which is the binary representation of \\(2 + 4 + 16 + 64 = 86\\). We then add \\(-1 \times 128 + 86 = -42\\). Or, in the positional notation:

\\[
    -42 = -1 \times 2^7 + 1 \times 2^6 + 1 \times 2^4 + 1 \times 2^2 + 1 \times 2^1 = -128 + 64 + 16 + 4 + 2
\\]

Let's see this in action (you can run the code by pressing the play button <i class="fa fa-play"></i>):

```rust
let x: i8 = -42;
println!("{:b}", x); // prints "11010110"
```

Rust also provides the `usize` and `isize` types, which are architecture-dependent and represent the size of a pointer. For example, on a 64-bit architecture, the `usize` type is equivalent to `u64`, and the `isize` type is equivalent to `i64`. They are also often used to represent the size of a collection, or the index of an element in a collection:

```rust
let word = "hello";
let n = word.len(); // n is of type usize
let values = [1, 2, 3, 4, 5];
let first = values[0]; // first is of type i32
let m = values.len(); // m is of type usize
for i in 0..m {
    println!("{}", values[i]);
}
```

As a simple comparison, the following table shows the range of values that can be represented by some of the integer types:

| Type | Minimum value | Maximum value |
| ---- | ------------- | ------------- |
| `u8` | 0 | 255 |
| `i8` | -128 | 127 |
| `u16` | 0 | 65535 |
| `i16` | -32768 | 32767 |
| `u32` | 0 | 4294967295 |
| `i32` | -2147483648 | 2147483647 |
