2024-08-10T23:16:35-07:00 Should have started taking notes sooner.

Plastic folding table typical specs:
![[Pasted image 20240810231707.png]]

Table top surface is typically HDPE (high-density polyethylene).


Amscope boom stand specs:
- https://amscope.com/products/saw?tw_source=google&tw_adid=&tw_campaign=16705014684
- AmScope White Single Arm Boom Stand for Stereo Microscopes - Steel Arm, Tube Mount , 76mm Focus Block
- 9" x 9" x 1-3/4" Solid Cast Steel Base  
- 40 lbs in Weight

Creating a part for the leg pipe:
![[Pasted image 20240810231840.png]]

Other params:
![[Pasted image 20240810231849.png]]

![[Pasted image 20240810231906.png]]
- took pbb 20 min or so to build this geom


High strength steel:
![[Pasted image 20240810231919.png]]

2024-08-10T23:24:04-07:00
Fuck need to scale the mesh so that it is not too dense along the pipe:
![[Pasted image 20240810232417.png]]

Boundary load of 186 N from the 40 lbs weight:
![[Pasted image 20240810232810.png]]

2024-08-10T23:28:36-07:00
Cleaned up and optimized the mesh:
![[Pasted image 20240810233014.png]]
![[Pasted image 20240810233024.png]]
![[Pasted image 20240810233040.png]]



HDPE does not have Young's modulus and poisson's ratio...
![[Pasted image 20240810232849.png]]
![[Pasted image 20240810232905.png]]
- going to use 1000 MPa and 0.3

2024-08-10T23:39:12-07:00
Done. Going to call it a day and go sleep.
![[Pasted image 20240810233923.png]]
- yaaay







