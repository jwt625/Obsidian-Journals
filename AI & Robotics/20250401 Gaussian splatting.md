
2025-04-01T22:08:40-07:00

Cloning the repo:
- https://github.com/graphdeco-inria/gaussian-splatting
Paper:
- https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/

Going to try conda... Conda was installed by homebrew. I do not feel good about this...
```
conda env create --file environment.yml  
conda activate gaussian_splatting
```

Look at this shit:
![[Pasted image 20250401221346.png]]

Yeah of course:
```
wentaojiang@Wentaos-Mac-mini gaussian-splatting % conda env create --file environment.yml
conda activate gaussian_splatting
/opt/homebrew/anaconda3/lib/python3.12/argparse.py:2006: FutureWarning: `remote_definition` is deprecated and will be removed in 25.9. Use `conda env create --file=URL` instead.
  action(self, namespace, argument_values, option_string)
Retrieving notices: ...working... done
Channels:
 - pytorch
 - conda-forge
 - defaults
Platform: osx-arm64
Collecting package metadata (repodata.json): done
Solving environment: failed

PackagesNotFoundError: The following packages are not available from current channels:

  - python=3.7.13*
  - cudatoolkit=11.6*

Current channels:

  - https://conda.anaconda.org/pytorch
  - https://conda.anaconda.org/conda-forge
  - https://repo.anaconda.com/pkgs/main
  - https://repo.anaconda.com/pkgs/r

To search for alternate channels that may provide the conda package you're
looking for, navigate to

    https://anaconda.org

and use the search bar at the top of the page.



CondaError: Run 'conda init' before 'conda activate'
```

Cursor suggestions:
-    conda config --add channels nvidia
-    conda update conda
-    conda init

Do I need CUDA for this??
![[Pasted image 20250401222437.png]]

Going to consult chatGPT.
- https://www.reddit.com/r/GaussianSplatting/comments/1c4buxi/opensplat_now_supports_training_gaussian_splats/
- https://github.com/pierotofy/OpenSplat

# Brush
Following
- https://www.youtube.com/watch?v=VM6trBcPlaU
	- this guy is a legend


The fuck is brush
- https://github.com/ArthurBrussee/brush
- got v0.2.0:
![[Pasted image 20250401223558.png]]
2025-04-01T22:38:38-07:00
Going to try the garden preset.
holy shit the example just trains.

To prep data:
- https://github.com/jonstephens85/colmap-gradio
Following instructions inside:
`brew install colmap`
- damn this is taking a while
- 2025-04-01T22:52:54-07:00
- ok done
`brew install imagemagick`
- done. This one is quick

```
conda create --name colmap_env python=3.9 --no-default-packages -y
conda activate colmap_env
```
- Hmm always had to run `source ~/.zshrc` after `conda init` to make `conda activate` work...

```
git clone https://github.com/jonstephens85/colmap-gradio.git
cd colmap-gradio
pip install gradio==4.43.0
```

2025-04-01T23:01:12-07:00
ok all run and installed.

Moment of truth: `python colmap_gradio.py`
- of course this fucking python is linked to a different python
- going to try the installation in cursor

Ok I had to run this freaking line of code:
- `/opt/homebrew/anaconda3/envs/colmap_env/bin/python colmap_gradio.py `
- seems like it is running!
![[Pasted image 20250401230533.png]]

2025-04-03T21:04:26-07:00
It took 6 hours to train. Forgot to update yesterday morning.
![[Pasted image 20250403210536.png]]

# Trying my own images
Should try the training data preparation with COLMAP.
Stupid freaking python.
- ` /opt/homebrew/anaconda3/envs/colmap_env/bin/python colmap_gradio.py`

`
E20250403 21:20:25.982065 976648 feature_extraction.cc:265] IMG_7942.HEIC BITMAP_ERROR: Failed to read the image file format.`
Fuck HEIC.
- `for f in *.HEIC; do sips -s format png "$f" --out "${f%.*}.png"; done`

2025-04-03T21:25:45-07:00
okay now it seems to be running COLMAP in Gradio fine.

![[Pasted image 20250403212710.png]]

This is not going to work...
2025-04-03T21:32:48-07:00
Well it is actually working??
![[Pasted image 20250403213254.png]]
I'm going to 3DGS all the shit I've done teardown.
Looks amazing man:
![[Pasted image 20250403221100.png]]

2025-04-03T22:14:54-07:00
Took photos of the innolight transceiver.
Also why are the png files 10x larger than heic??

2025-04-03T23:22:22-07:00
innolight & servo adapter
![[Pasted image 20250403232241.png]]

# Deploy online interactive viewer

2025-04-03T23:32:02-07:00
ASked chatGPT for options:
- https://github.com/antimatter15/splat?tab=readme-ov-file
	- looks pretty good
- https://github.com/mkkellogg/GaussianSplats3D
	- hmm this seems more general, and three.js based

Going to try the three.js based one.
- holy it almost froze my computer..

2025-04-04T00:23:00-07:00

The default scene is from
```
const url = new URL(
    params.get("url") || "train.splat",
    "https://huggingface.co/cakewalk/splat-data/resolve/main/"
);

```

Converting ply to splat:
- `python3 convert.py your_model.ply --output your_model.splat`
- installed `plyfile`
- `python3 convert.py innolight.ply --output test.splat`
- converted
- 

2025-04-04T00:29:15-07:00
Ok got it rendered locally. However it looks worse. The spherical harmonic order is lower?
![[Pasted image 20250404004137.png]]
Yes it is because of the spherical harmonics order.
It is from this section in `convert.py`:
```
SH_C0 = 0.28209479177387814

color = np.array(
[
0.5 + SH_C0 * v["f_dc_0"],
0.5 + SH_C0 * v["f_dc_1"],
0.5 + SH_C0 * v["f_dc_2"],
1 / (1 + np.exp(-v["opacity"])),
]
)
```

ugh files are also too big:
- https://docs.github.com/en/repositories/working-with-files/managing-large-files
- the splat is only 20.9 MB though
- going to ignore the ply file.

2025-04-04T01:04:04-07:00
Deployed. Took a while for the pages to actually run and become active:
- https://jwt625.github.io/splat/
Looks like crap though because of zeroth order only.
![[Pasted image 20250404010453.png]]

## Make the webGL rendering with 3rd order spherical harmonics
2025-04-04T21:48:40-07:00
Updated `convert.py`
Now the splat file is ~150 MB.
Need to figure out large file on github.
- `git lfs install`
	- error
	- `brew install git-lfs`
	- ok now the above runs
- `git lfs track "*.splat"`

2025-04-04T21:55:41-07:00
What is this bullshit
![[Pasted image 20250404215546.png]]

Going to create a new repo.
2025-04-04T22:03:46-07:00
Done.
- https://github.com/jwt625/splat-lfs
2025-04-04T22:14:31-07:00
Hmm github page not working,  likely because it is using the small pointer file. Piece of shit git LFS.
Going to host the splat on hugging face.

2025-04-04T22:35:13-07:00
Tried loading the -v2 that has up to 3rd SH. Froze my browser. Fuck.

2025-04-04T23:06:12-07:00
Testing up to 0~1 SH.
- `python3 convert.py innolight.ply --output test.splat --sh_order 1`

2025-04-04T23:08:43-07:00
ok even this freeze the browser. It must be the rendering.

2025-04-04T23:28:25-07:00
ok actually read and understood convert.py.

## How does the rendering work?

2025-04-05T00:24:11-07:00
Still trying to understand the rendering
- https://github.com/ArthurBrussee/brush/blob/main/crates/brush-render/src/gaussian_splats.rs
- ok here is the actual rendering part: https://github.com/ArthurBrussee/brush/blob/main/crates/brush-render/src/shaders/project_visible.wgsl
- I see the same SH_C0 coefficient, great.
- there are some magic numbers. Reference:
	- https://jcgt.org/published/0002/02/06/
	- ![[Pasted image 20250405003404.png]]
Ok thank you o3-mini-high:
![[Pasted image 20250405003905.png]]
Useful stuff from the paper:
![[Pasted image 20250405004133.png]]
![[Pasted image 20250405004257.png]]
![[Pasted image 20250405004311.png]]

2025-04-05T00:47:51-07:00
Now the wgsl code makes more sense. Can I just use this wgsl, or get a glsl version?
Will continue after sleep.

2025-04-07T23:49:28-07:00
uh fuck is this going to force me to learn Rust lol
- https://www.reddit.com/r/webgpu/comments/10v5w53/why_is_the_webgpu_shading_language_wgsl_syntax/



## Rendering from ply

2025-04-07T22:42:46-07:00
Reading main.js and found it could directly load from ply file
- http://localhost:8080/?url=https://huggingface.co/datasets/jwt625/splat/resolve/main/innolight.ply

Also watching the compute shader 101:
- https://www.youtube.com/watch?v=DZRn_jNZjbw

Reading:
- https://webgpufundamentals.org/webgpu/lessons/webgpu-wgsl.html

2025-04-08T22:39:17-07:00
Saw Jonathan Stephens is using https://www.cloudcompare.org/main.html
- https://www.simulation.openfields.fr/index.php/cloudcompare-downloads/7-cloudcompare-macos-binaries

2025-04-09T00:01:22-07:00
Reading rust code from Brush-render
in `project_visible.wgsl`:
- the SH coefficients:
```

struct ShCoeffs {
    b0_c0: vec3f,

    b1_c0: vec3f,
    b1_c1: vec3f,
    b1_c2: vec3f,

    b2_c0: vec3f,
    b2_c1: vec3f,
    b2_c2: vec3f,
    b2_c3: vec3f,
    b2_c4: vec3f,

    b3_c0: vec3f,
    b3_c1: vec3f,
    b3_c2: vec3f,
    b3_c3: vec3f,
    b3_c4: vec3f,
    b3_c5: vec3f,
    b3_c6: vec3f,

    b4_c0: vec3f,
    b4_c1: vec3f,
    b4_c2: vec3f,
    b4_c3: vec3f,
    b4_c4: vec3f,
    b4_c5: vec3f,
    b4_c6: vec3f,
    b4_c7: vec3f,
    b4_c8: vec3f,
}

```
- `fn sh_coeffs_to_color` 

Where the SH coeffs are read:
- `project_visible.wgsl`
```
fn num_sh_coeffs(degree: u32) -> u32 {
    return (degree + 1) * (degree + 1);
}
```
```

    var sh = ShCoeffs();
    sh.b0_c0 = read_coeffs(&base_id);

    if sh_degree >= 1 {
        sh.b1_c0 = read_coeffs(&base_id);
        sh.b1_c1 = read_coeffs(&base_id);
        sh.b1_c2 = read_coeffs(&base_id);

        if sh_degree >= 2 {
            sh.b2_c0 = read_coeffs(&base_id);
            sh.b2_c1 = read_coeffs(&base_id);
            sh.b2_c2 = read_coeffs(&base_id);
            sh.b2_c3 = read_coeffs(&base_id);
            sh.b2_c4 = read_coeffs(&base_id);

            if sh_degree >= 3 {
                sh.b3_c0 = read_coeffs(&base_id);
                sh.b3_c1 = read_coeffs(&base_id);
                sh.b3_c2 = read_coeffs(&base_id);
                sh.b3_c3 = read_coeffs(&base_id);
                sh.b3_c4 = read_coeffs(&base_id);
                sh.b3_c5 = read_coeffs(&base_id);
                sh.b3_c6 = read_coeffs(&base_id);

                if sh_degree >= 4 {
                    sh.b4_c0 = read_coeffs(&base_id);
                    sh.b4_c1 = read_coeffs(&base_id);
                    sh.b4_c2 = read_coeffs(&base_id);
                    sh.b4_c3 = read_coeffs(&base_id);
                    sh.b4_c4 = read_coeffs(&base_id);
                    sh.b4_c5 = read_coeffs(&base_id);
                    sh.b4_c6 = read_coeffs(&base_id);
                    sh.b4_c7 = read_coeffs(&base_id);
                    sh.b4_c8 = read_coeffs(&base_id);
                }
            }
        }
    }
```

2025-04-09T00:33:16-07:00
Ran out of my patience for today...
Going to check rerun instead.
- following: https://rerun.io/docs/getting-started/installing-viewer
2025-04-09T01:06:16-07:00
damn it just works:
![[Pasted image 20250409010621.png]]
I need to go to sleep...
Continue rendering tomorrow!


2025-04-10T23:03:37-07:00
Gosh why can't I fold a message in chatGPT. This is stupid.

2025-04-17T22:50:18-07:00
Back on this again. Damn it has been a whole week.
Asked cursor to add comments to `main.js`
```
1. getProjectionMatrix: Creates a 3D-to-2D projection matrix using camera parameters

2. getViewMatrix: Creates a view matrix from camera position and rotation

3. multiply4: Performs matrix multiplication on two 4x4 matrices

4. invert4: Computes the inverse of a 4x4 matrix

5. rotate4: Applies rotation to a 4x4 matrix around a specified axis

6. translate4: Applies translation to a 4x4 matrix

7. createWorker: Sets up a WebGL worker for processing 3D data

8. main: The main function that initializes and runs the 3D viewer

9. resize: Handles window resize events

10. frame: The main render loop

11. isPly: Checks if data is in PLY format

12. selectFile: Handles file selection and loading

13. preventDefault: Prevents default browser behavior
```









# Captures

## Coherent laser
2025-04-13T19:54:29-07:00
![[image_20250413_191411.jpg]]
- body cam POV lol

Colmap: 
- `colmap-gradio` repo
- `/opt/homebrew/anaconda3/envs/colmap_env/bin/python colmap_gradio.py`
- 2025-04-13T19:55:56-07:00
	- ok running
2025-04-13T20:29:57-07:00
- done. Jesus that took forever (35 min). It better be good.

2025-04-14T22:20:37-07:00
Started second round after removing the inner cover.
COLMAP running. 128 images.
2025-04-14T22:41:18-07:00
Done. 20 min. Not bad.


# Reference:
Original paper: https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/
- https://github.com/graphdeco-inria/gaussian-splatting
Brush: 
- https://www.youtube.com/watch?v=VM6trBcPlaU
- https://github.com/ArthurBrussee/brush
nerfstudio: https://docs.nerf.studio/nerfology/methods/splat.html
Efficient Spherical Harmonic Evaluation: https://jcgt.org/published/0002/02/06/
SH related:
- [Plenoxels: Radiance Fields Without Neural Networks](https://openaccess.thecvf.com/content/CVPR2022/html/Fridovich-Keil_Plenoxels_Radiance_Fields_Without_Neural_Networks_CVPR_2022_paper.html)
	- https://alexyu.net/plenoxels/
	- wow this is almost 3DGS![[Pasted image 20250417233120.png]]
	- trilinear interp is important
- [Instant neural graphics primitives with a multiresolution hash encoding](https://dl.acm.org/doi/10.1145/3528223.3530127)
















