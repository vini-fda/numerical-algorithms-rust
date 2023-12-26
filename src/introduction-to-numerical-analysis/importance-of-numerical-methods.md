# Importance of Numerical Methods

## Why are numerical algorithms important?

In a traditional calculus course, you learn how to derive formulas for integrals, ODEs, and PDEs. For example, you
learn that the integral of \\(\sin x\\) is \\(-\cos x + C\\), and that the solution to the ODE \\(\frac{dy}{dx} = y\\) is
\\(y = Ce^x\\). However, in practice, we often encounter problems that are too difficult (or impossible) to solve analytically, and
therefore we must use numerical methods to approximate the solution. *This is one of many scenarios where numerical methods
are used in practice.*

It is a common misconception to think that numerical methods are exclusively used to solve problems that are too difficult to solve analytically.
In fact, numerical methods are used all the time in practice, even when we *can* solve the problem analytically. The reason
for this is that numerical methods are often faster than analytical methods. For example, if we want to
find the root of a polynomial, we can use the [Newton-Raphson method](root-finding-algorithms/newton-raphson-method.md),
which is an iterative method that approximates the root of a function. This method is often faster than
solving the polynomial analytically.

Also, sometimes we *need* the numerical representation of the solution, and not the symbolic representation. For example,
when drawing a graph of a function, we need to evaluate the function at many points, and therefore we need to use algorithms to approximate the function up to the resolution of the screen to accurately draw the graph. Again, it is important to remember that there is no
such thing as a "continuous" function in a digital computer[^x86-cos], because computers are discrete machines.

Another big mistake is thinking that numerical methods are inherently inaccurate. In fact, numerical methods can be *arbitrarily*
accurate, in the sense that we can make the solution get as close to the true solution as we want, given enough time and memory.
Of course, you wouldn't (and can't) use a 32-bit floating point number to represent the position of a particle in a simulation
of the solar system, but that's not because numerical methods are inaccurate, it's because you are using the wrong data type
to accurately represent the position of that particle.

<!-- TODO: MOVE THIS ELSEWHERE 
Not only that, but we want these approximate solutions to be accurate, in the sense that they *actually* get as close to the
true solution as possible, given constraints such as time and memory. For example, if we want to find the root of a polynomial,
we want to be able to say to the computer "find the root of this polynomial up to 10 decimal places of accuracy". This is
called the *precision* of the solution. -->

In general, numerical methods are suitable for the following mathematical problems:

- Problems that are too difficult, impossible or computationally expensive to solve analytically
- Problems that require speed, and therefore we are willing to sacrifice precision up to a certain extent
- Applications that require a graphical or numerical (instead of symbolic) representation of the solution
- Situations where the input data is inherently imprecise, and therefore we neither need nor are able to obtain a precise solution

## Case studies

### Pendulum

Let's say you want to simulate the motion of a pendulum. Using Newton's laws of motion and gravitation, we can
derive the following differential equation:

\\[
\frac{d^2 \theta}{dt^2} = -\frac{g}{\ell} \sin \theta
\\]

where \\(\theta\\) is the angle of the pendulum, \\(g\\) is the gravitational acceleration, and \\(\ell\\) is the length
of the pendulum. This is a second-order non-linear ordinary differential equation (ODE), which means that it contains
second-order derivatives of the unknown function \\(\theta\\). This is a very difficult equation to solve
analytically, and therefore we must use numerical methods to simulate the motion of the pendulum.

### N-body problem

Let's say you want to precisely predict the trajectory of a probe going from the Earth to Jupyter. Evidently,
Newton's laws of motion and gravitation are at play in this physical system, but unlike two-body systems, the
solar system contains many bodies which interact with each other through gravity[^three-body-problem]. This means that the equations
of motion are very complex, and impossible to solve analytically. Therefore, we must use numerical methods to
simulate the system and predict the trajectory of the probe.

### Computer graphics

The field of computer graphics is a great example of how numerical methods are used in practice. For example,
when drawing a 3D scene, we need to project the 3D coordinates of the objects onto a 2D screen. This is done
using a technique called [perspective projection](https://en.wikipedia.org/wiki/3D_projection#Perspective_projection),
which is a numerical algorithm that approximates the projection of a 3D point onto a 2D screen.

### Linear algebra

Linear algebra is another field where numerical methods are used extensively. For example, when solving a system
of linear equations, we can use the [Gaussian elimination](linear-algebra/gaussian-elimination.md) algorithm to
find the solution. This algorithm is often faster than solving the system directly, and it is also more
numerically stable.

## Conclusion

These are just some of many instances where numerical methods are used in practice. In fact, numerical methods are
used all throughout science and engineering. TODO

-----

[^x86-cos]: You might say "what about the [`FCOS`](https://www.felixcloutier.com/x86/fcos) instruction in the x86 ISA?". Well, that's not a continuous function, as it implements a numerical algorithm to approximate the cosine of a number.

[^three-body-problem]: A simpler version of the problem is the [Three Body Problem](https://en.wikipedia.org/wiki/Three-body_problem), which
consists of three bodies (e.g. the Sun, the Earth, and the Moon) interacting with each other through gravity. This
problem is also impossible to solve analytically, and therefore we must use numerical methods to simulate the system.
