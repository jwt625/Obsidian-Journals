2024-12-18T19:25:37-08:00
Mainly wanna figure out if the hook shape is necessary or not.
Reference:
- https://x.com/jwt0625/status/1869325104888549886
- https://www.ainfostore.com/lb-osj-07120-sf-open-boundary-quad-ridged-dual-polarization-horn-antenna-0-7-12-ghz-12db-gain-sma-female

Threshold & extract seems to work fine:
![[Pasted image 20241218192634.png]]

Going to select / crop the image first.
2024-12-18T19:41:47-08:00
Made the plot interactive.

![[Pasted image 20241218194140.png]]

Ok need to make this into a curve.
![[Pasted image 20241218194723.png]]

Yeees
![[Pasted image 20241218194929.png]]


2024-12-18T21:13:07-08:00
Conversion from pixel to mm:
- scale bar: x = 688 - 252 = 436 pixel
- 258.2 mm
- 0.5922 mm per pixel

2024-12-18T21:24:40-08:00
Added in COMSOL, and did minor fix. Very similar to the vivaldi antenna:
![[Pasted image 20241218212454.png]]
![[Pasted image 20241218213352.png]]
![[Pasted image 20241218213430.png]]

Original freqs:
- range(2[GHz],0.5[GHz],6.5[GHz])

