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
- `<GUESS_1D>`: equation describing \\(\Psi\\) (in the case of 1D)
- `<NX>`: number of sample points. This value requires [special attention](#importance-of-sampling-frequency).
- `<XMIN>` and `<XMAX>`: bounds of position basis.

These tags are encapsulated in the `<SIMULATION>` parrent. A wave function of Gaussian appearance can be generated with
```
<SIMULATION>
  <DIM>1</DIM> 
  <FILENAME>0.000_1.bin</FILENAME>
  <GUESS_1D>exp( -0.25*((x-x_0)/sigma_x)^2 )</GUESS_1D>
  <NX>256</NX>
  <XMIN>-10</XMIN>
  <XMAX>10</XMAX>
  <CONSTANTS>
    <N>1</N>
    <x_0>0</x_0>
    <sigma_x>1</sigma_x>
  </CONSTANTS>
</SIMULATION>
```
Within the `<CONSTANTS>` one can define numeric values for constants used in the `<GUESS_1D>` tag. 
`<N>` is a normalization constant, the wave-function is always multiplied with.  

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
  <GUESS_2D>exp( -0.25*((x-x_0)/sigma_x)^2 )*exp( -0.25*((y-y_0)/sigma_y)^2 )</GUESS_2D>
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

------------------
### Importance of sampling frequency
The split-step Fourier method uses position and momentum space to calculate the wave-function's time-propagation. 
Therefore, one hast to keep in mind that the momentum state of the wave-function can only be correctly accounted for
if the space-grid [has enough sampling points](https://en.wikipedia.org/wiki/Nyquist_frequency).  
This means that if \\(\Delta x\\) is the whole position space consisting of \\(N\\) samples, 
we have a sampling rate of \\(\delta x = \Delta x / N\\). If we Fourier transform any wave-form, 
we can safely represent momentum states within \\(\Delta k = 2\pi / \delta x\\). 
The sampling rate in momentum space is \\(\delta k = 2\pi / \Delta x\\).
------------------