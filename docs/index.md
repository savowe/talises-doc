toc_depth: 1
# TALISES Documentation
-----------------
[TALISES](https://github.com/savowe/talises) (This Ain't a LInear Schrödinger Equation Solver) is an easy-to-use C++ implementation of the Split-Step Fourier Method, for numeric calculation of a wave function's time-propagation under the Schrödinger equation.

### Features
- the wave-function \\(\Psi\\) may include an arbitrary number of internal and external degrees of freedom
- simple implementation of Hamiltonians with time-, position-dependent and nonlinear potentials \\(V(\Psi,r,t)\\)
- speed of C++, the FFTW and GSL libaries and multithreading

#### Have a look at some exemplary simulations
-   [Potential barrier](/user-guide/examples/potential_barrier)
-   [Atom interferometry in 1D harmonic trap](/user-guide/examples/1D_harmonic_trap)
-   [Momentum transfer in 2D harmonic trap](/user-guide/examples/2D_harmonic_trap)
-   [Stimulated Raman transitions](/user-guide/examples/Raman_transitions)
-   [Bose-Einstein condensate collision](/user-guide/examples/BEC_scattering)