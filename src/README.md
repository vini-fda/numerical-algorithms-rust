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
- Solve ordinary differential equations
- Solve partial differential equations
- Optimize functions
- Perform fast Fourier transforms
- Apply numerical algorithms to real world problems

Most importantly, this book will give you the tools to understand how numerical algorithms work, and how to implement them in a way that is fast, safe, and easy to maintain. You can then specialize and learn more advanced topics and algorithms in your field of interest.

You will also learn about fundamental limitations in how we represent numbers in a computer, and how to avoid common pitfalls when implementing numerical algorithms.
Crucially, that includes understanding how to avoid [floating point errors](https://en.wikipedia.org/wiki/Floating-point_arithmetic#Accuracy_problems) and [rounding errors](https://en.wikipedia.org/wiki/Round-off_error), understanding the difference between [stability](https://en.wikipedia.org/wiki/Numerical_stability) and [convergence](https://en.wikipedia.org/wiki/Convergence_(mathematics)) and also understanding that computers are fundamentally discrete machines, and that we need to use [discretization](https://en.wikipedia.org/wiki/Discretization) to approximate and solve continuous problems.

## Why specifically Rust?

There are many excellent books on numerical methods and algorithms, but most of them are written in C, C++, or Fortran. Rust is a modern programming language that is fast, safe, and expressive. Its rich type system allows us to write code that is easy to read and maintain, and also adopts a mix of principles from procedural, functional, and object-oriented programming paradigms, such as:

- Using algebraic data types (ADTs) to model data
- [Making illegal states unrepresentable](https://fsharpforfunandprofit.com/posts/designing-with-types-making-illegal-states-unrepresentable/)
- Using pattern matching to handle different cases
- Using generics to write reusable code
- Using traits to define behavior
- Using closures to write higher-order functions
- Using iterators to process collections

...and much more. Rust also has a great package manager called [Cargo](https://doc.rust-lang.org/cargo/) that makes it easy to manage dependencies and build projects. It also has a built-in testing framework that allows us to write unit tests and integration tests. In addition, Rust has a great community that is very welcoming and helpful.

Finally, Rust is a great language for learning numerical algorithms because it is a low-level language that gives us full control over memory management, and allows us to write code that is as fast as C or C++ without sacrificing memory safety.
