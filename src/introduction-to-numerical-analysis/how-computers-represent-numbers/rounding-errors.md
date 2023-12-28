# Rounding Errors

As commented in the previous section, computers can only represent a limited amount of numbers. This means that when we try to represent a number that is not exactly representable in the computer, we must round it to the nearest representable number. This is called a *rounding error*.

In the example we had before, we saw that the number \\(0.1\\) is not exactly representable in the `f32` format.
This is because \\(0.1 = 1/10\\) cannot be represented in the form of an integer coefficient times a power of 2, which is what the floating point format requires.

## Arithmetic Errors

We will now talk about rounding errors in floating point *arithmetic*. We will see that, in general, the result of a floating point operation is not exactly representable in the floating point format, and therefore we store the result as a rounded number, which obviously incurs in a rounding error.

### Addition

```rust
let x: f32 = 0.1;
let y: f32 = 0.2;
let z: f32 = x + y;
//show 20 digits
println!("z = {:.20}", z);
```

As we can see, the result of the addition is not exactly \\(0.3\\), but a number that is very close to it.

### Subtraction

```rust
let x: f32 = 0.1;
let y: f32 = 0.2;
let z: f32 = y - x;
//show 20 digits
println!("x = {:.20}", x);
println!("z = {:.20}", z);

// let's make a comparison with x
if z == x {
    println!("z == x");
} else {
    println!("z != x");
}
```

As we can see, the result of the subtraction `0.2 - 0.1` *does* equal the floating point number `0.1` (which, as you might remember, is not exactly \\(0.1\\), but a number that is very close to it).

### Multiplication

```rust
let x: f32 = 0.1;
let y: f32 = 0.2;
let z: f32 = x * y;
//show 20 digits
println!("z = {:.20}", z);
```

### Division

```rust
let x: f32 = 0.1;
let y: f32 = 0.2;
let z: f32 = x / y;
//show 20 digits
println!("z = {:.20}", z);
```

## Consequences of Rounding Errors

Because of rounding errors, we cannot expect that some mathematical properties of the arithmetic operations hold in floating point arithmetic.
For example:

### Associativity of Addition and Multiplication

The *associative* property of addition and multiplication is a fundamental property of these operations on the real numbers, and it states that,
for all \\(a, b, c \in \mathbb{R}\\), we have:

\\[
a + (b + c) = (a + b) + c
\\]

and

\\[
a \cdot (b \cdot c) = (a \cdot b) \cdot c
\\]

We will give examples of floating point numbers that do not satisfy these properties. It is important
to note that these are *carefully constructed* examples, and that they are not representative of the general case.
But they do illustrate the fact that we cannot expect these properties to hold in floating point arithmetic.

**Associativity of Addition** \\(a + (b + c) = (a + b) + c\\) does not hold, in general, for floating point numbers. Let's see an example:

```rust
let a: f32 =  1.0000001;
let b: f32 = -1.0;
let c: f32 = -0.0000001;
let s1 = a + (b + c);
let s2 = (a + b) + c;
println!("s1 = a + (b + c) = {:.20}", s1);
println!("s2 = (a + b) + c = {:.20}", s2);

if s1 == s2 {
    println!("s1 == s2");
} else {
    println!("s1 != s2");
}
```

**Associativity of Multiplication** \\(a \cdot (b \cdot c) = (a \cdot b) \cdot c\\) also doesn't hold, in general. Let's see an example:

```rust
let a: f32 = 1.0e32;
let b: f32 = 7.0;
let c: f32 = 1.0e-32;
let p1 = a * (b * c);
let p2 = (a * b) * c;
println!("p1 = a * (b * c) = {:.20}", p1);
println!("p2 = (a * b) * c = {:.20}", p2);

if p1 == p2 {
    println!("p1 == p2");
} else {
    println!("p1 != p2");
}
```
