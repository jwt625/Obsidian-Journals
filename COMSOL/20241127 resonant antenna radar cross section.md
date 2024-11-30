
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

# Debug FF plot with control sim

2024-11-28T22:00:06-08:00
Control sim without the antenna:
- still the same far field pattern.
- Good news, some settings are wrong.
![[Pasted image 20241128220033.png]]
Also the background field is still a bit curved/bent:
![[Pasted image 20241128220543.png]]

Checked the radar cross section. Should be checking the relative electric field: `abs(comp1.emw.relEz)`.

So this is just from the mesh:
![[Pasted image 20241128220711.png]]
Going to turn off the extra pads and the finer mesh.
2024-11-28T22:12:07-08:00
Turned off the extra mesh. There is still something in relEz:
![[Pasted image 20241128221223.png]]
- also turned off fixed color range
Background field:
![[Pasted image 20241128221308.png]]
- `emw.Ebz`
Ez:
![[Pasted image 20241128221336.png]]

Back to the control sim with the finer mesh around the antenna:
relEz:
![[Pasted image 20241128221515.png]]
Ez:
![[Pasted image 20241128221534.png]]
Ok so good news the mesh is not doing anything weird.

Fuck it let's build a model from scratch.
Start: 2024-11-28T22:18:30-08:00
![[Pasted image 20241128222718.png]]
This looks freaking fine.
Ok going to add the antenna.

2024-11-28T22:37:36-08:00
Hmm it looks good at a single frequency but wrong as soon as the frequency is swept:
![[Pasted image 20241128223751.png]]
![[Pasted image 20241128223806.png]]

The radar cross section eval is:
- `10*log10(at2(r0*cos(phi),r0*sin(phi),emw.bRCS2D))`

Also going to run 4e9 Hz to see:
![[Pasted image 20241128224036.png]]
Why tf is this different from f0??
Running 4.459e9:
![[Pasted image 20241128224248.png]]
Nonsense...
4.4597e9:
Wow okay:
![[Pasted image 20241128224349.png]]

The 3D sphere RCS sim:
![[Pasted image 20241128224930.png]]
emw.bRCS3D is just |Efar|^2.


Freq sweep, basically looks the same as before:
![[Pasted image 20241128225731.png]]

relE, manual range to be -1 to 1, 3.5 GHz:
![[Pasted image 20241128225827.png]]
Oh fuck because the background field is not updated properly...
Fuck me.

2024-11-28T23:18:01-08:00
Fixed:
![[Pasted image 20241128231803.png]]
![[Pasted image 20241128231818.png]]
Going to do a finer sweep around 3.5 GHz.
- 2 min 23 s
- This 2019 macbook is killing me.
![[Pasted image 20241128233248.png]]
![[Pasted image 20241128233427.png]]

Finer from 3.2 to 3.5 GHz.
![[Pasted image 20241128234100.png]]

2024-11-28T23:45:23-08:00
Running 3.34 ~ 3.36 GHz.
Ohh sweeeet:![[Pasted image 20241128235058.png]]

Field on resonant:
![[Pasted image 20241128235151.png]]
What is the Chu–Harrington limit of this shit:
- size = a ~ 1.5 mm
- lambda = 86.6 mm
- k = 2pi/lambda
- Chu-Harrington limit of Q is 776.
- Corresponding max FWHM is 4.5 MHz.

+-10 color range:
![[Pasted image 20241129001810.png]]
2024-11-29T00:18:16-08:00
Exported gif.

Efar^2 is **1e-5**:
![[Pasted image 20241129002537.png]]
- comparing to 9e-12 ~ 1e-11 when off-resonance
- 1e6 larger RCS







