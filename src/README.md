# Preface

## NOTE: THIS BOOK IS STILL A DRAFT

## About the author

Hi! I'm Vinícius Freitas de Almeida, an engineering student from Brazil.
I'm passionate about programming, math, and science in general.
As you'd expect from someone who's writing a book about *Numerical algorithms in Rust*, I'm also a *big* fan of the Rust programming language, and I've been using
it as my primary language for the past 3 years or so.

I'm writing this book during my free time, and I hope you enjoy it!
If you have any questions, suggestions, or corrections, feel free to open an issue or a pull request on the [GitHub repository for this book](https://github.com/vini-fda/numerical-algorithms-rust). Also, be sure to star the repository if you like the book!

## Why I'm writing this book

I'm writing this book because:

- I want to deepen my own understanding about numerical algorithms
- I think there's a *serious* lack of resources on numerical algorithms that are written with Rust in mind (as a matter of fact, there's somewhat of a lack of freely available, high-quality online resources on such algorithms in the first place)
- As a more programming-oriented mind, I think a lot of undergraduate/graduate numerical analysis courses are either too superficial (by abstracting away implementation details) or too theoretical (by focusing too much on the math), and I want to provide a more unified approach to the subject, which links the theory to the practice, but always grounded in the programming aspect of it.

Crucially, I think that Rust is the *perfect* language for this kind of book, for many reasons that we'll get to in the next section,
but they all boil down to the fact that Rust has learned from the mistakes of other languages, and thus addresses many of the challenges and pitfalls commonly associated with systems programming (looking at you, C and C++!).

Also, I want *more* people to write scientific, numerical, and high-performance code in Rust, and I hope that this book can help with that.

> #### Rust's numerical computing ecosystem
> As a PSA: if you are looking for crates that implement numerical algorithms, check out the [science/mathematics section of the Rust Cookbook](https://rust-lang-nursery.github.io/rust-cookbook/science/mathematics.html).
> Common libraries include ndarray, nalgebra, num, and num-traits.
> Also, there's the [Are we learning yet?](https://www.arewelearningyet.com/) website, which provides
> an assessment of the state of the Rust ecosystem for machine learning,
> which includes a list of many crates related to
> ML and scientific computing in Rust.
