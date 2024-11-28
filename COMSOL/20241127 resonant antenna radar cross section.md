
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

2024-11-27T23:26:50-08:00
Ok built and got it running:
![[Pasted image 20241127232659.png]]
![[Pasted image 20241127232714.png]]

Probably need some loss to see the resonance? Or look at the phase and amplitude in the lumped inductor?
- sol time is 9 s, probably fast enough to do a quick freq sweep.

2024-11-27T23:36:51-08:00
Hmm weird looking average Ez between the antenna:
![[Pasted image 20241127233702.png]]

2D FF:
![[Pasted image 20241127233736.png]]
- Why is the FF not symmetric??
6.5 GHz is the strongest:
![[Pasted image 20241127233822.png]]
Zoom-in:
![[Pasted image 20241127233857.png]]

Adding |E|^2 to the average plot:
![[Pasted image 20241127234535.png]]
Ok going to sweep 3 to 4 GHz with finer step.
2024-11-27T23:52:03-08:00
Done 3-4 GHz:
![[Pasted image 20241127235122.png]]
![[Pasted image 20241127235201.png]]
However FF is monotonic:
![[Pasted image 20241127235235.png]]

2024-11-28T00:00:07-08:00
Ok found the scattered BC selection was wrong. Rerun...
- nope no change. Guess the PML is doing its job.
Ok the FF calculation selection was wrong... Rerun.
- fuck, it is fucking the same...
2024-11-28T00:10:45-08:00
- give up for now, need to sleep.








