
2025-02-10T23:53:07-08:00
Motivation:
- because I saw this tweet from Blake:
- https://x.com/bscholl/status/1888939430833975765
![[Pasted image 20250210235325.png]]

# Air properties
Going to get the air density distribution.
From chatGPT:
![[Pasted image 20250210235342.png]]
- dumbass drew the vertical dashed line for typical airplane elevation along air density...
Asked it to fix it:
![[Pasted image 20250210235519.png]]

Going to approximate this linearly.
Also this is so long. Probably still need many wavelengths to see the effect.
- anyway will change the min and max density to 0.4 and 1.2

Also need speed of sound
![[Pasted image 20250211000154.png]]
- hmm does not change much

# Pressure acoustics
Ok no varying properties, seems fine:
![[Pasted image 20250211000607.png]]

Soltime 1 second.

Enable variation:
![[Pasted image 20250211000718.png]]
Hmm small difference.

2025-02-11T00:07:47-08:00

Going to 10x the frequency. This is 1 kHz. Doing 10 kHz. Also 2x the mesh.
New mesh:
![[Pasted image 20250211000755.png]]
- DOF 177k, soltime 2 s. 

No varying properties:
![[Pasted image 20250211000841.png]]

With properties variation
![[Pasted image 20250211000925.png]]

Hmm cannot tell by eye. Would need a cutline plot
2025-02-11T00:12:00-08:00
![[Pasted image 20250211001157.png]]
x axis is from y = -1 m to y = 1 m, i.e., from dense and fast to light and slow.
- varying properties has more power going downward.

Playing with velocity change. Hmm seems like it is only changing the phase, not changing the amplitude. Interesting.
Also seems independent of frequency. This is 5 kHz:
![[Pasted image 20250211001921.png]]


2025-02-11T00:28:50-08:00
Ok got the radiation pattern:
- 5 kHz
![[Pasted image 20250211002856.png]]
- hmm why??
- 8 kHz:
![[Pasted image 20250211003012.png]]
- ???
- Could it be the radiator size?
	- changed to be 4x smaller, still get the same radiation pattern.
	- maybe it is the mesh... Going to do 2x finer.
- okay yeah it is the mesh:
![[Pasted image 20250211003225.png]]

At 1 kHz:
![[Pasted image 20250211003311.png]]
- god damn it, why??
![[Pasted image 20250211003502.png]]
Zero angle is to the right. Why is no wave going upward??

2025-02-11T00:36:15-08:00
Checking properties:
![[Pasted image 20250211003623.png]]
- `acpr.c`
![[Pasted image 20250211003650.png]]
- `acpr.rho`
Both looks correct.

2025-02-11T01:00:13-08:00

Ok just realized this polar plot in dB is confusing...
![[Pasted image 20250211010030.png]]


Sweeping the slope of change:
![[Pasted image 20250211011533.png]]

2025-02-11T01:15:35-08:00
I should go sleep...


2025-02-11T08:37:40-08:00
Tried making the medium isotropic near the source, still getting more power down.

![[Pasted image 20250211083839.png]]

![[Pasted image 20250211083850.png]]
- Two more possibilities:
	- this is sound pressure, not power
	- far field calculation is wrong for anisotropic media

# Discrete ray tracing

Going to do a ray tracing.
Colored by time:
![[Pasted image 20250211085724.png]]

Back to pressure acoustics, added isotropic shell before FF calculation:
![[Pasted image 20250211090306.png]]

![[Pasted image 20250211090317.png]]

The above is 3 kHz, this is 1 kHz:
![[Pasted image 20250211090414.png]]
6 kHz:
![[Pasted image 20250211090616.png]]

Changed back to anisotropic, plotting intensity:
![[Pasted image 20250211092201.png]]

# Continuous ray tracing
![[Pasted image 20250211095757.png]]
Color by elevation angle:
![[Pasted image 20250211222901.png]]


2025-02-11T22:27:29-08:00
Hmm somehow fixed by increasing the velocity change:
![[Pasted image 20250211222740.png]]
![[Pasted image 20250211222751.png]]
Streamline really matches the ray optics sim:
![[Pasted image 20250211231601.png]]


FF looks like a mess:
![[Pasted image 20250211222810.png]]
![[Pasted image 20250211222828.png]]


Wider rectangle:
![[Pasted image 20250211232157.png]]

