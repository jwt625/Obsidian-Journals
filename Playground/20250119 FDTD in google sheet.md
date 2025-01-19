2025-01-19T10:28:00-08:00

Forgot where I saw it but I saw a laplacian solver in microsoft excel. Thus I naturally want one for FDTD in google sheet.
![[Pasted image 20250119103032.png]]

![[Pasted image 20250119102835.png]]

Color is with conditional formatting:
![[Pasted image 20250119103120.png]]

`**Extensions > Apps Script**` Is not really working, but after turning on iterative calculation it updates whenever something got edited.

Two issues so far:
- auto update is annoying
- the source is not oscillating.
![[Pasted image 20250119103226.png]]

2025-01-19T10:41:03-08:00
Ok got the oscillating source working:
- `SIN(2 * PI() * $F$6 * $F$77 * $F$1)`


2025-01-19T11:16:26-08:00
Maybe I should check the equations:
![[Pasted image 20250119115416.png]]
![[Pasted image 20250119111635.png]]
Do I need to update them separately?

Stability criteria:
![[Pasted image 20250119115216.png]]
- https://link.springer.com/book/10.1007/978-3-031-01712-4
- going to calculate and verify this

1D transmission line example:
![[Pasted image 20250119123215.png]]

2025-01-19T13:07:08-08:00
Hell yeah it is working. I'm a dumb ass, the coupling should be shifted so that it would actually propagate.
![[Pasted image 20250119130729.png]]
Going to do 2D after lunch.
