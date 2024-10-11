2024-10-10T22:00:26-07:00
First version geom:
![[Pasted image 20241010220040.png]]
![[Pasted image 20241010220046.png]]

# Material properties
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



