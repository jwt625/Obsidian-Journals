
# Version 1, standing 1D
2024-10-10T22:00:26-07:00
First version geom:
![[Pasted image 20241010220040.png]]
![[Pasted image 20241010220046.png]]

## Material properties
Looking up 3D printed PLA:
- https://static.igem.org/mediawiki/2015/2/24/CamJIC-Specs-Strength.pdf
![[Pasted image 20241010220142.png]]
![[Pasted image 20241010220434.png]]
- https://en.wikipedia.org/wiki/Shear_modulus

So v = 0.27
Mg/m^3 is a weird unit... Just say 1250 kg/m3 okay??

2024-10-10T22:07:21-07:00
Added:
![[Pasted image 20241010220718.png]]
Fuck, typo, should be 3.5e9 Pa.

Mesh:
![[Pasted image 20241010221110.png]]
![[Pasted image 20241010221809.png]]
Hmm it is still too stiff and not really propagating.
Need to Move the coupling closer to the displacement? Need larger a?
Let's reduce the resonant frequency first.

2024-10-10T23:21:26-07:00
Found typo in periodic BC. Should use the same kF in the parameters instead of changing it. Could mess up the plotting for the arrayed dataset.

![[test.gif]]

2024-10-11T21:26:06-07:00
First version, already printed:
- model62_solid_slow_wave_toy_20241010_arrayed_for_geom_export.mph
![[Pasted image 20241011212651.png]]
![[Pasted image 20241011212619.png]]

Second version, standing up:
- model62_solid_slow_wave_toy_20241011_cuvy.mph
![[Pasted image 20241011212747.png]]![[Pasted image 20241011212827.png]]

Does not propagate as well.


2024-10-14T21:42:20-07:00
Tried using steel ball. Does not help much. The meander helps more:
![[Pasted image 20241014214311.png]]
![[Pasted image 20241014214244.png]]

![[Pasted image 20241014215017.png]]
![[Pasted image 20241014215038.png]]

Surprisingly not a big difference. The steel ball is not doing much. Maybe should just keep pushing down the frequency.

# Version 2, circular
2024-12-22

Did not take any notes at the time.

![[test_ring_20241222.gif]]

Tweet:
https://x.com/jwt0625/status/1870885083592478749

# Traveling wave actuators
2025-01-08T21:00:16-08:00
Planning to do three rods pushed up by a wavy wheel.
Hmm the wavy wheel might not work...
Probably should do a three phase engine like aarrangement
Yes something like this but three phase:
![[Pasted image 20250108210305.png]]
- from: https://makerworld.com/en/models/677354?from=search#profileId-605509

2025-01-08T23:26:38-08:00
Edited the shaft to be three phase using onshape:
![[Pasted image 20250108232727.png]]
Started printing about 1 h ago. Small parts not sticking well

2025-01-09T00:42:59-08:00
Seems to be working fine!
![[Pasted image 20250109004305.png]]
Really should have printed an extra stick for poking the ring resonator.
