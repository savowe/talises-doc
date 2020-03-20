# Handling binary data
The wave-functions used by TALISES are saved as binary data files.
The binaries do not only contain information of the wave-function amplitudes at every grid point,
but also contextual information like the time, position space size, sampling rate and more.  
The binary is comprised of a header containing this additional information and the body containing the wave-functions real and imaginary amplitude at every space point.
### File structure
The header is 1380 bytes long. Its structure is described in the `\include\my_structs.h` file.
it reads
```
struct generic_header
{
  long long nself;    // size of struct
  long long nDatatyp; // size of data type
  long long nself_and_data;
  long long nDims;
  long long nDimX;
  long long nDimY;
  long long nDimZ;
  int       bAtom;
  int       bComplex;
  double    t;
  double    xMin;
  double    xMax;
  double    yMin;
  double    yMax;
  double    zMin;
  double    zMax;
  double    dx;
  double    dy;
  double    dz;
  double    dkx;
  double    dky;
  double    dkz;
  double    dt;
  int       ks; // coordinate system
  int       fs; // 1 -> fourier space, 0 -> real space
  int       nFuture[99];
  double    dFuture[100];
};
```
Much of this information is necessary for TALISES in order to calculate the wave-functions time-propagation and some information is just useful for the user.
The binaries following the header represent the real and imaginary wave-function amplitudes, where each number is double float precision (8 Bytes).
For example, a wave-function represented by 256x256 grid points takes memory of  
<div>$$ 256\cdot 256 \cdot 2 \cdot 8 \,\mathrm{B} = 10499568 \,\mathrm{B} \approx 1.026 \,\mathrm{kB}.$$ </div>  

We can make this information more accesible for the regular user with python.

### The readbin.py script

The `\python-scripts\readbin.py` is a simple implementation to convert the binary data to dictionaries which can be further handled in python or directly saved as a .mat file.
For example, executing `python3 readbin.py 0.000_1.bin 0.000_2.bin` will save both binaries as .mat files.
If you `import readbin` in your own pyton script, you will get a function `readbin(filename)` that returns the data of *filename* in a dictionary,
which you can then further analyze e.g. plot. See `\python-scripts\plotbin.py` as an example.