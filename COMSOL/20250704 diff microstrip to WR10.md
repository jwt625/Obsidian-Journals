2025-07-04T10:49:45-07:00
Because I saw this post:
- https://x.com/Aaronia_AG/status/1941048489238319584https://x.com/Aaronia_AG/status/1941048489238319584
![[Pasted image 20250704105019.png]]

Also good reference:
- https://ieeexplore.ieee.org/abstract/document/5781943
- ![[Pasted image 20250704105658.png]]
- ![[Pasted image 20250704105735.png]]
- ![[Pasted image 20250704105710.png]]
- pretty good



It is not tnar straight forward. S21 sucks and S11 also sucks:
![[Pasted image 20250704105303.png]]
![[Pasted image 20250704105310.png]]
- optimal shift around 3.75e-4

shift sweep:
```
2.0000E-4	1.0000E11	-4.8781	-1.7355
2.5000E-4	1.0000E11	-4.6331	-1.8595
3.0000E-4	1.0000E11	-4.5258	-1.9167
3.5000E-4	1.0000E11	-4.4982	-1.9320
4.0000E-4	1.0000E11	-4.4529	-1.9566
4.5000E-4	1.0000E11	-4.5747	-1.8889
5.0000E-4	1.0000E11	-4.6173	-1.8659
5.5000E-4	1.0000E11	-4.8938	-1.7250
6.0000E-4	1.0000E11	-4.9679	-1.6901
6.5000E-4	1.0000E11	-5.2602	-1.5580
7.0000E-4	1.0000E11	-5.3497	-1.5208
7.5000E-4	1.0000E11	-5.5284	-1.4489
8.0000E-4	1.0000E11	-5.6754	-1.3933
8.5000E-4	1.0000E11	-5.7911	-1.3516
9.0000E-4	1.0000E11	-5.9916	-1.2813
9.5000E-4	1.0000E11	-6.1488	-1.2298
0.0010000	1.0000E11	-6.3210	-1.1770
```





does not seem very dependent on shift.
2025-07-04T10:53:22-07:00
Going to sweep width of the "antenna" `w_stub`.
- nominal is wy = 1.27 mm


Stub width sweep (w_stub, freq, S12, S22):
```
5.0000E-4	1.0000E11	-10.807	-0.38638
5.5000E-4	1.0000E11	-10.296	-0.43620
6.0000E-4	1.0000E11	-9.7326	-0.49928
6.5000E-4	1.0000E11	-9.3483	-0.54794
7.0000E-4	1.0000E11	-8.8026	-0.62622
7.5000E-4	1.0000E11	-8.3644	-0.69697
8.0000E-4	1.0000E11	-7.9670	-0.76911
8.5000E-4	1.0000E11	-7.4930	-0.86629
9.0000E-4	1.0000E11	-7.1141	-0.95368
9.5000E-4	1.0000E11	-6.7231	-1.0545
0.0010000	1.0000E11	-6.3362	-1.1664
0.0010500	1.0000E11	-5.9578	-1.2893
0.0011000	1.0000E11	-5.6112	-1.4150
0.0011500	1.0000E11	-5.1786	-1.5932
0.0012000	1.0000E11	-4.8741	-1.7347
0.0012500	1.0000E11	-4.6097	-1.8701
0.0013000	1.0000E11	-4.2118	-2.0998
0.0013500	1.0000E11	-4.0052	-2.2330
0.0014000	1.0000E11	-3.7421	-2.4186
```

2025-07-04T11:13:41-07:00
Freq sweep:
![[Pasted image 20250704111344.png]]
Ugh why does this suck?

2025-07-04T11:17:52-07:00
Ok I think the substrate thickness also matters
https://doi.org/10.1109/APMC.2008.4957941

![[Pasted image 20250704111908.png]]

Added a cavity backing of lambda/4 around 90 GHz:
![[Pasted image 20250704112054.png]]

Yep better around 90 GHz:
![[Pasted image 20250704112241.png]]

Going to do 95 GHz, as well as sweeping shift and w_stub around it.
2025-07-04T11:28:44-07:00
Stub still seems to be the longer the better.

2025-07-04T11:34:24-07:00
Holy shift is doing a lot:
```
5.0000E-4	9.5000E10	-0.17633	-14.232
5.5000E-4	9.5000E10	-0.16408	-14.539
6.0000E-4	9.5000E10	-0.075399	-18.174
6.5000E-4	9.5000E10	-0.050427	-20.248
7.0000E-4	9.5000E10	-0.013821	-30.048
7.5000E-4	9.5000E10	-0.026321	-23.963
8.0000E-4	9.5000E10	-0.089114	-17.409
8.5000E-4	9.5000E10	-0.21908	-13.249
9.0000E-4	9.5000E10	-0.46500	-10.027
9.5000E-4	9.5000E10	-0.75343	-8.0316
0.0010000	9.5000E10	-1.2038	-6.1959
0.0010500	9.5000E10	-1.6622	-5.0014
0.0011000	9.5000E10	-2.2533	-3.9485
0.0011500	9.5000E10	-2.8269	-3.2191
0.0012000	9.5000E10	-3.4399	-2.6346
```

And it corresponds to well centered antenna, makes sense:
![[Pasted image 20250704113535.png]]

2025-07-04T11:42:24-07:00

![[S_param.png]]
![[Ey.gif]]


# Conclusion

This is just like many other coax / microstrip to WR transition, antenna + resonant cavity backing.

Use TEM boundary mode for the port, and assign ground and terminal/potentials accordingly:
![[Pasted image 20250704120933.png]]![[Pasted image 20250704120944.png]]












