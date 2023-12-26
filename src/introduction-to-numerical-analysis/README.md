# Introduction to Numerical Analysis

**Numerical analysis** is an area of applied mathematics that studies algorithms for solving mathematical problems that are difficult or impossible to solve analytically. It also provides answers to questions such as:

- How do we know that a numerical algorithm is correct?
- What is the error of a numerical algorithm?
- If I must have an error less than \\(10^{-6}\\), how many iterations do I need to perform?

Instead of solving a problem by deriving a formula, in numerical analysis we approximate the solution using more elementary mathematical operations such as addition, multiplication, and division, often in an iterative fashion.

For example, how do we find the square root of a number? In algebra, you might just be satisfied with the analytical, *perfect* answer \\(\sqrt{2}\\). But what good is it to a scientist or engineer to know that the square root of 2 is \\(\sqrt{2}\\)? We need to know the *decimal representation* of the square root of 2, which, when truncated up to the 11th decimal place is precisely \\(1.41421356237\\). Not only that, but we need to know a reproducible, step by step *procedure* to calculate the square root of *any positive number* up to a given precision, which is what numerical analysis is all about.

"But how do we actually calculate the square root of 2 with a decimal representation?", you ask. Well, we can use a numerical algorithm called the [Babylonian method](https://en.wikipedia.org/wiki/Methods_of_computing_square_roots#Babylonian_method) to approximate the square root. The Babylonian method is an iterative algorithm that uses the following formula to approximate the square root of a number \\(a \in \mathbb{R}_+\\):

\\[
x_{n+1} = \frac{1}{2} \left( x_n + \frac{a}{x_n} \right)
\\]

where \\(x_0\\) is an initial guess, and \\(x_n\\) is the \\(n\\) th approximation of the square root of \\(a\\). We can use this formula to approximate the square root of \\(a = 2\\):

\\[
\begin{align}
x_0 &= 1 \\\\
x_1 &= \frac{1}{2} \left( x_0 + \frac{a}{x_0} \right) = \frac{1}{2} \left( 1 + \frac{2}{1} \right) = \frac{3}{2} = 1.5 \\\\
x_2 &= \frac{1}{2} \left( x_1 + \frac{a}{x_1} \right) = \frac{1}{2} \left( 3/2 + \frac{2}{3/2} \right) = \frac{17}{12} = 1.41\bar{6} \\\\
x_3 &= \frac{1}{2} \left( x_2 + \frac{a}{x_2} \right) = \frac{1}{2} \left( 17/12 + \frac{2}{17/12} \right) = \frac{577}{408} = 1.414\overline{2156862745098039} \\\\
\vdots \\\\
\sqrt{2} &= 1.4142135623730951\dots
\end{align}
\\]

Note: we use the notation \\(\overline{a_1 a_2 \dots a_n}\\) to indicate that the sequence of digits \\(a_1 a_2 \dots a_n\\) repeats infinitely. For example, \\(1.\overline{21} = 1.212121\dots\\) means that the digits \\(21\\) repeat infinitely. This is called a [repeating decimal](https://en.wikipedia.org/wiki/Repeating_decimal), and can be applied to any number system, not just decimal numbers.

It is important to understand that numerical analysis *has nothing to do with computers or programming*. It is a branch of mathematics that studies the theoretical foundations of numerical algorithms. You can reason about a numerical algorithm just using pen and paper, for example, and *prove* properties
about it, such as its convergence, stability, and error bounds. There are no computers involved --- just mathematical reasoning.

However, in practice, we use computers to implement numerical algorithms because they allow us to perform complex calculations quickly and accurately. A relatively modern computer can perform billions of operations per second, and can store billions of numbers in memory.

Let's give an example from the early 2010s: the [i7-3770k processor from Intel](https://ark.intel.com/content/www/us/en/ark/products/65523/intel-core-i7-3770k-processor-8m-cache-up-to-3-90-ghz.html), which was released in 2012, has a base clock speed of 3.5 GHz, 8 cores, with 4 operations per cycle[^ops-per-cycle-intel]. As a very rough estimate, we can use this formula to calculate the number of floating-point operations per second (flops):

\\[
\text{Operations per second} = \text{Clock speed} \times \text{Number of cores} \times \text{Operations per cycle}
\\]

This gives us a rough estimate of \\(3.5 \times 10^9 \times 8 \times 4 = 112 \times 10^9\\) operations per second, or 112 gigaflops. That means 112 *billion* operations every second! Again, this is a very rough estimate, as it grossly overestimates the number of operations per second. In reality, the number of flops is much lower, but it is still a very large number on the order of billions, which no human can match.

## Ok, but is this book about numerical analysis or numerical algorithms?

As the title implies, this book is about *numerical algorithms*, not *numerical analysis*. The difference is subtle, but important. Numerical analysis 
is a very broad field that includes topics such as [error propagation](https://en.wikipedia.org/wiki/Numerical_analysis#Generation_and_propagation_of_errors), [stability](https://en.wikipedia.org/wiki/Numerical_stability), [convergence](https://en.wikipedia.org/wiki/Convergence_(mathematics)), [complexity](https://en.wikipedia.org/wiki/Computational_complexity_theory#Continuous_complexity_theory), and [discretization](https://en.wikipedia.org/wiki/Discretization). It is a very theoretical field, and it is not the primary focus of this book.

But understanding numerical analysis is important to understand numerical algorithms, because it provides the theoretical underpinnings for them, and that's why this book will cover some of the theoretical aspects of numerical analysis.
If you want to deeply understand *why* numerical algorithms work, then you should definitely read in-depth books about the topic[^numerical-analysis-books].

To put it simply, this book will focus on the practical aspects of these algorithms. This includes a hands-on approach with code snippets and examples in Rust, and advice on how to implement these algorithms correctly, as well as ways to test them and evaluate their performance. We will not delve deeply into proofs and theorems, but we will discuss the theoretical aspects of these algorithms, though only to the extent that is necessary to understand how to implement them correctly.

## What makes numerical algorithms different from other algorithms?

There are many different types of algorithms, such as sorting algorithms, graph algorithms, search algorithms, and so on. What makes numerical algorithms different from regular algorithms, and what makes them noteworthy of study, is the fact that they fundamentally deal with *approximations* to continuous (and often infinite) mathematical objects, such as real numbers, functions, and derivatives.

Therefore, the difficulty in designing numerical algorithms is that we must deal with the fact that computers are *discrete* and *finite* machines, and that we must use *discretization* to approximate continuous objects. This is a fundamental limitation of computers, and it is important to understand how to deal with it.

---------

[^ops-per-cycle-intel]: Intel actually [does not provides flops/cycle info available anymore](https://www.intel.com/content/www/us/en/support/articles/000057415/processors.html), but their [Export Compliance Metrics for Intel® Microprocessors](https://www.intel.com/content/www/us/en/support/articles/000005755/processors.html) page shares material in which they publish a [112 gflops figure](https://www.intel.com/content/dam/support/us/en/documents/processors/APP-for-Intel-Core-Processors.pdf) for peak floating-point performance for the i7-3770k processor.

[^numerical-analysis-books]: [Numerical Analysis](https://www.amazon.com/Numerical-Analysis-Richard-L-Burden/dp/1305253663) by Richard L. Burden and J. Douglas Faires, or [Numerical Analysis](http://people.cs.uchicago.edu/~ridg/newna/nalrs.pdf) by L. Ridgway Scott are often recommended, but I have not read them myself.
