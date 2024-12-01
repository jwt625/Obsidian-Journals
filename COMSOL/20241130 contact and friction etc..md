2024-11-30T15:53:56-08:00
Checking out the examples:

# Self-contact of a loaded spring
![[Pasted image 20241130155430.png]]

2024-11-30T16:10:21-08:00
Damn took 13 min to run.
Before contact:
![[Pasted image 20241130161028.png]]
After:
![[Pasted image 20241130161051.png]]
The convergence also looks different before and after contact:
![[Pasted image 20241130161242.png]]

How does it make this plot showing results from four parameters at the same time??
![[Pasted image 20241130161153.png]]
From inherit style?
![[Pasted image 20241130161524.png]]
ok also the `plot array`:
![[Pasted image 20241130161700.png]]
![[Pasted image 20241130161719.png]]

There is quite a few options inside the contact node:
![[Pasted image 20241130161834.png]]

This global equation is also very interesting:
![[Pasted image 20241130162229.png]]
![[Pasted image 20241130162302.png]]
- the `aveop1` function is taking over the end surface.
The contact pair also needs to be setup under component -> definition.

Going to check another model about sliding.
## Sliding wedge
2024-11-30T20:01:52-08:00
![[Pasted image 20241130200147.png]]

The solver looks annoying. Different solvers are doing different contact method and solution method. What are the differences?
![[Pasted image 20241130200345.png]]

Ok I think I should just start building one.

## Two cylinders in a box

![[Pasted image 20241130202815.png]]
![[Pasted image 20241130202831.png]]
Five pairs of contacts:
![[Pasted image 20241130202851.png]]


2024-11-30T20:27:57-08:00
It is not fucking converging:
![[Pasted image 20241130202803.png]]

Going to try the contact from the loaded spring contact model:
![[Pasted image 20241130203132.png]]

Still does not converge, however it does when I put the global equation and variable force back in.
What is this quantum tunneling:
![[Pasted image 20241130203558.png]]
![[Pasted image 20241130204216.png]]
![[Pasted image 20241130204225.png]]

2024-11-30T20:53:37-08:00
Piece of shit:
![[Pasted image 20241130205340.png]]



