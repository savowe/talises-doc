# Generating a wave-function
------------------------
## The gen_psi_0 utility program
TALISES comes with only two executable programs, one of which is gen_psi_0.
While the main program does the heavy-duty time propataion of the wave-function, gen_psi_0 is generating the initial wave-function.  
The gen_psi_0 program requires an XML file as input in order to generate a binary data file containing the wave-functions information.  
In the next few sections we generate some exemplary wave-functions.

### Generating a 1D wave-function

The XML file needs to include the following tags  

- `<DIM>`: number of spatial dimensions
- `<FILENAME>`: name of the generated file
- `<PSI_REAL_1D>`: equation describing the real part of \\(\Psi\\) (in the case of 1D)
- `<PSI_IMAG_1D>`: equation describing the imaginary part of \\(\Psi\\)
- `<N>`: the wave-function gets normalized to \\( \int \mathrm{d}\vec{r} |\Psi|^2 = N\\)
- `<NX>`: number of sample points. This value requires [special attention](#importance-of-sampling-frequency).
- `<XMIN>` and `<XMAX>`: bounds of position basis.

These tags are encapsulated in the `<SIMULATION>` parrent. A wave function of Gaussian appearance with initial momentum \\(\hbar k\\) can be generated with
```
<SIMULATION>
  <DIM>1</DIM> 
  <FILENAME>0.000_1.bin</FILENAME>
  <PSI_REAL_1D>exp( -0.25*((x-x_0)/sigma_x)^2 )*cos(k*x)</PSI_REAL_1D>
  <PSI_IMAG_1D>exp( -0.25*((x-x_0)/sigma_x)^2 )*sin(k*x)</PSI_IMAG_1D>
  <NX>256</NX>
  <XMIN>-10</XMIN>
  <XMAX>10</XMAX>
  <CONSTANTS>
    <N>1</N>
	<k>2</k>
    <x_0>0</x_0>
    <sigma_x>1</sigma_x>
  </CONSTANTS>
</SIMULATION>
```
Within the `<CONSTANTS>` one can define numeric values for constants used in the `<PSI_****_1D>` tag.  
Since C++ does not have complex numbers natively, we define real and imaginary part seperately.
In this example the wave-function has a Gaussian density distribution and an initial momentum:
$$ \Psi \approx \exp \Big[-\Big(\frac{x-x_0}{2\sigma_x}\Big)^2 + i k x \Big]  $$
`<N>` is a normalization constant. The program normalizes the wave-functions, such that
\\( \int \mathrm{d}\vec{r} |\Psi|^2 = N\\).  

If we save this file as gauss.xml we can finally generate the wave-function with
````
gen_psi_0 gauss.xml
````
This creates a file named 0.000_1.bin in the current working directory. 
TALISES comes with a some simple python script readbin.py that can convert this data directly into a .mat file or import it into a numpy array in python for further analysis. The second script plotbin.py can make plot of the binary file.  
For this simply type
```
python3 plotbin.py 0.000_1.bin
```
This will create a .png file which should look like this  

![1D Gaussian wave-function plot](https://raw.githubusercontent.com/savowe/talises-doc/master/figs/1D_gaussian.png)

See our example for [simulating Rabi oscillations](/user-guide/examples/rabi_oscillations/) which uses this wave-function and transfers it into a second internal state with a oscillatory time-dependent potential.

------------------
### Generating a 2D wave-function
Following the previous example it is very straightfoward to extend the XML file to generate a wave-function in two dimensions.
This time we create a 2D Gaussian wave-function centered at \\(x_0=-5\\) and \\(y_0=-5\\).  
````
<SIMULATION>
  <DIM>2</DIM> 
  <FILENAME>0.000_1.bin</FILENAME>
  <PSI_REAL_2D>exp( -0.25*((x-x_0)/sigma_x)^2 )*exp( -0.25*((y-y_0)/sigma_y)^2 )</PSI_REAL_2D>
  <PSI_IMAG_2D> 0 </PSI_IMAG_2D>
  <CONSTANTS>
    <N>1</N>
    <x_0>-5</x_0>
    <y_0>-5</y_0>
    <sigma_x>1</sigma_x>
    <sigma_y>1</sigma_y>
  </CONSTANTS>
    <NX>256</NX>
    <NY>256</NY>
    <XMIN>-10</XMIN><XMAX>10</XMAX>
    <YMIN>-10</YMIN><YMAX>10</YMAX>
</SIMULATION>
````
If you plot the resulting binary file using the plotbin.py script you should see something like this  

![1D Gaussian wave-function plot](https://raw.githubusercontent.com/savowe/talises-doc/master/figs/2D_gaussian.png)  

In our example [Momentum transfer in 2D](/user-guide/examples/rabi_oscillations/) you can learn how to use oscillatory time and position dependent potentials (aka light) to move this wave packet into the upper right corner.  

You can be much more creative with you wave-functions, for example
```
1/2*(sign(x--5)+sign(-x+3))*abs(sin(1/8*(x^2+y^2))*sin(2*(x+y)))*exp(-0.25*(y/3)^2)
```
will give you

![1D Gaussian wave-function plot](https://raw.githubusercontent.com/savowe/talises-doc/master/figs/arbitrary_wave_function.png)  

### Build-in functions

The muparser libary allows for following functions  

<table>
	<tbody><tr>
	  <td><b>Name</b></td>  <td><b>Argc.</b></td>  <td><b>Explanation</b></td>
	</tr>
	<tr><td><code>sin</code></td>   <td class="centered">1</td>     <td>sine function</td></tr>
	<tr><td><code>cos</code></td>   <td class="centered">1</td>     <td>cosine function</td></tr>
	<tr><td><code>tan</code></td>   <td class="centered">1</td>     <td>tangens function</td></tr>
	<tr><td><code>asin</code></td>  <td class="centered">1</td>     <td>arcus sine function</td></tr>
	<tr><td><code>acos</code></td>  <td class="centered">1</td>     <td>arcus cosine function</td></tr>
	<tr><td><code>atan</code></td>  <td class="centered">1</td>     <td>arcus tangens function</td></tr>
	<tr><td><code>sinh</code></td>  <td class="centered">1</td>     <td>hyperbolic sine function</td></tr>
	<tr><td><code>cosh</code></td>  <td class="centered">1</td>     <td>hyperbolic cosine</td></tr>
	<tr><td><code>tanh</code></td>  <td class="centered">1</td>     <td>hyperbolic tangens function</td></tr>
	<tr><td><code>asinh</code></td> <td class="centered">1</td>     <td>hyperbolic arcus sine function</td></tr>
	<tr><td><code>acosh</code></td> <td class="centered">1</td>     <td>hyperbolic arcus tangens function</td></tr>
	<tr><td><code>atanh</code></td> <td class="centered">1</td>     <td>hyperbolic arcur tangens function</td></tr>
	<tr><td><code>log2</code></td>  <td class="centered">1</td>     <td>logarithm to the base 2</td></tr>
	<tr><td><code>log10</code></td> <td class="centered">1</td>     <td>logarithm to the base 10</td></tr>
	<tr><td><code>log</code></td>   <td class="centered">1</td>     <td>logarithm to base e (2.71828...)</td></tr>
	<tr><td><code>ln</code></td>    <td class="centered">1</td>     <td>logarithm to base e (2.71828...)</td></tr>
	<tr><td><code>exp</code></td>   <td class="centered">1</td>     <td>e raised to the power of x</td></tr>
	<tr><td><code>sqrt</code></td>  <td class="centered">1</td>     <td>square root of a value</td></tr>
	<tr><td><code>sign</code></td>  <td class="centered">1</td>     <td>sign function -1 if x&lt;0; 1 if x&gt;0</td></tr>
	<tr><td><code>rint</code></td>  <td class="centered">1</td>     <td>round to nearest integer</td></tr>
	<tr><td><code>abs</code></td>   <td class="centered">1</td>     <td>absolute value</td></tr>
	<tr><td><code>min</code></td>   <td class="centered">var.</td>  <td>min of all arguments</td></tr>
	<tr><td><code>max</code></td>   <td class="centered">var.</td>  <td>max of all arguments</td></tr>
	<tr><td><code>sum</code></td>   <td class="centered">var.</td>  <td>sum of all arguments</td></tr>
	<tr><td><code>avg</code></td>   <td class="centered">var.</td>  <td>mean value of all arguments</td></tr>
</tbody>
</table>
------------------
### Importance of sampling frequency
The split-step Fourier method uses position and momentum space to calculate the wave-function's time-propagation. 
Therefore, one hast to keep in mind that the momentum state of the wave-function can only be correctly accounted for
if the space-grid [has enough sampling points](https://en.wikipedia.org/wiki/Nyquist_frequency).  
This means that if \\(\Delta x\\) is the whole position space consisting of \\(N\\) samples, 
we have a sampling rate of \\(\delta x = \Delta x / N\\). If we Fourier transform any wave-form, 
we can safely represent momentum states within \\(\Delta k = 2\pi / \delta x\\). 
The sampling rate in momentum space is \\(\delta k = 2\pi / \Delta x\\).
