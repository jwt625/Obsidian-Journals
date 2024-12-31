2024-12-30T21:58:50-08:00
![[Pasted image 20241230220008.png]]

This is FlyPanel BG-630 from BG Systems Inc.
- not much information online.
- It does looks similar to other joysticks from them:
	- https://www.bgsystems.com/assets/jfx-mk-101.pdf
![[Pasted image 20241230220113.png]]
![[Pasted image 20241230220957.png]]



More manuals:
- https://www.bgsystems.com/manuals.html


I really just need info on what the pins are for the DB-15 connector.

Okay this one seems relevant:
- https://www.bgsystems.com/assets/flyboxhwm.pdf
2024-12-30T22:18:54-08:00
Hell yeah this is the one:
- https://www.bgsystems.com/assets/jfxhwmanual.pdf
![[Pasted image 20241230221937.png]]
![[Pasted image 20241230222314.png]]
- where the fuck is the BG-14-xxx drawing
- Pin 13 and 14 carry power

Checking the cereal box manual:
- https://www.bgsystems.com/assets/cerealboxhwm.pdf
![[Pasted image 20241230222201.png]]


Should just power up and measure all the other channels.
- only 7 pins are used
![[Pasted image 20241230222511.png]]
![[Pasted image 20241230225708.png]]

Ok nah I think pin 14 (red) and 15 (black) are the power supply. It is sent to all three PCBs.
- blue, yellow, and orange are the three axis
- the extra white and red is going into the joystick handle and not sure what they are for.

Now I need a 5 V power supply.
Going to use AD2.

2024-12-30T23:23:50-08:00
Ok now I got AD2 hooked up on the windows PC.

2024-12-30T23:53:35-08:00
Lol it is working, so much easier than I expected.
This is me doing loops:
![[Pasted image 20241230235351.png]]
AD2 only has two channels. Also tested the 3rd dof and it's working fine.
Stop here for now.




