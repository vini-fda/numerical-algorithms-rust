# Introduction

Welcome to the **Numerical Algorithms in Rust** book! This book is intended for students and programmers who have a solid background in calculus and linear algebra.
It is also assumed that the reader has some experience with a programming language, preferably Rust. To learn more about Rust, please refer to the [Rust Book](https://doc.rust-lang.org/book/).

## Why are numerical algorithms important?

Numerical algorithms are used to solve mathematical problems that are difficult or impossible to solve analytically. They are used in many fields of science and engineering, such as Physics, Chemistry, Biology, Economics, and Computer Science. For example, numerical algorithms are used to:

- Simulate physical systems
- Solve difficult optimization problems
- Analyze large datasets
- Train machine learning models
- Solve differential equations

Throughout this book, we will give real world examples of how numerical algorithms are used in practice. Examples include:

- Simulating a pendulum
- Understanding heat transfer in a metal rod
- Understand growth of a population
- Finding the best price for a product given some constraints
- Analyzing an audio signal to remove noise
- Controlling a robot with a PID controller
- Solving the Schrödinger equation for a particle in a box

## What will I learn from this book?

This book will teach you how to implement numerical algorithms in Rust. You will learn how to:

- Solve nonlinear equations
- Solve systems of linear equations
- Interpolate and approximate functions
- Differentiate and integrate functions
- Solve ordinary differential equations (ODEs)
- Solve partial differential equations (PDEs)
- Optimize functions
- Perform fast Fourier transforms
- Apply numerical algorithms to real world problems

Most importantly, this book will give you the tools to understand how numerical algorithms work, and how to implement them in a way that is fast, safe, and easy to maintain. You can then specialize and learn more advanced topics and algorithms in your field of interest.

You will also learn about fundamental limitations in how we represent numbers in a computer, and how to avoid common pitfalls when implementing numerical algorithms.
Crucially, that includes understanding how to avoid [floating point errors](https://en.wikipedia.org/wiki/Floating-point_arithmetic#Accuracy_problems) and [rounding errors](https://en.wikipedia.org/wiki/Round-off_error), understanding the difference between [stability](https://en.wikipedia.org/wiki/Numerical_stability) and [convergence](https://en.wikipedia.org/wiki/Convergence_(mathematics)) and also understanding that computers are fundamentally discrete machines, and that we need to use [discretization](https://en.wikipedia.org/wiki/Discretization) to approximate and solve continuous problems.

## Why specifically Rust?

There are many excellent books on numerical methods and algorithms, but most of them are either written with low-level languages in mind, such as C, C++, or Fortran ---
and give you a more fundamental understanding of how these algorithms operate on computer hardware --- or they are written with high-level languages in mind, such as Python, Julia, or MATLAB --- which give you a more high-level understanding of how these algorithms work, at the cost of taking away some control over the implementation details.

Rust, on the other hand, is a modern programming language that allows the programmer to choose if they want to operate at a low-level or high-level,
by providing full control over memory management, and also providing abstractions that maintain invariant guarantees at compile-time, such as the borrow checker and the type system.

Rust's rich type system allows us to write code that is easy to read and maintain, and also adopts a mix of principles from procedural, functional, and object-oriented programming paradigms, such as:

- Using algebraic data types (ADTs) to model data
- [Making illegal states unrepresentable](https://fsharpforfunandprofit.com/posts/designing-with-types-making-illegal-states-unrepresentable/)
- Using pattern matching to handle different cases
- Using generics to write reusable code
- Using traits to define behavior
- Using closures to write higher-order functions
- Using iterators to process collections

...and much more.

Rust also has a great package manager called [Cargo](https://doc.rust-lang.org/cargo/) that makes it easy to manage dependencies and build projects. It also has a built-in testing framework that allows us to write unit tests and integration tests. In addition, Rust has a great community that is very welcoming and helpful.

Finally, Rust has great interoperability with other languages, such as Python, C, and C++. This means that we can easily call Rust code from Python, for example, and vice-versa. This is very useful when we want to use Rust's performance and safety guarantees in a Python project, for example.

All of those characteristics make Rust a great language to implement numerical algorithms, because these algorithms often require
a deep understanding of the underlying memory operations, as well as CPU instructions, and Rust's low-level capabilities
allow us to write code that can be mapped to such underlying operations in a relatively transparent way.

Also, having high-level abstractions allows us to create sophisticated APIs that are easy to use and maintain, and also allows us to
numerical code that is ergonomic, generic, and reusable. This means that, if you are *fluent* in Rust, reading
something such as a type signature for a function will often make it obvious what the function does, and what it expects as input.
For example, a derivative function that takes a function `f` as input and returns another function as output can be written as:

```rust
fn derivative(f: impl Fn(f64) -> f64) -> impl Fn(f64) -> f64 {
    // implementation...
}
```
