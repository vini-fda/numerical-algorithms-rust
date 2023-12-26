# How computers represent numbers

## The binary number system

Computers represent numbers using the [binary number system](https://en.wikipedia.org/wiki/Binary_number). This means that all numbers are represented using only two digits: 0 and 1. For example, the number 42 is represented as `101010` in binary.

Mathematically, we can represent a non-negative integer \\(x\\) in base \\(b \in \mathbb{N}\\) using the following formula:

\\[
x = \sum_{i=0}^{n-1} a_i b^i
\\]

where \\(a_i \in \\{0, 1, \dots, b - 1\\}\\) is the \\(i\\) th digit of the number, and \\(n\\) is the number of digits in the number. For example, the number 42 in base 10 is represented as:

\\[
42 = 4 \times 10^1 + 2 \times 10^0
\\]

In binary, the number 42 is represented as:

\\[
42 = 1 \times 2^5 + 0 \times 2^4 + 1 \times 2^3 + 0 \times 2^2 + 1 \times 2^1 + 0 \times 2^0 = 101010
\\]

This is called the [positional notation](https://en.wikipedia.org/wiki/Positional_notation) of a number, because the position of each digit determines its value. For example, the first digit from the right is multiplied by \\(b^0\\), the second digit is multiplied by \\(b^1\\), and so on.

This is probably not new to you, but once we introduce negative numbers to complete the integers \\(\mathbb{Z}\\), fractions to complete the rational numbers \\(\mathbb{Q}\\), and irrational numbers to complete the real numbers \\(\mathbb{R}\\), things get a little more complicated, and we need to make decisions with fundamental consequences and trade-offs.

For example, do we want arbitrary precision? if so, we may need to grow our numbers indefinitely. If we want to represent fractions exactly, we can use a tuple of integers \\((a, b)\\), with \\(b \neq 0\\) to represent the fraction \\(\frac{a}{b}\\). We will soon see, though, that this is not a good idea, because it is not possible to represent all fractions exactly using a finite number of bits, and performing calculations with these fractions might make the numbers grow indefinitely, which is not good for performance.

It is often the case that we want speed, and therefore we need to sacrifice precision. For example, we can use a fixed number of bits to represent a number, but this means that we can only represent a finite amount of numbers, and that we will lose precision when performing calculations.

## Limitations of computer memory

Unlike in mathematics, where we can represent however many numbers we want with *infinite* precision, computers have a limited amount of memory, and therefore can only represent up to a finite quantity of numbers, and every number must have finite precision.

To put this mathematically, when we have \\(n\\) bits of memory, by the multiplicative principle, we can represent up to \\(2^n\\) numbers.
Let's represent the bit sequence by \\(b_0 b_1 \dots b_{n-1}\\), where \\(b_i \in \\{0, 1\\}\\) is the \\(i\\) th bit of the number.
Then, we can create a bijection between the set of bit sequences \\(B\\) to *any* other set \\(S\\) (be it of numbers or otherwise), as long as \\(|S| = 2^n\\).

For example, using 2 bits of memory, we can represent up to \\(2^2 = 4\\) elements:

\\[
\begin{align}
00 &\leftrightarrow 33 \\\\
01 &\leftrightarrow -2/3 \\\\
10 &\leftrightarrow \text{cat} \\\\
11 &\leftrightarrow \pi
\end{align}
\\]

As represented above, *any* set of \\(2^n\\) elements can be represented using \\(n\\) bits of memory. But arbitrarily choosing
this set is not a good idea for us, because we want to leverage the binary representation of numbers to perform calculations on them.

In Rust, when a numeric type `T` has \\(n\\) bits of memory, it can represent up to \\(2^n\\) numbers. For example,
the `i32` type has 32 bits of memory, and therefore can represent up to \\(2^{32}\\) numbers. The `f32` type also
has 32 bits of memory, but it represents floating point numbers, and therefore can represent up to \\(2^{32}\\) *floating point numbers*.
As you see, the number of bits is the same, but the set of numbers \\(S\\) that we want to represent is different.

Not only that, but modern computer architectures have a memory hierarchy, which means that some types of memory are faster than others. For example, registers are the fastest type of memory, followed by cache, then RAM, and finally disk.

- **Registers** are the fastest type of memory because they are located directly on the CPU, and are therefore the fastest to access. However, registers are also the most expensive type of memory, and therefore there are only a few of them.
- **Cache** is the next fastest type of memory, and there is more of it than registers, but it is still expensive.
- **RAM** is the next fastest type of memory, and there is more of it than cache, but it is still expensive.
- Finally, **disk** is the slowest type of memory, but there is a lot of it, and it is cheap.

This hierarchy can be represented by the following diagram:

```text
registers > cache > RAM > disk
```

In general, processes running on a computer will use registers to perform operations on the CPU, use the cache to store data that is frequently accessed, and will use RAM to store data that is not frequently accessed or that cannot fit in the cache. The disk is used to store long-term data, such as files, and is rarely used for calculations (unless an application is memory-bound).

As a concrete example, in the [x86-64 instruction set architecture](https://en.wikipedia.org/wiki/X86-64), there are 16 general purpose registers, each of which can store 64 bits of data.
To store a single 64-bit floating point number, we can load that number into a register, and then perform operations on it.

In Rust, we can use primitive data types to represent numbers. For example, we can use the `i32` type to represent a 32-bit integer, or the `f64` type to represent a 64-bit floating point number.

```rust
let x: i32 = 42;
let y: f64 = 3.14;
```

These numbers can fit into a single register, and therefore can be processed very quickly. For example, when you perform an addition operation on two 32-bit integers, the CPU will load the two numbers into registers, perform the addition, and then store the result back into a register. Let's see this in action:

```rust
pub fn add(x: i32, y: i32) -> i32 {
    x + y
}
```

The generated x86-64 assembly for this function is:

```x86asm
add:
    push    rax
    add     edi, esi
    mov     dword ptr [rsp + 4], edi
    seto    al
    test    al, 1
    jne     .LBB1_2
    mov     eax, dword ptr [rsp + 4]
    pop     rcx
    ret
; *other parts are ommitted*
```

There's a lot of noise there, but the important part is this:

```x86asm
add     edi, esi
```

This is the actual addition instruction, and it is performed by the CPU on two registers: `edi` and `esi`. The result is then stored back into `edi`.
Then Rust performs some additional operations to check for overflow, and then returns the result.

But what about bigger numbers? For example, what if we want to represent the number \\(2^{100}\\)? This number is too big to fit into a single register, and therefore we need to develop a data structure that can represent arbitrarily large numbers. This is called a [big integer](https://en.wikipedia.org/wiki/Arbitrary-precision_arithmetic#Arbitrary-precision_integers). Solutions like this are often called *software implementations* of numbers because they are implemented in software, and not in hardware. For now, we will not discuss software implementations of numbers, but we will discuss them in more detail in a later chapter. (TODO)

The main takeaway is this: we prefer using the [primitive data types](https://doc.rust-lang.org/book/ch03-02-data-types.html) because they directly map to hardware (i.e. CPU) implementations of numbers, and therefore operations on them are performed in a very fast and reliable fashion. However, they are limited in size, which very often will create problems like overflow, underflow, and loss of precision.
