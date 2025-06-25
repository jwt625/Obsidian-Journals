
2025-06-24T22:10:08-07:00
![[Pasted image 20250624221221.png]]
Going after the first one.
![[Pasted image 20250624221107.png]]


wtf COMSOL
![[Pasted image 20250624222650.png]]

2025-06-24T22:39:15-07:00
nvm my bad, I was trying to import it as 3D.

![[Pasted image 20250624223935.png]]

Actually it just need to be a surface
![[Pasted image 20250624224118.png]]

Next, adding the ports:
![[Pasted image 20250624224148.png]]
Wait the above pattern does not look right...

ok it is because the y axis is flipped lol.

![[Pasted image 20250624224955.png]]
- fuck, it is still wrong, fully enclosed squares got filled.


2025-06-24T23:00:12-07:00
Klayout is the best:
![[Pasted image 20250624230016.png]]

![[Pasted image 20250624230203.png]]

![[Pasted image 20250624230528.png]]
![[Pasted image 20250624230932.png]]

![[Pasted image 20250624232415.png]]

WTF??
![[Pasted image 20250624232649.png]]
2025-06-24T23:33:55-07:00
what the fuck are you talking about, it's clearly between two conductive boundaries:
![[Pasted image 20250624233401.png]]

Maybe because I copied the PEC over from ewfd??
oh fuck it is because they are at the boundaries and are thus surrounded by PECs...
![[Pasted image 20250624234006.png]]

- https://www.comsol.com/forum/thread/84141/interior-port

not sure how they fixed it, but I fixed it by removing the PEC on the other side of the port...

2025-06-24T23:52:54-07:00
Damn it actually works
![[Pasted image 20250624235252.png]]
![[Pasted image 20250624235434.png]]

![[S.png]]