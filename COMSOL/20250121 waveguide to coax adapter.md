2025-01-21T23:30:10-08:00

Going to do WR10.
https://www.everythingrf.com/tech-resources/waveguides-sizes

![[Pasted image 20250121233036.png]]
![[Pasted image 20250121233030.png]]

Examples:
![[Pasted image 20250121233323.png]]

Hmm but the commercial ones have input from 90 deg:
![[Pasted image 20250121233352.png]]
- https://www.pasternack.com/wr10-ug387-mod-round-flange-to-1.0mm-female-waveguide-coax-adapter-pe-w10ca003-p.aspx
- Freaking $7820
- do they even have a taper?

From minicircuits:
![[Pasted image 20250121233834.png]]
- https://www.minicircuits.com/WebStore/dashboard.html?model=WR10-10R%2B

Ok let's just try sticking the pin inside.

2025-01-21T23:52:48-08:00
Done:
![[Pasted image 20250121235302.png]]
Pretty decent already.
Going to sweep the pin length and position a bit. Target at 90 GHz.

Port location sweep:
![[Pasted image 20250121235500.png]]
- going to use 0.75 mm

2025-01-21T23:55:36-08:00

Sweeping length of the pin inside.

![[Pasted image 20250121235742.png]]
- Going to do a finer sweep around 0.6 mm
2025-01-22T00:00:50-08:00
lol it is still 0.6 mm.
![[Pasted image 20250122000047.png]]

Ok now going to do the freq sweep again.
Hmm maybe the port location again.

![[S_param.png]]
![[S11_param.png]]

![[wave.gif]]

2025-01-22T00:07:24-08:00
I'm happy with this.

Including below the cutoff for fun:
![[S_param_WG_cutoff.png]]

Final parameters used:
![[Pasted image 20250122001457.png]]
And mesh:
![[Pasted image 20250122001513.png]]

