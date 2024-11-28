
2024-11-27T22:51:34-08:00
This is inspired by the cross section of an atom is lambda^2/4pi (or 2pi), much larger than the atom itself.

Going to build a small dipole antenna with capacitive pads and lumped inductor.

# Electrostatic sim for the capacitance

Start with electrostatic capacitance simulation, could probably use the fancy method. What is it called again?
- yes, electrostatic, boundary elements

2024-11-27T22:58:36-08:00
Yes, only took 2 seconds:
![[Pasted image 20241127225905.png]]
![[Pasted image 20241127225917.png]]
![[Pasted image 20241127225842.png]]
Terminal charge is 1.23e-14, i.e., C = 12.3 fF.
How much inductance do we need to hit 1 GHz? Maybe should use larger pads.
- need 100 nH to get 4.54 GHz
- impedance Z = 2851 Ohms. Pretty high.
Actually let's do 2x the width.
Got 19.6 fF.

# Build the RCS sim model

2024-11-27T23:04:06-08:00
Start by modifying the rcs_sphere model.






