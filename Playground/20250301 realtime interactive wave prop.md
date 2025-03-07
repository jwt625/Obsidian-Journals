2025-03-01T09:06:47-08:00
repo: https://github.com/chiuhans111/interactwave

Used git ingest to ask chatGPT:
- https://gitingest.com/

Installed npm with brew.
Then:
`npm install`
`npm run dev`

Yes now I have it running locally.
- http://localhost:5173/


# ChatGPT summary

https://chatgpt.com/share/67c3519c-7774-8011-bd23-eec6c4353594

## **Key Features and Functionality**

1. **Shader-Based Wave Simulation**
    
    - Uses **WebGL shaders** to compute wave propagation in real time.
    - Implements **FFT (Fast Fourier Transform)** for efficient computation.
    - Simulates **wavefront shaping and diffraction**.
2. **FFT-Based Computation**
    
    - The mathematical derivation in `readme.md` explains the **Cooley-Tukey FFT algorithm** used in the project.
    - Implements FFT in fragment shaders (`fft.frag`, `fft.js`) for **GPU-accelerated processing**.
3. **User Interaction**
    
    - Users can **click and drag** to interact with the simulation.
    - Mouse and touch events (`interactive.js`) update parameters dynamically.
    - `overlay.js` provides a **graphical overlay** of the wavefront.
4. **Rendering and Visualization**
    
    - **GLSL Shaders** (`draw.frag`, `shape.frag`, `wpm.frag`) handle wavefront rendering.
    - Implements **custom colormaps** (`colormap.frag`) for visualizing intensity and phase.
5. **Development with Vite & WebGL**
    
    - Uses **Vite** for bundling and hot-reloading.
    - `vite-plugin-glsl` allows importing GLSL shader files directly.
    - **TypeScript support** (`tsconfig.json`) ensures type safety.

---

## **Code Breakdown**

### **Frontend (Web UI)**

- **`index.html`** – Sets up the canvas and buttons.
- **`style.css`** – Defines UI styling.

### **Core WebGL Logic**

- **`interactive.js`** – Handles mouse input and updates simulation parameters.
- **`render.js`** – Sets up WebGL with `regl`.
- **`smoothvar.js`** – Implements smooth transitions for simulation parameters.

### **Shader-Based Computation**

- **`fft.frag` / `fft.js`** – Implements **Fast Fourier Transform (FFT)** in GLSL.
- **`wpm.frag` / `wpm.js`** – Implements **wave propagation method** (WPM).
- **`shape.frag` / `shape.js`** – Defines initial wavefront conditions.
- **`draw.frag` / `draw.js`** – Renders the wavefield.

### **Utilities**

- **`utils/colormap.frag`** – Defines color mapping functions.
- **`utils/pack.frag`** – Converts data between GLSL-compatible formats.

---

## **How It Works**

6. **Initial Wavefront Creation**
    
    - The shape shader (`shape.frag`) generates an initial wavefront based on user input.
7. **FFT Computation**
    
    - The wavefield undergoes an **FFT transformation** (`fft.frag`), representing the field in frequency space.
8. **Wave Propagation**
    
    - The **WPM shader (`wpm.frag`)** propagates the wavefield by applying a phase shift in the Fourier domain.
9. **Inverse FFT (IFFT)**
    
    - The inverse FFT reconstructs the wavefield in the spatial domain.
10. **Rendering**
    
    - The final wave pattern is displayed using the `draw.frag` shader.

---

## **Potential Applications**

- **Optical wave simulation** (Fourier optics, diffraction)
- **Acoustic wave modeling**
- **Electromagnetic wave analysis**
- **Signal processing & FFT visualization**

---

## **Final Thoughts**

This project is a **real-time wave simulation tool** using **WebGL and FFT**. The well-structured shader pipeline efficiently simulates and visualizes wavefront propagation, making it suitable for **physics, optics, and signal processing applications**.



# Reading the code

2025-03-01T13:36:22-08:00
Got `Shader languages support` plugin for syntax highlight for `.drag` files.

Built-in functions:
![[Pasted image 20250301133707.png]]

Custom functions:
![[Pasted image 20250301133722.png]]

External libraries:

| Library              | Purpose                            | Used In                              |
| -------------------- | ---------------------------------- | ------------------------------------ |
| `regl`               | GPU rendering (WebGL wrapper)      | `src/render.js`, `src/shaders/*.js`  |
| `vite-plugin-glsl`   | Importing GLSL shaders in JS files | `vite.config.js`, `src/shaders/*.js` |
| `sass`               | CSS preprocessor                   | `src/style.css`                      |
| `typescript`         | Static typing for JS               | `tsconfig.json`, `src/vite-env.d.ts` |
| `vite`               | Development server & bundler       | `vite.config.js`, `package.json`     |
| `@microsoft/clarity` | User interaction analytics         | `src/main.js`                        |
| `gh-pages`           | GitHub Pages deployment            | `package.json`                       |




Got `Dependency Cruiser Extension` :
![[Pasted image 20250301134017.png]]
- it is not resolving the frag files properly.
- `npm install vite-plugin-glsl --save-dev
- 

# Trying to add a thin lens

2025-03-01T14:51:10-08:00
![[Pasted image 20250301145125.png]]
Broken. Already fixed a bunch of errors. Lens radius is also changing beam radius.
- I think this is likely due to the event listener. Since the cursor drag still only changes the beam size, and the lens size and beam size could be different. It is just the slider is also changing the beam size.

2025-03-01T15:27:17-08:00
![[Pasted image 20250301152652.png]]
We are both dumb fuck.


2025-03-01T15:24:01-08:00
Ok fixed the appearance of the lens:
![[Pasted image 20250301152410.png]]

2025-03-01T18:22:21-08:00
Ok I got it work about 1h ago. Turns out I should be updating it in real space in shape.js.
- see https://x.com/jwt0625/status/1896012120304861340
![[Pasted image 20250301182321.png]]


# Thick spherical lens

2025-03-01T19:45:09-08:00
How do we propagate different refractive index?

2025-03-02T20:01:40-08:00
I don't think wave propagation method is going to work for this.

2025-03-02T23:41:03-08:00
Found weird numerical issue? The old FFT+WPM+FFT decays faster when there are more z grids
With `const NH = Math.round(N / 4)` (original):
![[Pasted image 20250302234159.png]]

With `const NH = Math.round(N*2)`:
![[Pasted image 20250302234251.png]]

# 2D FD-BPM

2025-03-01T12:54:59-08:00

Following chapter 2.1.1 of book Photonic Devices for Telecommunications.
Coordinates:
- z is direction of prop
- y is out of plane (translational symmetric)
- x is where the wave is confined
TE: E field is parallel to y.
TM: H field is parallel to y.

Funny this TM would be what people call TE in 3D waveguides.

![[Pasted image 20250301125731.png]]
![[Pasted image 20250301125817.png]]
![[Pasted image 20250302200210.png]]

Jump to ref. 27:
![[Pasted image 20250302200302.png]]
- paraxial approx: https://ocw.mit.edu/courses/6-974-fundamentals-of-photonics-quantum-electronics-spring-2006/871c32e6e4a44cbb1546741ef06f0f2f_parax_wav_eq_gau.pdf
![[Pasted image 20250304001927.png]]


Also should read FFT-BPM:
- Thylén1989: Theory and Applications of the Beam Propagation Method:
	- https://opg.optica.org/abstract.cfm?uri=GWOE-1989-SB1
	- no access, also not on sci-hub
- Thylén1982: The beam propagation method: an analysis of its applicability
	- https://link.springer.com/article/10.1007/bf00619865
	- this one is good and fundamental, from the same person
![[Pasted image 20250302203230.png]]
![[Pasted image 20250302203238.png]]

2025-03-02T23:43:25-08:00
Spent the whole night debugging it. Found it works better with much larger domain... Maybe because the source gets cutoff too sharply.
Domain = 300 (increased by 10x):
![[Pasted image 20250302234447.png]]

3000:
![[Pasted image 20250302234516.png]]
- should update the source.
- 2025-03-04T00:40:01-08:00
- updated the source to a more smooth gaussian. Did not help.
- found that reducing the grid resolution helps...

n = 7 instead of 10:
![[Pasted image 20250304005050.png]]
Increase domain size by 10x vs decrease dz manually in BPM by 10x:
![[Pasted image 20250304004914.png]]
![[Pasted image 20250304005024.png]]

## Debug BPM convergence

2025-03-04T01:04:14-08:00
Use python to check Beam Prop stability:
![[Pasted image 20250304010423.png]]
![[Pasted image 20250304010430.png]]
This sucks.

2025-03-04T21:10:32-08:00
Ok fixed the code and now it is actually propagating:
![[Pasted image 20250304211040.png]]
Why is it blowing up??

Increase domain size from 2^7 to 2^8 made it worse:
![[Pasted image 20250304211113.png]]
- This looks exactly like the GLSL simulation.
- So I guess it is good news

Asking ChatGPT to adapt it into RK4:
- https://en.wikipedia.org/wiki/Runge%E2%80%93Kutta_methods

RK4, no longer diverging!
![[Pasted image 20250304211958.png]]
Smaller beam:
![[Pasted image 20250304212025.png]]

Add phase for focusing is working as well:
![[Pasted image 20250304212216.png]]
- damn okay chatGPT is good

Changing the x grid size is somehow changing the beam behavior or defocusing:
Nx = 2^9:
![[Pasted image 20250304212528.png]]

Nx = 2^7:
![[Pasted image 20250304212551.png]]
At least RK4 is doing its job.

Ok back to bpm.frag:
Naive:
![[Pasted image 20250304221127.png]]

RK4:
![[Pasted image 20250304221154.png]]

It is much better, but still not enough...

Meanwhile, the python version could run 2000 steps no problem:
![[Pasted image 20250304221338.png]]

And even 10k steps:
![[Pasted image 20250304221536.png]]

Ok stupid chatGPT were not computing the intermediate fields properly:
![[Pasted image 20250304222500.png]]
- still using the old E field


## RK4 in GLSL
2025-03-05T00:42:09-08:00
Spent ~ 3 h ish, still not working. Compiles but not propagating.

## Use 16 bit instead of 8 bit
2025-03-05T00:42:37-08:00

Going to try this instead.
What is the original encoding?


# Continue in python, adding n distribution
2025-03-05T22:09:20-08:00
Adding lens seems working. However beam is not affected by it:
![[Pasted image 20250305220935.png]]
- oh n0 was set to 1.5...
Updated:
![[Pasted image 20250305221553.png]]

Now the propagation is diverging:
![[Pasted image 20250305221609.png]]

Even setting the n_lens to 1.0 still cause divergence:
![[Pasted image 20250305221654.png]]
However if they are both high (n0=1.5 and n_lens=1.55) then no divergence:
![[Pasted image 20250305221738.png]]
- however it looks like the lens is not doing anything?

n0=n_lens=1.35:
![[Pasted image 20250305221837.png]]
n0=1.35, n_lens=1.55:
![[Pasted image 20250305221930.png]]
No difference.
- they look different on two separate monitors lol.
The lens is probably too small and thin to make a difference. n_lens=2.5 made a difference:
![[Pasted image 20250305222107.png]]

## Divergence test:
n0=n_lens=1.001:
![[Pasted image 20250305222303.png]]
n0=n_lens=1.01:
![[Pasted image 20250305222316.png]]
n-=n_lens=1.1:
![[Pasted image 20250305222328.png]]
Why??

Here is n=1.001, Nx=2^9, domain size 30 um vs 60 um wide:
![[Pasted image 20250305223741.png]]
![[Pasted image 20250305223748.png]]

I think o3-mini-high got it, 1/dx^2
![[Pasted image 20250305223823.png]]
i.e., do not over-mesh along x.

2025-03-05T22:38:57-08:00
ok the fucking lens seems to be working:
![[Pasted image 20250305223907.png]]
Let's try to make the lens clip the beam:
![[Pasted image 20250305224319.png]]
idk maybe this is how it should look like.
![[Pasted image 20250305224356.png]]
This is some album cover shit lol.
![[Pasted image 20250305224826.png]]

2025-03-05T23:15:54-08:00
Implemented an S bend:
![[Pasted image 20250305231601.png]]

2025-03-05T23:24:53-08:00
Implemented eigen mode source:
![[Pasted image 20250305232501.png]]
- why so many strips?
- let's try narrower waveguide:
![[Pasted image 20250305232542.png]]
fucking hell how is this working I'm in tears...

Ok it is time to import from GDS files.

Testing straight WG:
![[Pasted image 20250305233647.png]]
Hmm seems like it is exciting multiple modes.
Narrower waveguide is better (0.5 um):
![[Pasted image 20250305233718.png]]

2025-03-05T23:58:43-08:00
Trying to increase z step size, convergence problem again
![[Pasted image 20250305235854.png]]
- this is Nx = 2^9, and dz = 4\*dx, domain size 100 um
- However 3\*dx is fine, and looks identical to dz=dx:
![[Pasted image 20250306000003.png]]
Need to fix the mode source:
![[Pasted image 20250306000042.png]]
2025-03-06T00:17:19-08:00
Fixed:
![[Pasted image 20250306001721.png]]

Yeah now this looks like fundamental mode:
![[Pasted image 20250306001806.png]]
1 um wide:
![[Pasted image 20250306001818.png]]

Going to continue with GDS import tmw.

2025-03-06T22:38:16-08:00
Fixed bug in slab mode source, not it looks better, and got option to excite higher order modes:
![[Pasted image 20250306223835.png]]

Look at all these modes with a 4 um wide waveguide:
![[Pasted image 20250306223915.png]]

2025-03-06T23:23:39-08:00
Actually found some bugs about the slab mode, now fixed. Not sure why it was working before:
![[Pasted image 20250306232357.png]]


## Adding PML
2025-03-06T23:37:25-08:00
magnitude = 5, too high:
![[Pasted image 20250306233801.png]]

Reduced to 1 worked fine:
![[Pasted image 20250306233816.png]]

Final, comparing between with and without PML:
![[Pasted image 20250306233831.png]]
![[Pasted image 20250306233841.png]]

2025-03-07T00:13:13-08:00
Found the wave is decaying when dz is too high.
This is dz=2\*dx vs dz=3\*dx:
![[Pasted image 20250307001354.png]]
![[Pasted image 20250307001338.png]]

Ok going to use 2dx instead of 3dx from now on.
Also should figure out better treatment of discrete boundary:
![[Pasted image 20250307001547.png]]
- maybe it is time to read Vassallo1992 again.
- 
2025-03-07T00:17:26-08:00

An MMI should still work fine. Let's try MMI.
2025-03-07T00:40:57-08:00
Yes!
![[Pasted image 20250307004421.png]]
![[Pasted image 20250307004059.png]]
I'm surprised how well o3-mini-high is doing.






# Reading


2025-03-03T22:55:51-08:00
Going to read 
![[Pasted image 20250303225601.png]]
- ok this looks highly relevant.




## Going inside reference
2025-03-01T13:16:35-08:00

![[Pasted image 20250301131632.png]]
![[Pasted image 20250301131648.png]]


Tri-diagonal:
- Numerical Recipes in C




# Reading & references

## wave propagation method

Optical tomographic reconstruction based on multi-slice wave propagation method
- https://doi.org/10.1364/OE.25.022595

Light propagation through microlenses: a new simulation method
- https://doi.org/10.1364/AO.32.004984





## Beam prop

2025-03-01T09:54:37-08:00

```
RSoft's BeamPROP software is based on the Beam Propagation Method (BPM), a numerical technique for simulating the propagation of optical fields in waveguide structures. While the specific academic papers underpinning BeamPROP's implementation are not publicly specified by RSoft, the BPM itself has been extensively studied and documented in the scientific literature. For a foundational understanding of BPM, you might consider reviewing the following academic resources:​[WebThesis+2Synopsys+2ResearchGate+2](https://www.synopsys.com/photonic-solutions/rsoft-photonic-device-tools/passive-device-beamprop.html)

- **"Beam Propagation Method: Analysis and Assessment" by Feit and Fleck (1978):** This seminal paper introduced the BPM for optical waveguide analysis, providing a comprehensive overview of the method's theoretical foundations.​
    
- **"Numerical Simulation of Optical Wave Propagation with the Beam Propagation Method" by Hadley (1992):** This paper discusses the numerical aspects of BPM, including its implementation for simulating optical wave propagation.​[Synopsys+4IEEE Xplore+4IJMER+4](https://ieeexplore.ieee.org/document/9206453/)
    

These resources offer in-depth insights into the theoretical underpinnings of the Beam Propagation Method, which forms the basis of BeamPROP's simulation capabilities.​[Synopsys](https://www.synopsys.com/photonic-solutions/rsoft-photonic-device-tools/passive-device-beamprop.html)
```

Light propagation in graded-index optical fibers
- https://doi.org/10.1364/AO.17.003990
- this one is a classic

## Book

Numerical Simulation of Optical Wave Propagation with Examples in MATLAB
- https://spie.org/publications/book/866274

Photonic Devices for Telecommunications
- https://link.springer.com/book/10.1007/978-3-642-59889-0
- chapter 2

![[Pasted image 20250301105436.png]]

Numerical recipes in C: the art of scientific computing


## Papers cited in chapter 2.2.1:
- Chung1990: An assessment of finite difference beam propagation method
	- https://ieeexplore.ieee.org/abstract/document/59679
- Vassallo1992: Improvement of finite difference methods for step-index optical waveguides
	- https://digital-library.theiet.org/doi/abs/10.1049/ip-j.1992.0024
