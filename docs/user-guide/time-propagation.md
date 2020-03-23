# Propagate the wave-function
The heart of TALISES lies in the ability to propagate a wave-function 
\\( \hat{U} |\Psi(r,t_0)\rangle = |\Psi(r,t)\rangle\\) in accordance to the Schrödinger equation
<div>$$i\hbar\frac{\partial}{\partial t} |\Psi(r,t)\rangle 
= \Big( -\frac{\hbar^2}{2m} \nabla^2 + \hat{V} (\Psi, r, t) \Big)  |\Psi(r,t)\rangle .$$</div>
The wave-function includes internal and external degrees of freedom and you can easily
implement your own position-, time-dependent or nonlinear potential \\( \hat{V} (\Psi, r, t) \\).

```
<SIMULATION>
  <DIM>1</DIM>
  <INTERNAL_DIM>3</INTERNAL_DIM>
  <FILENAME>0.000_1.bin</FILENAME>
  <FILENAME_2>0.000_2.bin</FILENAME_2>
  <FILENAME_3>0.000_2.bin</FILENAME_3>
  <CONSTANTS>
    <b>0</b>
  </CONSTANTS>
  <VCONSTANTS>
    <Alpha_1>0.000365368,0.000365368,0.000365368, 0.000365368</Alpha_1>
  </VCONSTANTS>
  <SEQUENCE>
    <interact Nk="500" dt="0.02" output_freq="each" pn_freq="each" rabi_output_freq="none"
    H_11_real="0" H_11_imag="0" 		H_12_real="0" H_12_imag="0"
                                        H_22_real="0" H_22_imag="0"     >50</interact> 
    <freeprop Nk="1000" dt="2" output_freq="each" pn_freq="each"
        H_11_real="0" H_11_imag="0" 	H_22_real="0" H_22_imag="0"     2000</freeprop>
  </SEQUENCE>
</SIMULATION>

```

TODO:  
- implement nonlinear potential in additional sequence. real and imaginary part of individual internal levels.  
- change H to V in code  
- remove alpha and exchange it by L, T and m.