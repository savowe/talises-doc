# TALISES Documentation
-----------------
[TALISES](https://github.com/savowe/talises) (This Ain't a LInear Schrödinger Equation Solver) is an easy-to-use C++ implementation of the Split-Step Fourier Method, for numeric calculation of a wave function's time-propagation under the Schrödinger equation.
It's code is based on the [ATUS-2 package](https://github.com/GPNUM/atus2) programm developed at ZARM (Center of Applied Space Technology and Microgravity, University of Bremen).
### Features
- Calculation of a wave-function's time propagation under a (non)linear Schrödinger equation
- the wave-function \\(\Psi\\) may include an arbitrary number of internal and external degrees of freedom
- simple implementation of own Hamiltonians with time and position dependent potentials \\(V(\Psi,r,t)\\)
- speed of C++ and the FFTW and GSL libaries with multithreading