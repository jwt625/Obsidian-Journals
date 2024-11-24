2024-11-23T13:31:24-08:00
Reading this paper:
https://pubs.aip.org/aapt/ajp/article/92/12/945/3321339/Why-is-there-no-Poisson-spot-in-a-solar-eclipse
Shrinked 10x but still hard with beam envelop:
![[Pasted image 20241123133323.png]]

2024-11-23T13:57:11-08:00
Ok this size worked the best:
![[Pasted image 20241123135718.png]]

![[Pasted image 20241123135743.png]]

Power:
![[Pasted image 20241123135821.png]]

2x larger gem:
![[Pasted image 20241123140024.png]]


2024-11-23T14:02:44-08:00

Comparing ewbe and ewfd:

ewbe:
![[Pasted image 20241123140249.png]]

ewfd: (5x smaller geom and 2x denser mesh):
- still not fine enough:
![[Pasted image 20241123140519.png]]

Ok let's go 2x smaller.
- wait actually wrong boundary condition you idiot...
ewbe:
![[Pasted image 20241123141234.png]]
- DOF = 143 k, soltime = 2 s
ewfd:
![[Pasted image 20241123141310.png]]
- DOF = 1.0M, soltime = 10 s.


2024-11-23T14:22:41-08:00
Trying to reproduce this one from the paper:
![[Pasted image 20241123142248.png]]
I'm not getting any peak:
![[Pasted image 20241123142640.png]]


How big the spot should be:
![[Pasted image 20241123152714.png]]
![[Pasted image 20241123152637.png]]

2024-11-23T15:37:59-08:00
Realized the input has to be a ring...
2024-11-23T16:10:21-08:00
Nope changing it to a ring does not help.

Found another nice ref:
- https://www.lighttrans.com/use-cases/application/observation-of-the-poisson-spot.html
![[Pasted image 20241123161100.png]]
![[Pasted image 20241123163628.png]]
Theirs:
![[Pasted image 20241123161119.png]]
Mine using the same params:
![[Pasted image 20241123161141.png]]
Actually wavelength is 1 um. Switching to 532 nm.
![[Pasted image 20241123161223.png]]
Plotting E^2:
![[Pasted image 20241123161446.png]]
Ok maybe it is the beam size. Doubling it.
Still no good:
![[Pasted image 20241123163211.png]]

2024-11-23T17:01:30-08:00
Fuck yeah, got it using axial symmetric 2D sim:

![[Pasted image 20241123170151.png]]
![[Pasted image 20241123170206.png]]









