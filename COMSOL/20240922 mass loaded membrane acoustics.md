
2024-09-22T22:57:37-07:00
Following this paper:
https://www.mdpi.com/1996-1944/16/3/1308
- god I hate MDPI papers

2024-09-22T23:19:41-07:00
Seems to be working?
![[Pasted image 20240922231949.png]]
Fixed the legend:
![[Pasted image 20240922233754.png]]


Also maybe no need for numerical port since the mode profile is flat (TBA).


2024-09-22T23:30:08-07:00
Added mass loading:
![[Pasted image 20240922233012.png]]

Hmm why is S11 higher than 1??

The membrane eigen modes are different:
![[Pasted image 20240922233259.png]]
![[Pasted image 20240922233308.png]]

2024-09-22T23:40:35-07:00
Using finer mesh for the membrane and check the mechanical modes.
![[Pasted image 20240922234220.png]]
![[Pasted image 20240922234118.png]]
![[Pasted image 20240922234146.png]]

2024-09-23T00:04:27-07:00
Freq sweep done.
![[Pasted image 20240923000430.png]]
- flipped the sign for S21 to match with their definition
- should pbb mesh the membrane better next


Found typo in material selection for the membrane.
Also found missed the internal tension for the membrane:
![[Pasted image 20240923001829.png]]

Found this post and Ivar saving the day:
- https://www.comsol.com/forum/thread/323451/how-to-provide-tension-in-membrane
	- actually it is Henrik not Ivar
- https://www.comsol.com/model/vibrating-membrane-12587
	- seems quite involving
![[Pasted image 20240923002329.png]]

2024-09-23T00:36:15-07:00
Getting nonsense even just solving the membrane:
![[Pasted image 20240923003658.png]]
Hmm why does the example mode uses prescribed displacement?
- going to copy them
2024-09-23T00:44:52-07:00
Ok find out somehow there is no out of plane motion:
![[Pasted image 20240923004502.png]]

Where is the stress stiffening from?
Membrane physics:
![[Pasted image 20240923005143.png]]
Hmm it looks different in the pressure acoustic + solid mech model:
![[Pasted image 20240923005438.png]]

Solid mech physics:
![[Pasted image 20240923005220.png]]
![[Pasted image 20240923005208.png]]

2024-09-23T01:03:25-07:00
Built a toy model and it is still only deforming in-plane...
![[Pasted image 20240923010336.png]]

Giving up for now, going to sleep.


