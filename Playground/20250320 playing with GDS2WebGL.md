
2025-03-20T00:17:45-07:00

See https://github.com/s-holst/GDS2WebGL

Error when converting layers:

![[Pasted image 20250320001808.png]]

![[Pasted image 20250320003131.png]]
2025-03-20T00:30:44-07:00
Got it to run, but I dont see anything in out.html.
- by making the following changes (commented out int eh screenshot):
![[Pasted image 20250320003114.png]]

Tried running my tiny tapeout GDS, it runs without skipping the None type, but still nothing in out.html...
- GDS from https://github.com/jwt625/tt-test/actions/runs/13360194554
![[Pasted image 20250320003248.png]]
- size of the chip also seems too small?

2025-03-20T00:33:37-07:00
Ok the size seems correct, here are from the LNSOI chip:
![[Pasted image 20250320003356.png]]
Hmm there's definitely shit inside this html but not sure why I do not see anything:
![[Pasted image 20250320003654.png]]

Going to ask chat.
