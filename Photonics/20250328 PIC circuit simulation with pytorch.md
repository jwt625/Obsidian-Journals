2025-03-28T22:47:11-07:00
Because most of photonic integrated circuits are linear.
Even the time dependency can be modeled with linear operations between different frequencies.

2025-03-28T23:39:54-07:00
Got it working. cursor pretty much wrote the whole thing:
![[Pasted image 20250328234004.png]]
![[Pasted image 20250328234009.png]]

![[Pasted image 20250328234014.png]]


2025-03-29T13:27:57-07:00
Added directional coupler with coupled mode theory
![[Pasted image 20250329132806.png]]

Tuned it to be closer to an actual 50:50 BS:
- L=0.00000195
- kappa/k0 = 100e-3
![[Pasted image 20250329133759.png]]

# Training!
2025-03-29T16:18:40-07:00
Setup the DC for training to learn kappa and delta.
Before training, same as above.
![[Pasted image 20250329161959.png]]
- parameters are now normalized to k0
After training:
![[Pasted image 20250329162018.png]]
Although this is unrealistic.
It should actually be optimized along the direction of propagation, i.e., it is more practical to control the coupling along the direction of propagation instead of controlling the coupling rate in the wavelength domain.

## Realistic directional coupler

2025-03-29T23:17:11-07:00
It should allow only a scaling of the kappa and delta vs wavelength curve.
Done. Single segment. The optimization did something lol:
![[Pasted image 20250329232049.png]]

Hmm not changing much before and after optimization:
![[Pasted image 20250329235815.png]]

![[Pasted image 20250329235829.png]]

Probably need more dof, or stronger dispersion..
2025-03-30T00:04:37-07:00
Ok added delta and kappa shift, seems to work:
![[Pasted image 20250330000447.png]]

Hmm interesting that different number of segments does not seem to do much:
- the above is 3 segments and below is 10 segments:
![[Pasted image 20250330000536.png]]
- they might need to be initialized to be slightly different?
- is the training tesults giving identical segments?
![[Pasted image 20250330000926.png]]
- ok not really, there is some distribution
vs. three segments:
![[Pasted image 20250330001012.png]]

five segments, before and after the training:
![[Pasted image 20250330001203.png]]
Before:
![[Pasted image 20250330001212.png]]
After:
![[Pasted image 20250330001221.png]]

# Add ring resonator
2025-03-30T01:30:11-07:00
Got the ring working using FP cavity:
![[Pasted image 20250330013021.png]]

However the ring used in the mzi is bullshit:
![[Pasted image 20250330013459.png]]

2025-03-30T01:54:51-07:00
ok got it working:
![[Pasted image 20250330015455.png]]

2025-03-30T11:36:25-07:00
Prompt for getting the cost function and the training code:
ok I have fixed the code. Now I want to train the nn over the ring resonator parameters to improve the linearity of the positive slope of the MZI fringes. Implement this cost function and the training. hint, could following the following steps for the cost function:
1. calculate the derivative of the MZI curve, and identify the segments with positive slope.
2. Take the average of the derivative for each positive segment, and then calculate the msq deviation of the actual derivative wrt. the average derivative over these segments
3. sum them up as the cost function to minimize. Please confirm you unerstand the task, provide feedback on this approach, and implement the lost function and the training code.


2025-03-30T11:46:28-07:00
Hmm not changing much?
![[Pasted image 20250330114633.png]]

Only minor change:
![[Pasted image 20250330114741.png]]

- likely because the loss values are too big.
- fixed. No improvement...

The loss function seems to be working, at least on simple examples:
![[Pasted image 20250330122852.png]]
Also working with the fancy mzi + ring:
![[Pasted image 20250330122953.png]]

Optimization method:
- tried LBFGS in addition to Adam. Worse.

2025-03-30T12:31:14-07:00
Played with initial ring parameters, now this is reward cheating:
![[Pasted image 20250330123126.png]]

2025-03-30T12:46:40-07:00
lol the mirror has gain lets go:
![[Pasted image 20250330124650.png]]


2025-03-30T12:58:25-07:00
Using sigmoid to make the r and a of the ring to be within 0 and 1:
![[Pasted image 20250330125836.png]]
- this pretty much looks like an MZI inside an MZI...

2025-03-30T13:20:07-07:00

Played around a bit more:
![[Pasted image 20250330132011.png]]
- L: 149.51112365722656
- r: -1.9592748880386353
- a: -3.7069528102874756 
- phase_shift: -9.004223823547363

# Adding splitters

2025-03-30T13:54:35-07:00
Done, in BasicComponents.py

Prompt:
Create a test script to use YSplitter, YCombiner, and Delayline to create a MZI. Do the forward pass and calculate & plot the output power. Take a look at similar test script in mzi_network.py

Yep working:
![[Pasted image 20250330135607.png]]

Could get some funny shapes:
![[Pasted image 20250330140430.png]]

2025-03-30T14:23:01-07:00
lol mom I'm doing linear neural network
![[Pasted image 20250330142312.png]]
![[Pasted image 20250330142611.png]]

Looks like a mess:
![[Pasted image 20250330142623.png]]

