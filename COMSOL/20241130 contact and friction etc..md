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




