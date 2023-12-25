# How computers represent numbers

## The binary number system

Computers represent numbers using the [binary number system](https://en.wikipedia.org/wiki/Binary_number). This means that all numbers are represented using only two digits: 0 and 1. For example, the number 42 is represented as `101010` in binary.

Mathematically, we can represent a number \\(x\\) in base \\(b\\) using the following formula:

\\[
x = \sum_{i=0}^n a_i b^i
\\]

where \\(a_i\\) is the \\(i\\) th digit of the number, and \\(n\\) is the number of digits in the number. For example, the number 42 in base 10 is represented as:

\\[
42 = 4 \times 10^1 + 2 \times 10^0
\\]

In binary, the number 42 is represented as:

\\[
42 = 1 \times 2^5 + 0 \times 2^4 + 1 \times 2^3 + 0 \times 2^2 + 1 \times 2^1 + 0 \times 2^0 = 101010
\\]

This is called the [positional notation](https://en.wikipedia.org/wiki/Positional_notation) of a number, because the position of each digit determines its value. For example, the first digit from the right is multiplied by \\(b^0\\), the second digit is multiplied by \\(b^1\\), and so on.

This is probably not new to you, but once we introduce negative numbers to complete the integers \\(\mathbb{Z}\\), fractions to complete the rational numbers \\(\mathbb{Q}\\), and irrational numbers to complete the real numbers \\(\mathbb{R}\\), things get a little more complicated, and we need to make decision with fundamental consequences and trade-offs.

For example, do we want arbitrary precision? if so, we may need to grow our numbers indefinitely. If we want to represent fractions exactly, we can use a tuple of integers \\((a, b)\\), with \\(b \neq 0\\) to represent the fraction \\(\frac{a}{b}\\). We will soon see, though, that this is not a good idea, because it is not possible to represent all fractions exactly using a finite number of bits, and performing calculations with these fractions might make the numbers grow indefinitely.

It is often the case that we want speed, and therefore we need to make trade-offs between speed and precision. For example, we can use a fixed number of bits to represent a number, but this means that we can only represent a finite amount of numbers, and that we will lose precision when performing calculations.

## Limitations of computer memory

Unlike in mathematics, where we can represent numbers with arbitrary precision, computers have a limited amount of memory, and therefore can only represent up to a finite quantity of numbers. To put this mathematically, when we say that a type `T` has \\(n\\) bits, we mean that it can represent up to \\(2^n\\) different numbers. For example, the `u8` type in Rust has 8 bits, and therefore can represent up to \\(2^8 = 256\\) different numbers.

Not only that, but modern computer architectures have a memory hierarchy, which means that some types of memory are faster than others. For example, registers are the fastest type of memory, followed by cache, then RAM, and finally disk.

- Registers are the fastest type of memory because they are located directly on the CPU, and are therefore the fastest to access. However, registers are also the most expensive type of memory, and therefore there are only a few of them.
- Cache is the next fastest type of memory, and there is more of it than registers, but it is still expensive.
- RAM is the next fastest type of memory, and there is more of it than cache, but it is still expensive.
- Finally, disk is the slowest type of memory, but there is a lot of it, and it is cheap.

This hierarchy can be represented by the following diagram:

```text
registers > cache > RAM > disk
```

In general, processes running on a computer will use registers and cache to store data that is frequently accessed, and will use RAM to store data that is not frequently accessed. The disk is used to store data that is not currently being used, but may be used in the future.

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

This is the actual addition operation, and it is performed on two registers: `edi` and `esi`. The result is then stored back into `edi`.
Then Rust performs some additional operations to check for overflow, and then returns the result.

But what about bigger numbers? For example, what if we want to represent the number \\(2^{64}\\)? This number is too big to fit into a single register, and therefore we need to develop a data structure that can represent arbitrarily large numbers. This is called a [big integer](https://en.wikipedia.org/wiki/Arbitrary-precision_arithmetic#Arbitrary-precision_integers). Solutions like this are often called *software implementations* of numbers because they are implemented in software, and not in hardware. For now, we will not discuss software implementations of numbers, but we will discuss them in more detail in a later chapter. (TODO)

The main takeaway is this: we prefer using the primitive types because they directly map to hardware implementations of numbers, and therefore are very fast. However, they are limited in size, which very often will create problems like overflow, underflow, and loss of precision.
