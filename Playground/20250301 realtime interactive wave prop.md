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
