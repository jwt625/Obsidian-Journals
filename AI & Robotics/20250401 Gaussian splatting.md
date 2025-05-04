
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

Code:
- gradio: `/opt/homebrew/anaconda3/envs/colmap_env/bin/python colmap_gradio.py`


## 20250413 Coherent laser
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

## 20240420 spectra physics navigator J40-X30SC
/Documents/3DGS/20250420_spectra_physics_navigator_J40-X30SC
2025-04-20T00:17:43-07:00
- Started colmap 1 min ago.
- 2025-04-20T00:42:02-07:00 done 10 min ago.
```
=== Image Preparation ===
No scaling selected. Using original images.

=== Running COLMAP Reconstruction ===
Workspace: /Users/wentaojiang/Documents/3DGS/20250420_spectra_physics_navigator_J40-X30SC
=== Creating database ===
Running: colmap database_creator --database_path /Users/wentaojiang/Documents/3DGS/20250420_spectra_physics_navigator_J40-X30SC/database.db
=== Extracting features ===
Running: colmap feature_extractor --database_path /Users/wentaojiang/Documents/3DGS/20250420_spectra_physics_navigator_J40-X30SC/database.db --image_path /Users/wentaojiang/Documents/3DGS/20250420_spectra_physics_navigator_J40-X30SC/images
STDERR:
I20250420 00:16:52.168972 11872476 misc.cc:44] 
==============================================================================
Feature extraction
==============================================================================
I20250420 00:16:52.177546 11872489 sift.cc:721] Creating SIFT GPU feature extractor
I20250420 00:16:53.297879 11872490 feature_extraction.cc:258] Processed file [1/136]
I20250420 00:16:53.297920 11872490 feature_extraction.cc:261]   Name:            IMG_8594.jpeg
I20250420 00:16:53.297955 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:53.297969 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:53.297981 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:53.297994 11872490 feature_extraction.cc:280]   Features:        13453
I20250420 00:16:53.993916 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.865
I20250420 00:16:53.995951 11872490 feature_extraction.cc:258] Processed file [2/136]
I20250420 00:16:53.995973 11872490 feature_extraction.cc:261]   Name:            IMG_8595.jpeg
I20250420 00:16:53.995988 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:53.995999 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:53.996011 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:53.996021 11872490 feature_extraction.cc:280]   Features:        9472
I20250420 00:16:54.089807 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.631
I20250420 00:16:54.091382 11872490 feature_extraction.cc:258] Processed file [3/136]
I20250420 00:16:54.091405 11872490 feature_extraction.cc:261]   Name:            IMG_8596.jpeg
I20250420 00:16:54.091449 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:54.091485 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:54.091498 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:54.091511 11872490 feature_extraction.cc:280]   Features:        11635
I20250420 00:16:54.185135 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=33.928
I20250420 00:16:54.192840 11872490 feature_extraction.cc:258] Processed file [4/136]
I20250420 00:16:54.192866 11872490 feature_extraction.cc:261]   Name:            IMG_8597.jpeg
I20250420 00:16:54.192878 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:54.192905 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:54.192917 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:54.192929 11872490 feature_extraction.cc:280]   Features:        9795
I20250420 00:16:54.357376 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=33.501
I20250420 00:16:54.359956 11872490 feature_extraction.cc:258] Processed file [5/136]
I20250420 00:16:54.359982 11872490 feature_extraction.cc:261]   Name:            IMG_8598.jpeg
I20250420 00:16:54.360009 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:54.360023 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:54.360033 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:54.360062 11872490 feature_extraction.cc:280]   Features:        10884
I20250420 00:16:54.540762 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=33.807
I20250420 00:16:54.541641 11872490 feature_extraction.cc:258] Processed file [6/136]
I20250420 00:16:54.541656 11872490 feature_extraction.cc:261]   Name:            IMG_8599.jpeg
I20250420 00:16:54.541666 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:54.541676 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:54.541688 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:54.541697 11872490 feature_extraction.cc:280]   Features:        10008
I20250420 00:16:54.635797 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.668
I20250420 00:16:54.638916 11872490 feature_extraction.cc:258] Processed file [7/136]
I20250420 00:16:54.638939 11872490 feature_extraction.cc:261]   Name:            IMG_8600.jpeg
I20250420 00:16:54.638950 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:54.638960 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:54.638970 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:54.638980 11872490 feature_extraction.cc:280]   Features:        6896
I20250420 00:16:54.731577 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.283
I20250420 00:16:54.732347 11872490 feature_extraction.cc:258] Processed file [8/136]
I20250420 00:16:54.732369 11872490 feature_extraction.cc:261]   Name:            IMG_8601.jpeg
I20250420 00:16:54.732381 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:54.732391 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:54.732400 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:54.732410 11872490 feature_extraction.cc:280]   Features:        9948
I20250420 00:16:54.823608 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.511
I20250420 00:16:54.824298 11872490 feature_extraction.cc:258] Processed file [9/136]
I20250420 00:16:54.824312 11872490 feature_extraction.cc:261]   Name:            IMG_8602.jpeg
I20250420 00:16:54.824321 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:54.824332 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:54.824342 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:54.824352 11872490 feature_extraction.cc:280]   Features:        8752
I20250420 00:16:54.923163 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.185
I20250420 00:16:54.923962 11872490 feature_extraction.cc:258] Processed file [10/136]
I20250420 00:16:54.923979 11872490 feature_extraction.cc:261]   Name:            IMG_8603.jpeg
I20250420 00:16:54.923991 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:54.924003 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:54.924014 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:54.924024 11872490 feature_extraction.cc:280]   Features:        12430
I20250420 00:16:55.015976 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.275
I20250420 00:16:55.020082 11872490 feature_extraction.cc:258] Processed file [11/136]
I20250420 00:16:55.020104 11872490 feature_extraction.cc:261]   Name:            IMG_8604.jpeg
I20250420 00:16:55.020115 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:55.020125 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:55.020136 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:55.020146 11872490 feature_extraction.cc:280]   Features:        10834
I20250420 00:16:55.115234 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.404
I20250420 00:16:55.116086 11872490 feature_extraction.cc:258] Processed file [12/136]
I20250420 00:16:55.116099 11872490 feature_extraction.cc:261]   Name:            IMG_8605.jpeg
I20250420 00:16:55.116111 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:55.116122 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:55.116133 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:55.116143 11872490 feature_extraction.cc:280]   Features:        12132
I20250420 00:16:55.206697 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.619
I20250420 00:16:55.207648 11872490 feature_extraction.cc:258] Processed file [13/136]
I20250420 00:16:55.207661 11872490 feature_extraction.cc:261]   Name:            IMG_8606.jpeg
I20250420 00:16:55.207670 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:55.207681 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:55.207692 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:55.207701 11872490 feature_extraction.cc:280]   Features:        10977
I20250420 00:16:55.303769 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.452
I20250420 00:16:55.306735 11872490 feature_extraction.cc:258] Processed file [14/136]
I20250420 00:16:55.306759 11872490 feature_extraction.cc:261]   Name:            IMG_8607.jpeg
I20250420 00:16:55.306773 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:55.306784 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:55.306795 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:55.306805 11872490 feature_extraction.cc:280]   Features:        10319
I20250420 00:16:55.400933 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.638
I20250420 00:16:55.401826 11872490 feature_extraction.cc:258] Processed file [15/136]
I20250420 00:16:55.401839 11872490 feature_extraction.cc:261]   Name:            IMG_8608.jpeg
I20250420 00:16:55.401849 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:55.401860 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:55.401871 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:55.401882 11872490 feature_extraction.cc:280]   Features:        9131
I20250420 00:16:55.501112 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.596
I20250420 00:16:55.501897 11872490 feature_extraction.cc:258] Processed file [16/136]
I20250420 00:16:55.501920 11872490 feature_extraction.cc:261]   Name:            IMG_8609.jpeg
I20250420 00:16:55.501933 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:55.501945 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:55.501955 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:55.501965 11872490 feature_extraction.cc:280]   Features:        10639
I20250420 00:16:55.595690 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.563
I20250420 00:16:55.598338 11872490 feature_extraction.cc:258] Processed file [17/136]
I20250420 00:16:55.598359 11872490 feature_extraction.cc:261]   Name:            IMG_8610.jpeg
I20250420 00:16:55.598372 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:55.598384 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:55.598395 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:55.598405 11872490 feature_extraction.cc:280]   Features:        12488
I20250420 00:16:55.692065 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.297
I20250420 00:16:55.692963 11872490 feature_extraction.cc:258] Processed file [18/136]
I20250420 00:16:55.692978 11872490 feature_extraction.cc:261]   Name:            IMG_8611.jpeg
I20250420 00:16:55.692991 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:55.693003 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:55.693014 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:55.693025 11872490 feature_extraction.cc:280]   Features:        12228
I20250420 00:16:55.784919 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.149
I20250420 00:16:55.785776 11872490 feature_extraction.cc:258] Processed file [19/136]
I20250420 00:16:55.785792 11872490 feature_extraction.cc:261]   Name:            IMG_8612.jpeg
I20250420 00:16:55.785809 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:55.785818 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:55.785830 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:55.785840 11872490 feature_extraction.cc:280]   Features:        11276
I20250420 00:16:55.876142 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.195
I20250420 00:16:55.879436 11872490 feature_extraction.cc:258] Processed file [20/136]
I20250420 00:16:55.879454 11872490 feature_extraction.cc:261]   Name:            IMG_8613.jpeg
I20250420 00:16:55.879467 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:55.879479 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:55.879490 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:55.879500 11872490 feature_extraction.cc:280]   Features:        9506
I20250420 00:16:55.970594 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.209
I20250420 00:16:55.971386 11872490 feature_extraction.cc:258] Processed file [21/136]
I20250420 00:16:55.971402 11872490 feature_extraction.cc:261]   Name:            IMG_8614.jpeg
I20250420 00:16:55.971414 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:55.971426 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:55.971437 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:55.971447 11872490 feature_extraction.cc:280]   Features:        11164
I20250420 00:16:56.061314 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.493
I20250420 00:16:56.062354 11872490 feature_extraction.cc:258] Processed file [22/136]
I20250420 00:16:56.062368 11872490 feature_extraction.cc:261]   Name:            IMG_8615.jpeg
I20250420 00:16:56.062381 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:56.062392 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:56.062404 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:56.062415 11872490 feature_extraction.cc:280]   Features:        9274
I20250420 00:16:56.153977 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.492
I20250420 00:16:56.157881 11872490 feature_extraction.cc:258] Processed file [23/136]
I20250420 00:16:56.157900 11872490 feature_extraction.cc:261]   Name:            IMG_8616.jpeg
I20250420 00:16:56.157913 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:56.157923 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:56.157934 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:56.157944 11872490 feature_extraction.cc:280]   Features:        9340
I20250420 00:16:56.251206 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.435
I20250420 00:16:56.252115 11872490 feature_extraction.cc:258] Processed file [24/136]
I20250420 00:16:56.252133 11872490 feature_extraction.cc:261]   Name:            IMG_8617.jpeg
I20250420 00:16:56.252146 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:56.252157 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:56.252168 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:56.252179 11872490 feature_extraction.cc:280]   Features:        12088
I20250420 00:16:56.344744 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.385
I20250420 00:16:56.345547 11872490 feature_extraction.cc:258] Processed file [25/136]
I20250420 00:16:56.345561 11872490 feature_extraction.cc:261]   Name:            IMG_8618.jpeg
I20250420 00:16:56.345573 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:56.345587 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:56.345597 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:56.345607 11872490 feature_extraction.cc:280]   Features:        9176
I20250420 00:16:56.439603 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.348
I20250420 00:16:56.442533 11872490 feature_extraction.cc:258] Processed file [26/136]
I20250420 00:16:56.442549 11872490 feature_extraction.cc:261]   Name:            IMG_8619.jpeg
I20250420 00:16:56.442562 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:56.442574 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:56.442585 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:56.442596 11872490 feature_extraction.cc:280]   Features:        12890
I20250420 00:16:56.534322 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.295
I20250420 00:16:56.535454 11872490 feature_extraction.cc:258] Processed file [27/136]
I20250420 00:16:56.535472 11872490 feature_extraction.cc:261]   Name:            IMG_8620.jpeg
I20250420 00:16:56.535483 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:56.535493 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:56.535504 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:56.535516 11872490 feature_extraction.cc:280]   Features:        9032
I20250420 00:16:56.671397 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.367
I20250420 00:16:56.673383 11872490 feature_extraction.cc:258] Processed file [28/136]
I20250420 00:16:56.673413 11872490 feature_extraction.cc:261]   Name:            IMG_8621.jpeg
I20250420 00:16:56.673426 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:56.673436 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:56.673491 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:56.673532 11872490 feature_extraction.cc:280]   Features:        11919
I20250420 00:16:56.770747 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.487
I20250420 00:16:56.773741 11872490 feature_extraction.cc:258] Processed file [29/136]
I20250420 00:16:56.773770 11872490 feature_extraction.cc:261]   Name:            IMG_8622.jpeg
I20250420 00:16:56.773780 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:56.773790 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:56.773800 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:56.773810 11872490 feature_extraction.cc:280]   Features:        10412
I20250420 00:16:56.866235 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.285
I20250420 00:16:56.867026 11872490 feature_extraction.cc:258] Processed file [30/136]
I20250420 00:16:56.867040 11872490 feature_extraction.cc:261]   Name:            IMG_8623.jpeg
I20250420 00:16:56.867053 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:56.867065 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:56.867077 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:56.867087 11872490 feature_extraction.cc:280]   Features:        9216
I20250420 00:16:56.970721 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.340
I20250420 00:16:56.971541 11872490 feature_extraction.cc:258] Processed file [31/136]
I20250420 00:16:56.971556 11872490 feature_extraction.cc:261]   Name:            IMG_8624.jpeg
I20250420 00:16:56.971567 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:56.971578 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:56.971588 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:56.971598 11872490 feature_extraction.cc:280]   Features:        10512
I20250420 00:16:57.067600 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.297
I20250420 00:16:57.070307 11872490 feature_extraction.cc:258] Processed file [32/136]
I20250420 00:16:57.070326 11872490 feature_extraction.cc:261]   Name:            IMG_8625.jpeg
I20250420 00:16:57.070336 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:57.070346 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:57.070356 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:57.070367 11872490 feature_extraction.cc:280]   Features:        8947
I20250420 00:16:57.163838 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.544
I20250420 00:16:57.164777 11872490 feature_extraction.cc:258] Processed file [33/136]
I20250420 00:16:57.164794 11872490 feature_extraction.cc:261]   Name:            IMG_8626.jpeg
I20250420 00:16:57.164806 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:57.164819 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:57.164830 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:57.164840 11872490 feature_extraction.cc:280]   Features:        10112
I20250420 00:16:57.258777 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.533
I20250420 00:16:57.259748 11872490 feature_extraction.cc:258] Processed file [34/136]
I20250420 00:16:57.259765 11872490 feature_extraction.cc:261]   Name:            IMG_8627.jpeg
I20250420 00:16:57.259778 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:57.259789 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:57.259800 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:57.259811 11872490 feature_extraction.cc:280]   Features:        8542
I20250420 00:16:57.354664 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.343
I20250420 00:16:57.357061 11872490 feature_extraction.cc:258] Processed file [35/136]
I20250420 00:16:57.357079 11872490 feature_extraction.cc:261]   Name:            IMG_8628.jpeg
I20250420 00:16:57.357091 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:57.357103 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:57.357115 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:57.357125 11872490 feature_extraction.cc:280]   Features:        10761
I20250420 00:16:57.452523 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.363
I20250420 00:16:57.453531 11872490 feature_extraction.cc:258] Processed file [36/136]
I20250420 00:16:57.453545 11872490 feature_extraction.cc:261]   Name:            IMG_8629.jpeg
I20250420 00:16:57.453557 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:57.453569 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:57.453581 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:57.453593 11872490 feature_extraction.cc:280]   Features:        8261
I20250420 00:16:57.551128 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.467
I20250420 00:16:57.551776 11872490 feature_extraction.cc:258] Processed file [37/136]
I20250420 00:16:57.551792 11872490 feature_extraction.cc:261]   Name:            IMG_8630.jpeg
I20250420 00:16:57.551805 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:57.551817 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:57.551827 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:57.551838 11872490 feature_extraction.cc:280]   Features:        10727
I20250420 00:16:57.643064 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.511
I20250420 00:16:57.645550 11872490 feature_extraction.cc:258] Processed file [38/136]
I20250420 00:16:57.645567 11872490 feature_extraction.cc:261]   Name:            IMG_8631.jpeg
I20250420 00:16:57.645581 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:57.645592 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:57.645604 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:57.645615 11872490 feature_extraction.cc:280]   Features:        8402
I20250420 00:16:57.737655 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.438
I20250420 00:16:57.738413 11872490 feature_extraction.cc:258] Processed file [39/136]
I20250420 00:16:57.738432 11872490 feature_extraction.cc:261]   Name:            IMG_8632.jpeg
I20250420 00:16:57.738445 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:57.738457 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:57.738468 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:57.738479 11872490 feature_extraction.cc:280]   Features:        10960
I20250420 00:16:57.830781 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.307
I20250420 00:16:57.831586 11872490 feature_extraction.cc:258] Processed file [40/136]
I20250420 00:16:57.831600 11872490 feature_extraction.cc:261]   Name:            IMG_8633.jpeg
I20250420 00:16:57.831612 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:57.831625 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:57.831636 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:57.831647 11872490 feature_extraction.cc:280]   Features:        9195
I20250420 00:16:57.924361 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.273
I20250420 00:16:57.926764 11872490 feature_extraction.cc:258] Processed file [41/136]
I20250420 00:16:57.926782 11872490 feature_extraction.cc:261]   Name:            IMG_8634.jpeg
I20250420 00:16:57.926792 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:57.926802 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:57.926812 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:57.926823 11872490 feature_extraction.cc:280]   Features:        8491
I20250420 00:16:58.019225 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.284
I20250420 00:16:58.020054 11872490 feature_extraction.cc:258] Processed file [42/136]
I20250420 00:16:58.020073 11872490 feature_extraction.cc:261]   Name:            IMG_8635.jpeg
I20250420 00:16:58.020087 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:58.020098 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:58.020108 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:58.020118 11872490 feature_extraction.cc:280]   Features:        12337
I20250420 00:16:58.112716 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.351
I20250420 00:16:58.113935 11872490 feature_extraction.cc:258] Processed file [43/136]
I20250420 00:16:58.113948 11872490 feature_extraction.cc:261]   Name:            IMG_8636.jpeg
I20250420 00:16:58.113967 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:58.113979 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:58.113991 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:58.114001 11872490 feature_extraction.cc:280]   Features:        10306
I20250420 00:16:58.205483 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.568
I20250420 00:16:58.208043 11872490 feature_extraction.cc:258] Processed file [44/136]
I20250420 00:16:58.208063 11872490 feature_extraction.cc:261]   Name:            IMG_8637.jpeg
I20250420 00:16:58.208073 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:58.208083 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:58.208093 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:58.208103 11872490 feature_extraction.cc:280]   Features:        10974
I20250420 00:16:58.300313 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.541
I20250420 00:16:58.301285 11872490 feature_extraction.cc:258] Processed file [45/136]
I20250420 00:16:58.301306 11872490 feature_extraction.cc:261]   Name:            IMG_8638.jpeg
I20250420 00:16:58.301318 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:58.301330 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:58.301342 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:58.301352 11872490 feature_extraction.cc:280]   Features:        10312
I20250420 00:16:58.393951 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.516
I20250420 00:16:58.394711 11872490 feature_extraction.cc:258] Processed file [46/136]
I20250420 00:16:58.394726 11872490 feature_extraction.cc:261]   Name:            IMG_8639.jpeg
I20250420 00:16:58.394735 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:58.394745 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:58.394755 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:58.394765 11872490 feature_extraction.cc:280]   Features:        10274
I20250420 00:16:58.487221 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.471
I20250420 00:16:58.489897 11872490 feature_extraction.cc:258] Processed file [47/136]
I20250420 00:16:58.489913 11872490 feature_extraction.cc:261]   Name:            IMG_8640.jpeg
I20250420 00:16:58.489926 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:58.489938 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:58.489948 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:58.489959 11872490 feature_extraction.cc:280]   Features:        10821
I20250420 00:16:58.584443 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.391
I20250420 00:16:58.585350 11872490 feature_extraction.cc:258] Processed file [48/136]
I20250420 00:16:58.585366 11872490 feature_extraction.cc:261]   Name:            IMG_8641.jpeg
I20250420 00:16:58.585376 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:58.585386 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:58.585397 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:58.585407 11872490 feature_extraction.cc:280]   Features:        8231
I20250420 00:16:58.675230 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.358
I20250420 00:16:58.676102 11872490 feature_extraction.cc:258] Processed file [49/136]
I20250420 00:16:58.676123 11872490 feature_extraction.cc:261]   Name:            IMG_8642.jpeg
I20250420 00:16:58.676137 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:58.676148 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:58.676159 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:58.676169 11872490 feature_extraction.cc:280]   Features:        11272
I20250420 00:16:58.769450 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.540
I20250420 00:16:58.773540 11872490 feature_extraction.cc:258] Processed file [50/136]
I20250420 00:16:58.773558 11872490 feature_extraction.cc:261]   Name:            IMG_8643.jpeg
I20250420 00:16:58.773571 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:58.773582 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:58.773593 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:58.773603 11872490 feature_extraction.cc:280]   Features:        10194
I20250420 00:16:58.867497 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.481
I20250420 00:16:58.868491 11872490 feature_extraction.cc:258] Processed file [51/136]
I20250420 00:16:58.868506 11872490 feature_extraction.cc:261]   Name:            IMG_8644.jpeg
I20250420 00:16:58.868518 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:58.868530 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:58.868542 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:58.868554 11872490 feature_extraction.cc:280]   Features:        8902
I20250420 00:16:58.961107 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.390
I20250420 00:16:58.961834 11872490 feature_extraction.cc:258] Processed file [52/136]
I20250420 00:16:58.961850 11872490 feature_extraction.cc:261]   Name:            IMG_8645.jpeg
I20250420 00:16:58.961860 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:58.961870 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:58.961880 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:58.961891 11872490 feature_extraction.cc:280]   Features:        11602
I20250420 00:16:59.055281 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.470
I20250420 00:16:59.058074 11872490 feature_extraction.cc:258] Processed file [53/136]
I20250420 00:16:59.058097 11872490 feature_extraction.cc:261]   Name:            IMG_8646.jpeg
I20250420 00:16:59.058110 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:59.058122 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:59.058132 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:59.058143 11872490 feature_extraction.cc:280]   Features:        9492
I20250420 00:16:59.151674 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.516
I20250420 00:16:59.152448 11872490 feature_extraction.cc:258] Processed file [54/136]
I20250420 00:16:59.152465 11872490 feature_extraction.cc:261]   Name:            IMG_8647.jpeg
I20250420 00:16:59.152477 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:59.152489 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:59.152500 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:59.152511 11872490 feature_extraction.cc:280]   Features:        8665
I20250420 00:16:59.244899 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.490
I20250420 00:16:59.245640 11872490 feature_extraction.cc:258] Processed file [55/136]
I20250420 00:16:59.245657 11872490 feature_extraction.cc:261]   Name:            IMG_8648.jpeg
I20250420 00:16:59.245671 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:59.245682 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:59.245692 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:59.245705 11872490 feature_extraction.cc:280]   Features:        8353
I20250420 00:16:59.335225 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.425
I20250420 00:16:59.337606 11872490 feature_extraction.cc:258] Processed file [56/136]
I20250420 00:16:59.337626 11872490 feature_extraction.cc:261]   Name:            IMG_8649.jpeg
I20250420 00:16:59.337636 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:59.337646 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:59.337657 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:59.337669 11872490 feature_extraction.cc:280]   Features:        9609
I20250420 00:16:59.432134 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.422
I20250420 00:16:59.432929 11872490 feature_extraction.cc:258] Processed file [57/136]
I20250420 00:16:59.432945 11872490 feature_extraction.cc:261]   Name:            IMG_8650.jpeg
I20250420 00:16:59.432955 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:59.432965 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:59.432976 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:59.432986 11872490 feature_extraction.cc:280]   Features:        8471
I20250420 00:16:59.526497 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.398
I20250420 00:16:59.527237 11872490 feature_extraction.cc:258] Processed file [58/136]
I20250420 00:16:59.527252 11872490 feature_extraction.cc:261]   Name:            IMG_8651.jpeg
I20250420 00:16:59.527264 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:59.527277 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:59.527288 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:59.527299 11872490 feature_extraction.cc:280]   Features:        8803
I20250420 00:16:59.621892 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.463
I20250420 00:16:59.624774 11872490 feature_extraction.cc:258] Processed file [59/136]
I20250420 00:16:59.624791 11872490 feature_extraction.cc:261]   Name:            IMG_8652.jpeg
I20250420 00:16:59.624804 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:59.624816 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:59.624826 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:59.624837 11872490 feature_extraction.cc:280]   Features:        9556
I20250420 00:16:59.717697 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.403
I20250420 00:16:59.718955 11872490 feature_extraction.cc:258] Processed file [60/136]
I20250420 00:16:59.718972 11872490 feature_extraction.cc:261]   Name:            IMG_8653.jpeg
I20250420 00:16:59.718982 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:59.718992 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:59.719002 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:59.719013 11872490 feature_extraction.cc:280]   Features:        12592
I20250420 00:16:59.810400 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.403
I20250420 00:16:59.812013 11872490 feature_extraction.cc:258] Processed file [61/136]
I20250420 00:16:59.812028 11872490 feature_extraction.cc:261]   Name:            IMG_8654.jpeg
I20250420 00:16:59.812040 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:59.812053 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:59.812064 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:59.812075 11872490 feature_extraction.cc:280]   Features:        8218
I20250420 00:16:59.908883 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.390
I20250420 00:16:59.911285 11872490 feature_extraction.cc:258] Processed file [62/136]
I20250420 00:16:59.911307 11872490 feature_extraction.cc:261]   Name:            IMG_8655.jpeg
I20250420 00:16:59.911320 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:16:59.911331 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:16:59.911341 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:16:59.911352 11872490 feature_extraction.cc:280]   Features:        9470
I20250420 00:17:00.003895 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.399
I20250420 00:17:00.005187 11872490 feature_extraction.cc:258] Processed file [63/136]
I20250420 00:17:00.005205 11872490 feature_extraction.cc:261]   Name:            IMG_8656.jpeg
I20250420 00:17:00.005218 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:00.005230 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:00.005241 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:00.005252 11872490 feature_extraction.cc:280]   Features:        9540
I20250420 00:17:00.099016 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.383
I20250420 00:17:00.100353 11872490 feature_extraction.cc:258] Processed file [64/136]
I20250420 00:17:00.100368 11872490 feature_extraction.cc:261]   Name:            IMG_8657.jpeg
I20250420 00:17:00.100380 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:00.100392 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:00.100404 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:00.100414 11872490 feature_extraction.cc:280]   Features:        10410
I20250420 00:17:00.191351 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.390
I20250420 00:17:00.194545 11872490 feature_extraction.cc:258] Processed file [65/136]
I20250420 00:17:00.194567 11872490 feature_extraction.cc:261]   Name:            IMG_8658.jpeg
I20250420 00:17:00.194581 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:00.194591 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:00.194602 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:00.194612 11872490 feature_extraction.cc:280]   Features:        8318
I20250420 00:17:00.291404 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.390
I20250420 00:17:00.292630 11872490 feature_extraction.cc:258] Processed file [66/136]
I20250420 00:17:00.292678 11872490 feature_extraction.cc:261]   Name:            IMG_8659.jpeg
I20250420 00:17:00.292702 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:00.292724 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:00.292816 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:00.292840 11872490 feature_extraction.cc:280]   Features:        8268
I20250420 00:17:00.292981 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.460
I20250420 00:17:00.294062 11872490 feature_extraction.cc:258] Processed file [67/136]
I20250420 00:17:00.294111 11872490 feature_extraction.cc:261]   Name:            IMG_8660.jpeg
I20250420 00:17:00.294123 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:00.294135 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:00.294145 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:00.294157 11872490 feature_extraction.cc:280]   Features:        11039
I20250420 00:17:00.492272 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.463
I20250420 00:17:00.497786 11872490 feature_extraction.cc:258] Processed file [68/136]
I20250420 00:17:00.497810 11872490 feature_extraction.cc:261]   Name:            IMG_8661.jpeg
I20250420 00:17:00.497824 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:00.497835 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:00.497846 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:00.497857 11872490 feature_extraction.cc:280]   Features:        10576
I20250420 00:17:00.595362 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.468
I20250420 00:17:00.596365 11872490 feature_extraction.cc:258] Processed file [69/136]
I20250420 00:17:00.596383 11872490 feature_extraction.cc:261]   Name:            IMG_8662.jpeg
I20250420 00:17:00.596395 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:00.596407 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:00.596418 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:00.596429 11872490 feature_extraction.cc:280]   Features:        9176
I20250420 00:17:00.690711 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.468
I20250420 00:17:00.691579 11872490 feature_extraction.cc:258] Processed file [70/136]
I20250420 00:17:00.691596 11872490 feature_extraction.cc:261]   Name:            IMG_8663.jpeg
I20250420 00:17:00.691609 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:00.691622 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:00.691632 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:00.691643 11872490 feature_extraction.cc:280]   Features:        9599
I20250420 00:17:00.787200 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.475
I20250420 00:17:00.790657 11872490 feature_extraction.cc:258] Processed file [71/136]
I20250420 00:17:00.790678 11872490 feature_extraction.cc:261]   Name:            IMG_8664.jpeg
I20250420 00:17:00.790690 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:00.790701 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:00.790712 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:00.790722 11872490 feature_extraction.cc:280]   Features:        10639
I20250420 00:17:00.987654 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.381
I20250420 00:17:01.045886 11872490 feature_extraction.cc:258] Processed file [72/136]
I20250420 00:17:01.045945 11872490 feature_extraction.cc:261]   Name:            IMG_8665.jpeg
I20250420 00:17:01.045969 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:01.045991 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:01.046027 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:01.046051 11872490 feature_extraction.cc:280]   Features:        8323
I20250420 00:17:01.046130 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.345
I20250420 00:17:01.051540 11872490 feature_extraction.cc:258] Processed file [73/136]
I20250420 00:17:01.051746 11872490 feature_extraction.cc:261]   Name:            IMG_8666.jpeg
I20250420 00:17:01.051905 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:01.052037 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:01.052066 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:01.052141 11872490 feature_extraction.cc:280]   Features:        11035
I20250420 00:17:01.336587 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.558
I20250420 00:17:01.343427 11872490 feature_extraction.cc:258] Processed file [74/136]
I20250420 00:17:01.343464 11872490 feature_extraction.cc:261]   Name:            IMG_8667.jpeg
I20250420 00:17:01.343475 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:01.343485 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:01.343497 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:01.343506 11872490 feature_extraction.cc:280]   Features:        9202
I20250420 00:17:01.447609 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.536
I20250420 00:17:01.448829 11872490 feature_extraction.cc:258] Processed file [75/136]
I20250420 00:17:01.448854 11872490 feature_extraction.cc:261]   Name:            IMG_8668.jpeg
I20250420 00:17:01.448866 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:01.448876 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:01.448886 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:01.448913 11872490 feature_extraction.cc:280]   Features:        11585
I20250420 00:17:01.551820 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.407
I20250420 00:17:01.553323 11872490 feature_extraction.cc:258] Processed file [76/136]
I20250420 00:17:01.553342 11872490 feature_extraction.cc:261]   Name:            IMG_8669.jpeg
I20250420 00:17:01.553354 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:01.553364 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:01.553375 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:01.553387 11872490 feature_extraction.cc:280]   Features:        10277
I20250420 00:17:01.649958 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.330
I20250420 00:17:01.652726 11872490 feature_extraction.cc:258] Processed file [77/136]
I20250420 00:17:01.652752 11872490 feature_extraction.cc:261]   Name:            IMG_8670.jpeg
I20250420 00:17:01.652763 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:01.652773 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:01.652783 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:01.652794 11872490 feature_extraction.cc:280]   Features:        8891
I20250420 00:17:01.747660 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.330
I20250420 00:17:01.748399 11872490 feature_extraction.cc:258] Processed file [78/136]
I20250420 00:17:01.748414 11872490 feature_extraction.cc:261]   Name:            IMG_8671.jpeg
I20250420 00:17:01.748426 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:01.748438 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:01.748449 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:01.748461 11872490 feature_extraction.cc:280]   Features:        8990
I20250420 00:17:01.844540 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.316
I20250420 00:17:01.845643 11872490 feature_extraction.cc:258] Processed file [79/136]
I20250420 00:17:01.845659 11872490 feature_extraction.cc:261]   Name:            IMG_8672.jpeg
I20250420 00:17:01.845674 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:01.845686 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:01.845696 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:01.845707 11872490 feature_extraction.cc:280]   Features:        11516
I20250420 00:17:01.940023 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.323
I20250420 00:17:01.942570 11872490 feature_extraction.cc:258] Processed file [80/136]
I20250420 00:17:01.942587 11872490 feature_extraction.cc:261]   Name:            IMG_8673.jpeg
I20250420 00:17:01.942600 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:01.942612 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:01.942624 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:01.942660 11872490 feature_extraction.cc:280]   Features:        9879
I20250420 00:17:02.037684 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.319
I20250420 00:17:02.038728 11872490 feature_extraction.cc:258] Processed file [81/136]
I20250420 00:17:02.038748 11872490 feature_extraction.cc:261]   Name:            IMG_8674.jpeg
I20250420 00:17:02.038758 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:02.038767 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:02.038777 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:02.038793 11872490 feature_extraction.cc:280]   Features:        11245
I20250420 00:17:02.131580 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.337
I20250420 00:17:02.132504 11872490 feature_extraction.cc:258] Processed file [82/136]
I20250420 00:17:02.132522 11872490 feature_extraction.cc:261]   Name:            IMG_8675.jpeg
I20250420 00:17:02.132535 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:02.132546 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:02.132556 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:02.132566 11872490 feature_extraction.cc:280]   Features:        8483
I20250420 00:17:02.225314 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.241
I20250420 00:17:02.228019 11872490 feature_extraction.cc:258] Processed file [83/136]
I20250420 00:17:02.228041 11872490 feature_extraction.cc:261]   Name:            IMG_8676.jpeg
I20250420 00:17:02.228051 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:02.228061 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:02.228071 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:02.228081 11872490 feature_extraction.cc:280]   Features:        10015
I20250420 00:17:02.321830 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.392
I20250420 00:17:02.322669 11872490 feature_extraction.cc:258] Processed file [84/136]
I20250420 00:17:02.322685 11872490 feature_extraction.cc:261]   Name:            IMG_8677.jpeg
I20250420 00:17:02.322695 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:02.322703 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:02.322714 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:02.322724 11872490 feature_extraction.cc:280]   Features:        12006
I20250420 00:17:02.414053 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.454
I20250420 00:17:02.415254 11872490 feature_extraction.cc:258] Processed file [85/136]
I20250420 00:17:02.415268 11872490 feature_extraction.cc:261]   Name:            IMG_8678.jpeg
I20250420 00:17:02.415279 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:02.415292 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:02.415302 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:02.415313 11872490 feature_extraction.cc:280]   Features:        10988
I20250420 00:17:02.515794 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.454
I20250420 00:17:02.518996 11872490 feature_extraction.cc:258] Processed file [86/136]
I20250420 00:17:02.519016 11872490 feature_extraction.cc:261]   Name:            IMG_8679.jpeg
I20250420 00:17:02.519029 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:02.519040 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:02.519050 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:02.519062 11872490 feature_extraction.cc:280]   Features:        8457
I20250420 00:17:02.609781 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.441
I20250420 00:17:02.610728 11872490 feature_extraction.cc:258] Processed file [87/136]
I20250420 00:17:02.610743 11872490 feature_extraction.cc:261]   Name:            IMG_8680.jpeg
I20250420 00:17:02.610755 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:02.610769 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:02.610778 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:02.610790 11872490 feature_extraction.cc:280]   Features:        9911
I20250420 00:17:02.711926 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.441
I20250420 00:17:02.712709 11872490 feature_extraction.cc:258] Processed file [88/136]
I20250420 00:17:02.712723 11872490 feature_extraction.cc:261]   Name:            IMG_8681.jpeg
I20250420 00:17:02.712735 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:02.712749 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:02.712759 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:02.712769 11872490 feature_extraction.cc:280]   Features:        9541
I20250420 00:17:02.809401 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.511
I20250420 00:17:02.812053 11872490 feature_extraction.cc:258] Processed file [89/136]
I20250420 00:17:02.812074 11872490 feature_extraction.cc:261]   Name:            IMG_8682.jpeg
I20250420 00:17:02.812088 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:02.812098 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:02.812108 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:02.812119 11872490 feature_extraction.cc:280]   Features:        9227
I20250420 00:17:02.905292 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.467
I20250420 00:17:02.906422 11872490 feature_extraction.cc:258] Processed file [90/136]
I20250420 00:17:02.906440 11872490 feature_extraction.cc:261]   Name:            IMG_8683.jpeg
I20250420 00:17:02.906451 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:02.906463 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:02.906474 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:02.906486 11872490 feature_extraction.cc:280]   Features:        9893
I20250420 00:17:03.002621 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.271
I20250420 00:17:03.005438 11872490 feature_extraction.cc:258] Processed file [91/136]
I20250420 00:17:03.005460 11872490 feature_extraction.cc:261]   Name:            IMG_8684.jpeg
I20250420 00:17:03.005470 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:03.005481 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:03.005491 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:03.005501 11872490 feature_extraction.cc:280]   Features:        11271
I20250420 00:17:03.005519 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.271
I20250420 00:17:03.012425 11872490 feature_extraction.cc:258] Processed file [92/136]
I20250420 00:17:03.012460 11872490 feature_extraction.cc:261]   Name:            IMG_8685.jpeg
I20250420 00:17:03.012471 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:03.012481 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:03.012492 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:03.012502 11872490 feature_extraction.cc:280]   Features:        9782
I20250420 00:17:03.212869 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.335
I20250420 00:17:03.214322 11872490 feature_extraction.cc:258] Processed file [93/136]
I20250420 00:17:03.214337 11872490 feature_extraction.cc:261]   Name:            IMG_8686.jpeg
I20250420 00:17:03.214349 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:03.214360 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:03.214370 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:03.214380 11872490 feature_extraction.cc:280]   Features:        9404
I20250420 00:17:03.308288 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.255
I20250420 00:17:03.309172 11872490 feature_extraction.cc:258] Processed file [94/136]
I20250420 00:17:03.309190 11872490 feature_extraction.cc:261]   Name:            IMG_8687.jpeg
I20250420 00:17:03.309202 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:03.309212 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:03.309222 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:03.309232 11872490 feature_extraction.cc:280]   Features:        8905
I20250420 00:17:03.403357 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.305
I20250420 00:17:03.405642 11872490 feature_extraction.cc:258] Processed file [95/136]
I20250420 00:17:03.405658 11872490 feature_extraction.cc:261]   Name:            IMG_8688.jpeg
I20250420 00:17:03.405670 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:03.405683 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:03.405694 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:03.405704 11872490 feature_extraction.cc:280]   Features:        8711
I20250420 00:17:03.497613 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.306
I20250420 00:17:03.498477 11872490 feature_extraction.cc:258] Processed file [96/136]
I20250420 00:17:03.498492 11872490 feature_extraction.cc:261]   Name:            IMG_8689.jpeg
I20250420 00:17:03.498504 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:03.498517 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:03.498530 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:03.498540 11872490 feature_extraction.cc:280]   Features:        8253
I20250420 00:17:03.586306 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.306
I20250420 00:17:03.586927 11872490 feature_extraction.cc:258] Processed file [97/136]
I20250420 00:17:03.586949 11872490 feature_extraction.cc:261]   Name:            IMG_8690.jpeg
I20250420 00:17:03.586959 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:03.586969 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:03.586979 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:03.586989 11872490 feature_extraction.cc:280]   Features:        10319
I20250420 00:17:03.677911 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.306
I20250420 00:17:03.680444 11872490 feature_extraction.cc:258] Processed file [98/136]
I20250420 00:17:03.680465 11872490 feature_extraction.cc:261]   Name:            IMG_8691.jpeg
I20250420 00:17:03.680475 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:03.680485 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:03.680502 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:03.680516 11872490 feature_extraction.cc:280]   Features:        8830
I20250420 00:17:03.771736 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.299
I20250420 00:17:03.772454 11872490 feature_extraction.cc:258] Processed file [99/136]
I20250420 00:17:03.772473 11872490 feature_extraction.cc:261]   Name:            IMG_8692.jpeg
I20250420 00:17:03.772483 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:03.772495 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:03.772504 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:03.772516 11872490 feature_extraction.cc:280]   Features:        11591
I20250420 00:17:03.864637 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.299
I20250420 00:17:03.865697 11872490 feature_extraction.cc:258] Processed file [100/136]
I20250420 00:17:03.865713 11872490 feature_extraction.cc:261]   Name:            IMG_8693.jpeg
I20250420 00:17:03.865723 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:03.865733 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:03.865744 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:03.865756 11872490 feature_extraction.cc:280]   Features:        9797
I20250420 00:17:03.959172 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.257
I20250420 00:17:03.961582 11872490 feature_extraction.cc:258] Processed file [101/136]
I20250420 00:17:03.961601 11872490 feature_extraction.cc:261]   Name:            IMG_8694.jpeg
I20250420 00:17:03.961614 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:03.961627 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:03.961637 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:03.961647 11872490 feature_extraction.cc:280]   Features:        9890
I20250420 00:17:04.055256 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.307
I20250420 00:17:04.056327 11872490 feature_extraction.cc:258] Processed file [102/136]
I20250420 00:17:04.056349 11872490 feature_extraction.cc:261]   Name:            IMG_8695.jpeg
I20250420 00:17:04.056360 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:04.056370 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:04.056380 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:04.056391 11872490 feature_extraction.cc:280]   Features:        8195
I20250420 00:17:04.147838 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.313
I20250420 00:17:04.148459 11872490 feature_extraction.cc:258] Processed file [103/136]
I20250420 00:17:04.148475 11872490 feature_extraction.cc:261]   Name:            IMG_8696.jpeg
I20250420 00:17:04.148485 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:04.148494 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:04.148505 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:04.148516 11872490 feature_extraction.cc:280]   Features:        11259
I20250420 00:17:04.236776 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.334
I20250420 00:17:04.239473 11872490 feature_extraction.cc:258] Processed file [104/136]
I20250420 00:17:04.239490 11872490 feature_extraction.cc:261]   Name:            IMG_8697.jpeg
I20250420 00:17:04.239499 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:04.239511 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:04.239521 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:04.239532 11872490 feature_extraction.cc:280]   Features:        10701
I20250420 00:17:04.328841 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.334
I20250420 00:17:04.329843 11872490 feature_extraction.cc:258] Processed file [105/136]
I20250420 00:17:04.329859 11872490 feature_extraction.cc:261]   Name:            IMG_8698.jpeg
I20250420 00:17:04.329870 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:04.329881 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:04.329892 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:04.329903 11872490 feature_extraction.cc:280]   Features:        13009
I20250420 00:17:04.424489 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.438
I20250420 00:17:04.425843 11872490 feature_extraction.cc:258] Processed file [106/136]
I20250420 00:17:04.425860 11872490 feature_extraction.cc:261]   Name:            IMG_8699.jpeg
I20250420 00:17:04.425873 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:04.425885 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:04.425902 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:04.425915 11872490 feature_extraction.cc:280]   Features:        11794
I20250420 00:17:04.517556 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.289
I20250420 00:17:04.520641 11872490 feature_extraction.cc:258] Processed file [107/136]
I20250420 00:17:04.520660 11872490 feature_extraction.cc:261]   Name:            IMG_8700.jpeg
I20250420 00:17:04.520671 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:04.520680 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:04.520690 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:04.520702 11872490 feature_extraction.cc:280]   Features:        9491
I20250420 00:17:04.611449 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.285
I20250420 00:17:04.612404 11872490 feature_extraction.cc:258] Processed file [108/136]
I20250420 00:17:04.612425 11872490 feature_extraction.cc:261]   Name:            IMG_8701.jpeg
I20250420 00:17:04.612435 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:04.612445 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:04.612457 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:04.612468 11872490 feature_extraction.cc:280]   Features:        11102
I20250420 00:17:04.699896 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.344
I20250420 00:17:04.701278 11872490 feature_extraction.cc:258] Processed file [109/136]
I20250420 00:17:04.701294 11872490 feature_extraction.cc:261]   Name:            IMG_8702.jpeg
I20250420 00:17:04.701306 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:04.701319 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:04.701330 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:04.701341 11872490 feature_extraction.cc:280]   Features:        10120
I20250420 00:17:04.793166 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.269
I20250420 00:17:04.795953 11872490 feature_extraction.cc:258] Processed file [110/136]
I20250420 00:17:04.795971 11872490 feature_extraction.cc:261]   Name:            IMG_8703.jpeg
I20250420 00:17:04.795981 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:04.795991 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:04.796002 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:04.796012 11872490 feature_extraction.cc:280]   Features:        8590
I20250420 00:17:04.886389 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.303
I20250420 00:17:04.887523 11872490 feature_extraction.cc:258] Processed file [111/136]
I20250420 00:17:04.887542 11872490 feature_extraction.cc:261]   Name:            IMG_8704.jpeg
I20250420 00:17:04.887553 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:04.887565 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:04.887576 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:04.887588 11872490 feature_extraction.cc:280]   Features:        11018
I20250420 00:17:04.978215 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.301
I20250420 00:17:04.979218 11872490 feature_extraction.cc:258] Processed file [112/136]
I20250420 00:17:04.979231 11872490 feature_extraction.cc:261]   Name:            IMG_8705.jpeg
I20250420 00:17:04.979245 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:04.979259 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:04.979270 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:04.979280 11872490 feature_extraction.cc:280]   Features:        10481
I20250420 00:17:05.069729 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.301
I20250420 00:17:05.072337 11872490 feature_extraction.cc:258] Processed file [113/136]
I20250420 00:17:05.072358 11872490 feature_extraction.cc:261]   Name:            IMG_8706.jpeg
I20250420 00:17:05.072368 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:05.072379 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:05.072389 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:05.072400 11872490 feature_extraction.cc:280]   Features:        11120
I20250420 00:17:05.163182 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.301
I20250420 00:17:05.164139 11872490 feature_extraction.cc:258] Processed file [114/136]
I20250420 00:17:05.164153 11872490 feature_extraction.cc:261]   Name:            IMG_8707.jpeg
I20250420 00:17:05.164165 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:05.164178 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:05.164191 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:05.164201 11872490 feature_extraction.cc:280]   Features:        10368
I20250420 00:17:05.255224 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.301
I20250420 00:17:05.256121 11872490 feature_extraction.cc:258] Processed file [115/136]
I20250420 00:17:05.256135 11872490 feature_extraction.cc:261]   Name:            IMG_8708.jpeg
I20250420 00:17:05.256148 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:05.256161 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:05.256172 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:05.256182 11872490 feature_extraction.cc:280]   Features:        9553
I20250420 00:17:05.349987 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.374
I20250420 00:17:05.352735 11872490 feature_extraction.cc:258] Processed file [116/136]
I20250420 00:17:05.352751 11872490 feature_extraction.cc:261]   Name:            IMG_8709.jpeg
I20250420 00:17:05.352764 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:05.352777 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:05.352787 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:05.352798 11872490 feature_extraction.cc:280]   Features:        12136
I20250420 00:17:05.449994 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.296
I20250420 00:17:05.451063 11872490 feature_extraction.cc:258] Processed file [117/136]
I20250420 00:17:05.451079 11872490 feature_extraction.cc:261]   Name:            IMG_8710.jpeg
I20250420 00:17:05.451092 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:05.451105 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:05.451117 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:05.451128 11872490 feature_extraction.cc:280]   Features:        13654
I20250420 00:17:05.542972 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.549
I20250420 00:17:05.544528 11872490 feature_extraction.cc:258] Processed file [118/136]
I20250420 00:17:05.544548 11872490 feature_extraction.cc:261]   Name:            IMG_8711.jpeg
I20250420 00:17:05.544557 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:05.544569 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:05.544579 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:05.544588 11872490 feature_extraction.cc:280]   Features:        11635
I20250420 00:17:05.638741 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.765
I20250420 00:17:05.642972 11872490 feature_extraction.cc:258] Processed file [119/136]
I20250420 00:17:05.642992 11872490 feature_extraction.cc:261]   Name:            IMG_8712.jpeg
I20250420 00:17:05.643004 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:05.643016 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:05.643026 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:05.643036 11872490 feature_extraction.cc:280]   Features:        8281
I20250420 00:17:05.735107 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=32.673
I20250420 00:17:05.735749 11872490 feature_extraction.cc:258] Processed file [120/136]
I20250420 00:17:05.735762 11872490 feature_extraction.cc:261]   Name:            IMG_8713.jpeg
I20250420 00:17:05.735772 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:05.735783 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:05.735795 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:05.735805 11872490 feature_extraction.cc:280]   Features:        8655
I20250420 00:17:05.735821 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=35.180
I20250420 00:17:05.736573 11872490 feature_extraction.cc:258] Processed file [121/136]
I20250420 00:17:05.736585 11872490 feature_extraction.cc:261]   Name:            IMG_8714.jpeg
I20250420 00:17:05.736595 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:05.736604 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:05.736614 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:05.736624 11872490 feature_extraction.cc:280]   Features:        8339
I20250420 00:17:05.736639 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=35.995
I20250420 00:17:05.810907 11872490 feature_extraction.cc:258] Processed file [122/136]
I20250420 00:17:05.810936 11872490 feature_extraction.cc:261]   Name:            IMG_8715.jpeg
I20250420 00:17:05.810947 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:05.810957 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:05.810967 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:05.810978 11872490 feature_extraction.cc:280]   Features:        9940
I20250420 00:17:05.811017 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.377
I20250420 00:17:05.885156 11872490 feature_extraction.cc:258] Processed file [123/136]
I20250420 00:17:05.885185 11872490 feature_extraction.cc:261]   Name:            IMG_8716.jpeg
I20250420 00:17:05.885195 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:05.885206 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:05.885216 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:05.885227 11872490 feature_extraction.cc:280]   Features:        8760
I20250420 00:17:05.885264 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.318
I20250420 00:17:05.960763 11872490 feature_extraction.cc:258] Processed file [124/136]
I20250420 00:17:05.960791 11872490 feature_extraction.cc:261]   Name:            IMG_8717.jpeg
I20250420 00:17:05.960804 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:05.960815 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:05.960825 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:05.960836 11872490 feature_extraction.cc:280]   Features:        10894
I20250420 00:17:05.960876 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.335
I20250420 00:17:06.038008 11872490 feature_extraction.cc:258] Processed file [125/136]
I20250420 00:17:06.038038 11872490 feature_extraction.cc:261]   Name:            IMG_8718.jpeg
I20250420 00:17:06.038049 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:06.038059 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:06.038070 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:06.038080 11872490 feature_extraction.cc:280]   Features:        10306
I20250420 00:17:06.038120 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.251
I20250420 00:17:06.113303 11872490 feature_extraction.cc:258] Processed file [126/136]
I20250420 00:17:06.113335 11872490 feature_extraction.cc:261]   Name:            IMG_8719.jpeg
I20250420 00:17:06.113346 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:06.113355 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:06.113365 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:06.113375 11872490 feature_extraction.cc:280]   Features:        8681
I20250420 00:17:06.113412 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.313
I20250420 00:17:06.186987 11872490 feature_extraction.cc:258] Processed file [127/136]
I20250420 00:17:06.187019 11872490 feature_extraction.cc:261]   Name:            IMG_8720.jpeg
I20250420 00:17:06.187031 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:06.187039 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:06.187049 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:06.187059 11872490 feature_extraction.cc:280]   Features:        9281
I20250420 00:17:06.187100 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.286
I20250420 00:17:06.259429 11872490 feature_extraction.cc:258] Processed file [128/136]
I20250420 00:17:06.259457 11872490 feature_extraction.cc:261]   Name:            IMG_8721.jpeg
I20250420 00:17:06.259469 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:06.259477 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:06.259491 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:06.259502 11872490 feature_extraction.cc:280]   Features:        10056
I20250420 00:17:06.259541 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.312
I20250420 00:17:06.332173 11872490 feature_extraction.cc:258] Processed file [129/136]
I20250420 00:17:06.332206 11872490 feature_extraction.cc:261]   Name:            IMG_8722.jpeg
I20250420 00:17:06.332218 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:06.332228 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:06.332238 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:06.332249 11872490 feature_extraction.cc:280]   Features:        8395
I20250420 00:17:06.332293 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.317
I20250420 00:17:06.407114 11872490 feature_extraction.cc:258] Processed file [130/136]
I20250420 00:17:06.407140 11872490 feature_extraction.cc:261]   Name:            IMG_8723.jpeg
I20250420 00:17:06.407150 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:06.407160 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:06.407171 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:06.407181 11872490 feature_extraction.cc:280]   Features:        10306
I20250420 00:17:06.407222 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.325
I20250420 00:17:06.480683 11872490 feature_extraction.cc:258] Processed file [131/136]
I20250420 00:17:06.480706 11872490 feature_extraction.cc:261]   Name:            IMG_8724.jpeg
I20250420 00:17:06.480719 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:06.480729 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:06.480742 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:06.480755 11872490 feature_extraction.cc:280]   Features:        11327
I20250420 00:17:06.480795 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.298
I20250420 00:17:06.552706 11872490 feature_extraction.cc:258] Processed file [132/136]
I20250420 00:17:06.552743 11872490 feature_extraction.cc:261]   Name:            IMG_8725.jpeg
I20250420 00:17:06.552757 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:06.552767 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:06.552779 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:06.552793 11872490 feature_extraction.cc:280]   Features:        8579
I20250420 00:17:06.552835 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.324
I20250420 00:17:06.625105 11872490 feature_extraction.cc:258] Processed file [133/136]
I20250420 00:17:06.625131 11872490 feature_extraction.cc:261]   Name:            IMG_8726.jpeg
I20250420 00:17:06.625142 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:06.625152 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:06.625162 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:06.625171 11872490 feature_extraction.cc:280]   Features:        8855
I20250420 00:17:06.625208 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.269
I20250420 00:17:06.700945 11872490 feature_extraction.cc:258] Processed file [134/136]
I20250420 00:17:06.700976 11872490 feature_extraction.cc:261]   Name:            IMG_8727.jpeg
I20250420 00:17:06.700987 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:06.700996 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:06.701006 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:06.701017 11872490 feature_extraction.cc:280]   Features:        11080
I20250420 00:17:06.701056 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.276
I20250420 00:17:06.773897 11872490 feature_extraction.cc:258] Processed file [135/136]
I20250420 00:17:06.773927 11872490 feature_extraction.cc:261]   Name:            IMG_8728.jpeg
I20250420 00:17:06.773939 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:06.773952 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:06.773962 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:06.773973 11872490 feature_extraction.cc:280]   Features:        11196
I20250420 00:17:06.774017 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.208
I20250420 00:17:06.848712 11872490 feature_extraction.cc:258] Processed file [136/136]
I20250420 00:17:06.848737 11872490 feature_extraction.cc:261]   Name:            IMG_8729.jpeg
I20250420 00:17:06.848748 11872490 feature_extraction.cc:270]   Dimensions:      5712 x 4284
I20250420 00:17:06.848760 11872490 feature_extraction.cc:273]   Camera:          #1 - SIMPLE_RADIAL
I20250420 00:17:06.848771 11872490 feature_extraction.cc:276]   Focal Length:    4243.20px (Prior)
I20250420 00:17:06.848784 11872490 feature_extraction.cc:280]   Features:        8770
I20250420 00:17:06.848827 11872490 feature_extraction.cc:288]   GPS:             LAT=37.358, LON=-122.013, ALT=34.299
I20250420 00:17:06.866503 11872476 timer.cc:91] Elapsed time: 0.245 [minutes]

=== Running Exhaustive matching ===
Running: colmap exhaustive_matcher --database_path /Users/wentaojiang/Documents/3DGS/20250420_spectra_physics_navigator_J40-X30SC/database.db
STDERR:
I20250420 00:17:07.098105 11873516 misc.cc:44] 
==============================================================================
Feature matching
==============================================================================
I20250420 00:17:07.104872 11873517 sift.cc:1426] Creating SIFT GPU feature matcher
I20250420 00:17:07.126511 11873516 pairing.cc:168] Generating exhaustive image pairs...
I20250420 00:17:07.126530 11873516 pairing.cc:201] Matching block [1/3, 1/3]
I20250420 00:18:43.803871 11873516 feature_matching.cc:46] in 96.677s
I20250420 00:18:43.809424 11873516 pairing.cc:201] Matching block [1/3, 2/3]
I20250420 00:20:53.419618 11873516 feature_matching.cc:46] in 129.610s
I20250420 00:20:53.421128 11873516 pairing.cc:201] Matching block [1/3, 3/3]
I20250420 00:22:03.640733 11873516 feature_matching.cc:46] in 70.220s
I20250420 00:22:03.644526 11873516 pairing.cc:201] Matching block [2/3, 1/3]
I20250420 00:23:41.598788 11873516 feature_matching.cc:46] in 97.954s
I20250420 00:23:41.612107 11873516 pairing.cc:201] Matching block [2/3, 2/3]
I20250420 00:25:36.170377 11873516 feature_matching.cc:46] in 114.543s
I20250420 00:25:36.184890 11873516 pairing.cc:201] Matching block [2/3, 3/3]
I20250420 00:26:39.182360 11873516 feature_matching.cc:46] in 62.997s
I20250420 00:26:39.189152 11873516 pairing.cc:201] Matching block [3/3, 1/3]
I20250420 00:28:26.541958 11873516 feature_matching.cc:46] in 107.352s
I20250420 00:28:26.555058 11873516 pairing.cc:201] Matching block [3/3, 2/3]
I20250420 00:30:19.116237 11873516 feature_matching.cc:46] in 112.561s
I20250420 00:30:19.138893 11873516 pairing.cc:201] Matching block [3/3, 3/3]
I20250420 00:31:34.421595 11873516 feature_matching.cc:46] in 75.282s
I20250420 00:31:34.425029 11873516 timer.cc:91] Elapsed time: 14.455 [minutes]

=== Running sparse reconstruction ===
Running: colmap mapper --database_path /Users/wentaojiang/Documents/3DGS/20250420_spectra_physics_navigator_J40-X30SC/database.db --image_path /Users/wentaojiang/Documents/3DGS/20250420_spectra_physics_navigator_J40-X30SC/images --output_path /Users/wentaojiang/Documents/3DGS/20250420_spectra_physics_navigator_J40-X30SC/sparse
STDERR:
I20250420 00:31:34.573889 11898994 incremental_pipeline.cc:237] Loading database
I20250420 00:31:34.575593 11898994 database_cache.cc:66] Loading cameras...
I20250420 00:31:34.575626 11898994 database_cache.cc:76]  1 in 0.000s
I20250420 00:31:34.575640 11898994 database_cache.cc:84] Loading matches...
I20250420 00:31:34.586555 11898994 database_cache.cc:89]  1232 in 0.011s
I20250420 00:31:34.586583 11898994 database_cache.cc:105] Loading images...
I20250420 00:31:34.635280 11898994 database_cache.cc:153]  136 in 0.049s (connected 136)
I20250420 00:31:34.635324 11898994 database_cache.cc:164] Loading pose priors...
I20250420 00:31:34.635644 11898994 database_cache.cc:175]  136 in 0.000s
I20250420 00:31:34.635658 11898994 database_cache.cc:184] Building correspondence graph...
I20250420 00:31:34.657325 11898994 database_cache.cc:210]  in 0.022s (ignored 0)
I20250420 00:31:34.657533 11898994 timer.cc:91] Elapsed time: 0.001 [minutes]
I20250420 00:31:34.662870 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:34.696327 11898994 incremental_pipeline.cc:306] Initializing with image pair #22 and #19
I20250420 00:31:34.696625 11898994 incremental_pipeline.cc:311] Global bundle adjustment
I20250420 00:31:34.782476 11898994 incremental_pipeline.cc:390] Registering image #20 (3)
I20250420 00:31:34.782507 11898994 incremental_pipeline.cc:393] => Image sees 67 / 2073 points
I20250420 00:31:34.805430 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:34.888029 11898994 incremental_pipeline.cc:390] Registering image #15 (4)
I20250420 00:31:34.888056 11898994 incremental_pipeline.cc:393] => Image sees 233 / 2178 points
I20250420 00:31:34.946431 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:35.039098 11898994 incremental_pipeline.cc:390] Registering image #28 (5)
I20250420 00:31:35.039124 11898994 incremental_pipeline.cc:393] => Image sees 446 / 2603 points
I20250420 00:31:35.122292 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:35.238327 11898994 incremental_pipeline.cc:390] Registering image #89 (6)
I20250420 00:31:35.238363 11898994 incremental_pipeline.cc:393] => Image sees 444 / 2126 points
I20250420 00:31:35.324120 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:35.431313 11898994 incremental_pipeline.cc:390] Registering image #105 (7)
I20250420 00:31:35.431345 11898994 incremental_pipeline.cc:393] => Image sees 378 / 2296 points
I20250420 00:31:35.512048 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:35.622654 11898994 incremental_pipeline.cc:390] Registering image #31 (8)
I20250420 00:31:35.622682 11898994 incremental_pipeline.cc:393] => Image sees 339 / 1365 points
I20250420 00:31:35.690127 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:35.834167 11898994 incremental_pipeline.cc:390] Registering image #32 (9)
I20250420 00:31:35.834194 11898994 incremental_pipeline.cc:393] => Image sees 167 / 1662 points
I20250420 00:31:35.900879 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:36.019402 11898994 incremental_pipeline.cc:390] Registering image #88 (10)
I20250420 00:31:36.019428 11898994 incremental_pipeline.cc:393] => Image sees 184 / 1878 points
I20250420 00:31:36.074894 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:36.193701 11898994 incremental_pipeline.cc:390] Registering image #85 (11)
I20250420 00:31:36.193727 11898994 incremental_pipeline.cc:393] => Image sees 394 / 1757 points
I20250420 00:31:36.249779 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:36.393543 11898994 incremental_pipeline.cc:390] Registering image #63 (12)
I20250420 00:31:36.393568 11898994 incremental_pipeline.cc:393] => Image sees 162 / 1002 points
I20250420 00:31:36.515771 11898994 incremental_pipeline.cc:390] Registering image #75 (13)
I20250420 00:31:36.515797 11898994 incremental_pipeline.cc:393] => Image sees 171 / 1348 points
I20250420 00:31:36.560508 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:36.680227 11898994 incremental_pipeline.cc:390] Registering image #18 (14)
I20250420 00:31:36.680258 11898994 incremental_pipeline.cc:393] => Image sees 168 / 862 points
I20250420 00:31:36.802793 11898994 incremental_pipeline.cc:390] Registering image #90 (15)
I20250420 00:31:36.802822 11898994 incremental_pipeline.cc:393] => Image sees 110 / 751 points
I20250420 00:31:36.839527 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:36.955233 11898994 incremental_pipeline.cc:390] Registering image #94 (16)
I20250420 00:31:36.955257 11898994 incremental_pipeline.cc:393] => Image sees 108 / 1015 points
I20250420 00:31:37.091962 11898994 incremental_pipeline.cc:390] Registering image #104 (17)
I20250420 00:31:37.091986 11898994 incremental_pipeline.cc:393] => Image sees 125 / 1374 points
I20250420 00:31:37.129171 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:37.293496 11898994 incremental_pipeline.cc:390] Registering image #91 (18)
I20250420 00:31:37.293522 11898994 incremental_pipeline.cc:393] => Image sees 113 / 840 points
I20250420 00:31:37.391775 11898994 incremental_pipeline.cc:390] Registering image #34 (19)
I20250420 00:31:37.391801 11898994 incremental_pipeline.cc:393] => Image sees 165 / 1172 points
I20250420 00:31:37.447678 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:37.569984 11898994 incremental_pipeline.cc:390] Registering image #5 (20)
I20250420 00:31:37.570011 11898994 incremental_pipeline.cc:393] => Image sees 196 / 1039 points
I20250420 00:31:37.695657 11898994 incremental_pipeline.cc:390] Registering image #116 (21)
I20250420 00:31:37.695685 11898994 incremental_pipeline.cc:393] => Image sees 138 / 607 points
I20250420 00:31:37.760128 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:37.921934 11898994 incremental_pipeline.cc:390] Registering image #84 (22)
I20250420 00:31:37.921962 11898994 incremental_pipeline.cc:393] => Image sees 124 / 1256 points
I20250420 00:31:38.041033 11898994 incremental_pipeline.cc:390] Registering image #87 (23)
I20250420 00:31:38.041056 11898994 incremental_pipeline.cc:393] => Image sees 142 / 2099 points
I20250420 00:31:38.163815 11898994 incremental_pipeline.cc:390] Registering image #118 (24)
I20250420 00:31:38.163841 11898994 incremental_pipeline.cc:393] => Image sees 178 / 2013 points
I20250420 00:31:38.199781 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:38.376994 11898994 incremental_pipeline.cc:390] Registering image #73 (25)
I20250420 00:31:38.377019 11898994 incremental_pipeline.cc:393] => Image sees 199 / 1178 points
I20250420 00:31:38.489838 11898994 incremental_pipeline.cc:390] Registering image #80 (26)
I20250420 00:31:38.489863 11898994 incremental_pipeline.cc:393] => Image sees 126 / 815 points
I20250420 00:31:38.599553 11898994 incremental_pipeline.cc:390] Registering image #95 (27)
I20250420 00:31:38.599582 11898994 incremental_pipeline.cc:393] => Image sees 128 / 1138 points
I20250420 00:31:38.633994 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:38.955135 11898994 incremental_pipeline.cc:390] Registering image #33 (28)
I20250420 00:31:38.955165 11898994 incremental_pipeline.cc:393] => Image sees 119 / 1135 points
I20250420 00:31:39.074579 11898994 incremental_pipeline.cc:390] Registering image #23 (29)
I20250420 00:31:39.074606 11898994 incremental_pipeline.cc:393] => Image sees 140 / 943 points
I20250420 00:31:39.194028 11898994 incremental_pipeline.cc:390] Registering image #129 (30)
I20250420 00:31:39.194053 11898994 incremental_pipeline.cc:393] => Image sees 159 / 721 points
I20250420 00:31:39.220971 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:39.535158 11898994 incremental_pipeline.cc:390] Registering image #30 (31)
I20250420 00:31:39.535185 11898994 incremental_pipeline.cc:393] => Image sees 162 / 1316 points
I20250420 00:31:39.661914 11898994 incremental_pipeline.cc:390] Registering image #107 (32)
I20250420 00:31:39.661942 11898994 incremental_pipeline.cc:393] => Image sees 124 / 1372 points
I20250420 00:31:39.768364 11898994 incremental_pipeline.cc:390] Registering image #98 (33)
I20250420 00:31:39.768389 11898994 incremental_pipeline.cc:393] => Image sees 233 / 1562 points
I20250420 00:31:39.813663 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:40.116281 11898994 incremental_pipeline.cc:390] Registering image #35 (34)
I20250420 00:31:40.116307 11898994 incremental_pipeline.cc:393] => Image sees 255 / 1195 points
I20250420 00:31:40.228528 11898994 incremental_pipeline.cc:390] Registering image #14 (35)
I20250420 00:31:40.228550 11898994 incremental_pipeline.cc:393] => Image sees 127 / 1117 points
I20250420 00:31:40.341877 11898994 incremental_pipeline.cc:390] Registering image #93 (36)
I20250420 00:31:40.341899 11898994 incremental_pipeline.cc:393] => Image sees 94 / 1194 points
I20250420 00:31:40.437747 11898994 incremental_pipeline.cc:390] Registering image #92 (37)
I20250420 00:31:40.437768 11898994 incremental_pipeline.cc:393] => Image sees 121 / 1948 points
I20250420 00:31:40.465142 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:40.833109 11898994 incremental_pipeline.cc:390] Registering image #74 (38)
I20250420 00:31:40.833134 11898994 incremental_pipeline.cc:393] => Image sees 289 / 1812 points
I20250420 00:31:40.943281 11898994 incremental_pipeline.cc:390] Registering image #102 (39)
I20250420 00:31:40.943305 11898994 incremental_pipeline.cc:393] => Image sees 179 / 978 points
I20250420 00:31:41.052443 11898994 incremental_pipeline.cc:390] Registering image #103 (40)
I20250420 00:31:41.052469 11898994 incremental_pipeline.cc:393] => Image sees 116 / 718 points
I20250420 00:31:41.149955 11898994 incremental_pipeline.cc:390] Registering image #108 (41)
I20250420 00:31:41.149979 11898994 incremental_pipeline.cc:393] => Image sees 109 / 2342 points
I20250420 00:31:41.192839 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:41.768766 11898994 incremental_pipeline.cc:390] Registering image #97 (42)
I20250420 00:31:41.768790 11898994 incremental_pipeline.cc:393] => Image sees 152 / 2323 points
I20250420 00:31:41.883423 11898994 incremental_pipeline.cc:390] Registering image #96 (43)
I20250420 00:31:41.883450 11898994 incremental_pipeline.cc:393] => Image sees 133 / 1059 points
I20250420 00:31:41.981765 11898994 incremental_pipeline.cc:390] Registering image #76 (44)
I20250420 00:31:41.981791 11898994 incremental_pipeline.cc:393] => Image sees 194 / 1123 points
I20250420 00:31:42.088882 11898994 incremental_pipeline.cc:390] Registering image #77 (45)
I20250420 00:31:42.088908 11898994 incremental_pipeline.cc:393] => Image sees 140 / 771 points
I20250420 00:31:42.209141 11898994 incremental_pipeline.cc:390] Registering image #121 (46)
I20250420 00:31:42.209168 11898994 incremental_pipeline.cc:393] => Image sees 116 / 562 points
I20250420 00:31:42.232072 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:42.608240 11898994 incremental_pipeline.cc:390] Registering image #120 (47)
I20250420 00:31:42.608266 11898994 incremental_pipeline.cc:393] => Image sees 88 / 433 points
I20250420 00:31:42.696267 11898994 incremental_pipeline.cc:390] Registering image #64 (48)
I20250420 00:31:42.696296 11898994 incremental_pipeline.cc:393] => Image sees 131 / 954 points
I20250420 00:31:42.805765 11898994 incremental_pipeline.cc:390] Registering image #110 (49)
I20250420 00:31:42.805788 11898994 incremental_pipeline.cc:393] => Image sees 80 / 732 points
I20250420 00:31:42.903600 11898994 incremental_pipeline.cc:390] Registering image #21 (50)
I20250420 00:31:42.903627 11898994 incremental_pipeline.cc:393] => Image sees 109 / 846 points
I20250420 00:31:43.001376 11898994 incremental_pipeline.cc:390] Registering image #115 (51)
I20250420 00:31:43.001401 11898994 incremental_pipeline.cc:393] => Image sees 150 / 1176 points
I20250420 00:31:43.025151 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:43.743094 11898994 incremental_pipeline.cc:390] Registering image #29 (52)
I20250420 00:31:43.743124 11898994 incremental_pipeline.cc:393] => Image sees 248 / 1141 points
I20250420 00:31:43.849396 11898994 incremental_pipeline.cc:390] Registering image #78 (53)
I20250420 00:31:43.849426 11898994 incremental_pipeline.cc:393] => Image sees 65 / 847 points
I20250420 00:31:43.951844 11898994 incremental_pipeline.cc:390] Registering image #111 (54)
I20250420 00:31:43.951872 11898994 incremental_pipeline.cc:393] => Image sees 113 / 1521 points
I20250420 00:31:44.054701 11898994 incremental_pipeline.cc:390] Registering image #114 (55)
I20250420 00:31:44.054729 11898994 incremental_pipeline.cc:393] => Image sees 76 / 1998 points
I20250420 00:31:44.156697 11898994 incremental_pipeline.cc:390] Registering image #112 (56)
I20250420 00:31:44.156728 11898994 incremental_pipeline.cc:393] => Image sees 164 / 1408 points
I20250420 00:31:44.261991 11898994 incremental_pipeline.cc:390] Registering image #113 (57)
I20250420 00:31:44.262022 11898994 incremental_pipeline.cc:393] => Image sees 175 / 747 points
I20250420 00:31:44.289667 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:45.029595 11898994 incremental_pipeline.cc:390] Registering image #25 (58)
I20250420 00:31:45.029630 11898994 incremental_pipeline.cc:393] => Image sees 92 / 589 points
I20250420 00:31:45.150416 11898994 incremental_pipeline.cc:390] Registering image #26 (59)
I20250420 00:31:45.150446 11898994 incremental_pipeline.cc:393] => Image sees 104 / 1114 points
I20250420 00:31:45.251488 11898994 incremental_pipeline.cc:390] Registering image #81 (60)
I20250420 00:31:45.251523 11898994 incremental_pipeline.cc:393] => Image sees 109 / 564 points
I20250420 00:31:45.356739 11898994 incremental_pipeline.cc:390] Registering image #130 (61)
I20250420 00:31:45.356767 11898994 incremental_pipeline.cc:393] => Image sees 76 / 239 points
I20250420 00:31:45.449483 11898994 incremental_pipeline.cc:390] Registering image #13 (62)
I20250420 00:31:45.449512 11898994 incremental_pipeline.cc:393] => Image sees 59 / 394 points
I20250420 00:31:45.544302 11898994 incremental_pipeline.cc:390] Registering image #83 (63)
I20250420 00:31:45.544329 11898994 incremental_pipeline.cc:393] => Image sees 78 / 2632 points
I20250420 00:31:45.572455 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:46.012014 11898994 incremental_pipeline.cc:390] Registering image #86 (64)
I20250420 00:31:46.012041 11898994 incremental_pipeline.cc:393] => Image sees 115 / 2695 points
I20250420 00:31:46.131263 11898994 incremental_pipeline.cc:390] Registering image #52 (65)
I20250420 00:31:46.131292 11898994 incremental_pipeline.cc:393] => Image sees 175 / 449 points
I20250420 00:31:46.236114 11898994 incremental_pipeline.cc:390] Registering image #66 (66)
I20250420 00:31:46.236143 11898994 incremental_pipeline.cc:393] => Image sees 108 / 809 points
I20250420 00:31:46.350932 11898994 incremental_pipeline.cc:390] Registering image #56 (67)
I20250420 00:31:46.350960 11898994 incremental_pipeline.cc:393] => Image sees 120 / 1168 points
I20250420 00:31:46.465705 11898994 incremental_pipeline.cc:390] Registering image #44 (68)
I20250420 00:31:46.465732 11898994 incremental_pipeline.cc:393] => Image sees 80 / 833 points
I20250420 00:31:46.574033 11898994 incremental_pipeline.cc:390] Registering image #100 (69)
I20250420 00:31:46.574059 11898994 incremental_pipeline.cc:393] => Image sees 66 / 641 points
I20250420 00:31:46.592844 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:46.987115 11898994 incremental_pipeline.cc:390] Registering image #101 (70)
I20250420 00:31:46.987143 11898994 incremental_pipeline.cc:393] => Image sees 68 / 440 points
I20250420 00:31:47.085470 11898994 incremental_pipeline.cc:390] Registering image #136 (71)
I20250420 00:31:47.085498 11898994 incremental_pipeline.cc:393] => Image sees 74 / 516 points
I20250420 00:31:47.176045 11898994 incremental_pipeline.cc:390] Registering image #43 (72)
I20250420 00:31:47.176071 11898994 incremental_pipeline.cc:393] => Image sees 80 / 1378 points
I20250420 00:31:47.273537 11898994 incremental_pipeline.cc:390] Registering image #47 (73)
I20250420 00:31:47.273564 11898994 incremental_pipeline.cc:393] => Image sees 78 / 485 points
I20250420 00:31:47.368391 11898994 incremental_pipeline.cc:390] Registering image #132 (74)
I20250420 00:31:47.368427 11898994 incremental_pipeline.cc:393] => Image sees 81 / 910 points
I20250420 00:31:47.461589 11898994 incremental_pipeline.cc:390] Registering image #45 (75)
I20250420 00:31:47.461618 11898994 incremental_pipeline.cc:393] => Image sees 111 / 584 points
I20250420 00:31:47.558632 11898994 incremental_pipeline.cc:390] Registering image #46 (76)
I20250420 00:31:47.558655 11898994 incremental_pipeline.cc:393] => Image sees 141 / 803 points
I20250420 00:31:47.580231 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:47.981992 11898994 incremental_pipeline.cc:390] Registering image #42 (77)
I20250420 00:31:47.982021 11898994 incremental_pipeline.cc:393] => Image sees 82 / 826 points
I20250420 00:31:48.078119 11898994 incremental_pipeline.cc:390] Registering image #68 (78)
I20250420 00:31:48.078147 11898994 incremental_pipeline.cc:393] => Image sees 90 / 705 points
I20250420 00:31:48.169334 11898994 incremental_pipeline.cc:390] Registering image #134 (79)
I20250420 00:31:48.169358 11898994 incremental_pipeline.cc:393] => Image sees 114 / 2894 points
I20250420 00:31:48.265362 11898994 incremental_pipeline.cc:390] Registering image #133 (80)
I20250420 00:31:48.265390 11898994 incremental_pipeline.cc:393] => Image sees 336 / 3115 points
I20250420 00:31:48.377570 11898994 incremental_pipeline.cc:390] Registering image #55 (81)
I20250420 00:31:48.377596 11898994 incremental_pipeline.cc:393] => Image sees 108 / 725 points
I20250420 00:31:48.475188 11898994 incremental_pipeline.cc:390] Registering image #131 (82)
I20250420 00:31:48.475214 11898994 incremental_pipeline.cc:393] => Image sees 152 / 632 points
I20250420 00:31:48.574430 11898994 incremental_pipeline.cc:390] Registering image #62 (83)
I20250420 00:31:48.574455 11898994 incremental_pipeline.cc:393] => Image sees 101 / 445 points
I20250420 00:31:48.676856 11898994 incremental_pipeline.cc:390] Registering image #48 (84)
I20250420 00:31:48.676882 11898994 incremental_pipeline.cc:393] => Image sees 93 / 1074 points
I20250420 00:31:48.706295 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:49.303151 11898994 incremental_pipeline.cc:390] Registering image #70 (85)
I20250420 00:31:49.303179 11898994 incremental_pipeline.cc:393] => Image sees 88 / 1249 points
I20250420 00:31:49.401777 11898994 incremental_pipeline.cc:390] Registering image #71 (86)
I20250420 00:31:49.401809 11898994 incremental_pipeline.cc:393] => Image sees 97 / 1755 points
I20250420 00:31:49.496819 11898994 incremental_pipeline.cc:390] Registering image #119 (87)
I20250420 00:31:49.496846 11898994 incremental_pipeline.cc:393] => Image sees 160 / 1423 points
I20250420 00:31:49.595101 11898994 incremental_pipeline.cc:390] Registering image #57 (88)
I20250420 00:31:49.595124 11898994 incremental_pipeline.cc:393] => Image sees 74 / 519 points
I20250420 00:31:49.683482 11898994 incremental_pipeline.cc:390] Registering image #54 (89)
I20250420 00:31:49.683507 11898994 incremental_pipeline.cc:393] => Image sees 80 / 1015 points
I20250420 00:31:49.785425 11898994 incremental_pipeline.cc:390] Registering image #53 (90)
I20250420 00:31:49.785449 11898994 incremental_pipeline.cc:393] => Image sees 92 / 727 points
I20250420 00:31:49.882241 11898994 incremental_pipeline.cc:390] Registering image #127 (91)
I20250420 00:31:49.882261 11898994 incremental_pipeline.cc:393] => Image sees 73 / 944 points
I20250420 00:31:49.974702 11898994 incremental_pipeline.cc:390] Registering image #72 (92)
I20250420 00:31:49.974728 11898994 incremental_pipeline.cc:393] => Image sees 85 / 453 points
I20250420 00:31:50.063733 11898994 incremental_pipeline.cc:390] Registering image #69 (93)
I20250420 00:31:50.063758 11898994 incremental_pipeline.cc:393] => Image sees 61 / 427 points
I20250420 00:31:50.075489 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:50.508538 11898994 incremental_pipeline.cc:390] Registering image #40 (94)
I20250420 00:31:50.508563 11898994 incremental_pipeline.cc:393] => Image sees 63 / 507 points
I20250420 00:31:50.612355 11898994 incremental_pipeline.cc:390] Registering image #135 (95)
I20250420 00:31:50.612383 11898994 incremental_pipeline.cc:393] => Image sees 72 / 135 points
I20250420 00:31:50.710906 11898994 incremental_pipeline.cc:390] Registering image #49 (96)
I20250420 00:31:50.710974 11898994 incremental_pipeline.cc:393] => Image sees 54 / 408 points
I20250420 00:31:50.711601 11898994 incremental_pipeline.cc:404] => Could not register, trying another image.
I20250420 00:31:50.711623 11898994 incremental_pipeline.cc:390] Registering image #65 (96)
I20250420 00:31:50.711635 11898994 incremental_pipeline.cc:393] => Image sees 45 / 1135 points
I20250420 00:31:50.808749 11898994 incremental_pipeline.cc:390] Registering image #106 (97)
I20250420 00:31:50.808775 11898994 incremental_pipeline.cc:393] => Image sees 42 / 649 points
I20250420 00:31:50.809239 11898994 incremental_pipeline.cc:404] => Could not register, trying another image.
I20250420 00:31:50.809250 11898994 incremental_pipeline.cc:390] Registering image #67 (97)
I20250420 00:31:50.809262 11898994 incremental_pipeline.cc:393] => Image sees 42 / 492 points
I20250420 00:31:50.809720 11898994 incremental_pipeline.cc:404] => Could not register, trying another image.
I20250420 00:31:50.809731 11898994 incremental_pipeline.cc:390] Registering image #17 (97)
I20250420 00:31:50.809743 11898994 incremental_pipeline.cc:393] => Image sees 58 / 517 points
I20250420 00:31:50.924921 11898994 incremental_pipeline.cc:390] Registering image #61 (98)
I20250420 00:31:50.924947 11898994 incremental_pipeline.cc:393] => Image sees 36 / 307 points
I20250420 00:31:50.925259 11898994 incremental_pipeline.cc:404] => Could not register, trying another image.
I20250420 00:31:50.925271 11898994 incremental_pipeline.cc:390] Registering image #99 (98)
I20250420 00:31:50.925284 11898994 incremental_pipeline.cc:393] => Image sees 37 / 1969 points
I20250420 00:31:51.016974 11898994 incremental_pipeline.cc:390] Registering image #58 (99)
I20250420 00:31:51.017005 11898994 incremental_pipeline.cc:393] => Image sees 93 / 982 points
I20250420 00:31:51.114655 11898994 incremental_pipeline.cc:390] Registering image #79 (100)
I20250420 00:31:51.114691 11898994 incremental_pipeline.cc:393] => Image sees 84 / 807 points
I20250420 00:31:51.212138 11898994 incremental_pipeline.cc:390] Registering image #123 (101)
I20250420 00:31:51.212165 11898994 incremental_pipeline.cc:393] => Image sees 88 / 995 points
I20250420 00:31:51.310663 11898994 incremental_pipeline.cc:390] Registering image #122 (102)
I20250420 00:31:51.310694 11898994 incremental_pipeline.cc:393] => Image sees 167 / 804 points
I20250420 00:31:51.411487 11898994 incremental_pipeline.cc:390] Registering image #36 (103)
I20250420 00:31:51.411520 11898994 incremental_pipeline.cc:393] => Image sees 84 / 675 points
I20250420 00:31:51.434041 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:51.953446 11898994 incremental_pipeline.cc:390] Registering image #124 (104)
I20250420 00:31:51.953476 11898994 incremental_pipeline.cc:393] => Image sees 62 / 357 points
I20250420 00:31:52.051739 11898994 incremental_pipeline.cc:390] Registering image #38 (105)
I20250420 00:31:52.051767 11898994 incremental_pipeline.cc:393] => Image sees 54 / 237 points
I20250420 00:31:52.148880 11898994 incremental_pipeline.cc:390] Registering image #27 (106)
I20250420 00:31:52.148914 11898994 incremental_pipeline.cc:393] => Image sees 66 / 2134 points
I20250420 00:31:52.240804 11898994 incremental_pipeline.cc:390] Registering image #3 (107)
I20250420 00:31:52.240834 11898994 incremental_pipeline.cc:393] => Image sees 133 / 2308 points
I20250420 00:31:52.348773 11898994 incremental_pipeline.cc:390] Registering image #2 (108)
I20250420 00:31:52.348802 11898994 incremental_pipeline.cc:393] => Image sees 353 / 1024 points
I20250420 00:31:52.458424 11898994 incremental_pipeline.cc:390] Registering image #4 (109)
I20250420 00:31:52.458456 11898994 incremental_pipeline.cc:393] => Image sees 40 / 179 points
I20250420 00:31:52.458835 11898994 incremental_pipeline.cc:404] => Could not register, trying another image.
I20250420 00:31:52.458854 11898994 incremental_pipeline.cc:390] Registering image #24 (109)
I20250420 00:31:52.458868 11898994 incremental_pipeline.cc:393] => Image sees 37 / 1425 points
I20250420 00:31:52.552170 11898994 incremental_pipeline.cc:390] Registering image #1 (110)
I20250420 00:31:52.552202 11898994 incremental_pipeline.cc:393] => Image sees 64 / 1864 points
I20250420 00:31:52.650240 11898994 incremental_pipeline.cc:390] Registering image #16 (111)
I20250420 00:31:52.650271 11898994 incremental_pipeline.cc:393] => Image sees 90 / 1500 points
I20250420 00:31:52.740471 11898994 incremental_pipeline.cc:390] Registering image #117 (112)
I20250420 00:31:52.740499 11898994 incremental_pipeline.cc:393] => Image sees 144 / 1933 points
I20250420 00:31:52.845301 11898994 incremental_pipeline.cc:390] Registering image #9 (113)
I20250420 00:31:52.845332 11898994 incremental_pipeline.cc:393] => Image sees 265 / 1345 points
I20250420 00:31:52.950695 11898994 incremental_pipeline.cc:390] Registering image #51 (114)
I20250420 00:31:52.950721 11898994 incremental_pipeline.cc:393] => Image sees 37 / 535 points
I20250420 00:31:52.951197 11898994 incremental_pipeline.cc:404] => Could not register, trying another image.
I20250420 00:31:52.951208 11898994 incremental_pipeline.cc:390] Registering image #128 (114)
I20250420 00:31:52.951220 11898994 incremental_pipeline.cc:393] => Image sees 32 / 228 points
I20250420 00:31:52.951434 11898994 incremental_pipeline.cc:404] => Could not register, trying another image.
I20250420 00:31:52.951445 11898994 incremental_pipeline.cc:390] Registering image #106 (114)
I20250420 00:31:52.951457 11898994 incremental_pipeline.cc:393] => Image sees 160 / 649 points
I20250420 00:31:52.975770 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:53.641659 11898994 incremental_pipeline.cc:390] Registering image #67 (115)
I20250420 00:31:53.641692 11898994 incremental_pipeline.cc:393] => Image sees 56 / 492 points
I20250420 00:31:53.642284 11898994 incremental_pipeline.cc:404] => Could not register, trying another image.
I20250420 00:31:53.642298 11898994 incremental_pipeline.cc:390] Registering image #49 (115)
I20250420 00:31:53.642309 11898994 incremental_pipeline.cc:393] => Image sees 53 / 408 points
I20250420 00:31:53.642920 11898994 incremental_pipeline.cc:404] => Could not register, trying another image.
I20250420 00:31:53.642938 11898994 incremental_pipeline.cc:390] Registering image #4 (115)
I20250420 00:31:53.642961 11898994 incremental_pipeline.cc:393] => Image sees 39 / 179 points
I20250420 00:31:53.643222 11898994 incremental_pipeline.cc:404] => Could not register, trying another image.
I20250420 00:31:53.643234 11898994 incremental_pipeline.cc:390] Registering image #61 (115)
I20250420 00:31:53.643245 11898994 incremental_pipeline.cc:393] => Image sees 35 / 307 points
I20250420 00:31:53.643599 11898994 incremental_pipeline.cc:404] => Could not register, trying another image.
I20250420 00:31:53.643610 11898994 incremental_pipeline.cc:390] Registering image #51 (115)
I20250420 00:31:53.643621 11898994 incremental_pipeline.cc:393] => Image sees 37 / 535 points
I20250420 00:31:53.643951 11898994 incremental_pipeline.cc:404] => Could not register, trying another image.
I20250420 00:31:53.643963 11898994 incremental_pipeline.cc:390] Registering image #128 (115)
I20250420 00:31:53.643975 11898994 incremental_pipeline.cc:393] => Image sees 32 / 228 points
I20250420 00:31:53.644383 11898994 incremental_pipeline.cc:404] => Could not register, trying another image.
I20250420 00:31:53.644393 11898994 incremental_pipeline.cc:42] Retriangulation and Global bundle adjustment
I20250420 00:31:53.884457 11898994 incremental_pipeline.cc:390] Registering image #67 (115)
I20250420 00:31:53.884491 11898994 incremental_pipeline.cc:393] => Image sees 56 / 492 points
I20250420 00:31:53.885190 11898994 incremental_pipeline.cc:404] => Could not register, trying another image.
I20250420 00:31:53.885205 11898994 incremental_pipeline.cc:390] Registering image #49 (115)
I20250420 00:31:53.885214 11898994 incremental_pipeline.cc:393] => Image sees 53 / 408 points
I20250420 00:31:53.885706 11898994 incremental_pipeline.cc:404] => Could not register, trying another image.
I20250420 00:31:53.885717 11898994 incremental_pipeline.cc:390] Registering image #4 (115)
I20250420 00:31:53.885727 11898994 incremental_pipeline.cc:393] => Image sees 39 / 179 points
I20250420 00:31:53.886045 11898994 incremental_pipeline.cc:404] => Could not register, trying another image.
I20250420 00:31:53.886057 11898994 incremental_pipeline.cc:390] Registering image #61 (115)
I20250420 00:31:53.886067 11898994 incremental_pipeline.cc:393] => Image sees 35 / 307 points
I20250420 00:31:53.886278 11898994 incremental_pipeline.cc:404] => Could not register, trying another image.
I20250420 00:31:53.886294 11898994 incremental_pipeline.cc:390] Registering image #51 (115)
I20250420 00:31:53.886304 11898994 incremental_pipeline.cc:393] => Image sees 37 / 535 points
I20250420 00:31:53.886710 11898994 incremental_pipeline.cc:404] => Could not register, trying another image.
I20250420 00:31:53.886735 11898994 incremental_pipeline.cc:390] Registering image #128 (115)
I20250420 00:31:53.886749 11898994 incremental_pipeline.cc:393] => Image sees 32 / 228 points
I20250420 00:31:53.887077 11898994 incremental_pipeline.cc:404] => Could not register, trying another image.
I20250420 00:31:53.942826 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:53.958839 11898994 incremental_pipeline.cc:306] Initializing with image pair #67 and #49
I20250420 00:31:53.959100 11898994 incremental_pipeline.cc:311] Global bundle adjustment
I20250420 00:31:54.053117 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.057781 11898994 incremental_pipeline.cc:306] Initializing with image pair #7 and #8
I20250420 00:31:54.058032 11898994 incremental_pipeline.cc:311] Global bundle adjustment
I20250420 00:31:54.138037 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.138742 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.145107 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.145805 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.151213 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.151921 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.157939 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.158624 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.164039 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.164724 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.170593 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.171277 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.177081 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.177765 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.183197 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.183877 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.189464 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.190150 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.196528 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.197252 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.203828 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.204528 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.209473 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.210162 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.216250 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.216936 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.223337 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.224027 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.230000 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.230690 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.236497 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.237180 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.242954 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.243633 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.248924 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.249601 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.255510 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.256212 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.262276 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.262961 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.268797 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.269477 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.275676 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.276357 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.281805 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.282495 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.288463 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.289170 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.295157 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.295842 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.301467 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.302166 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.308298 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.308980 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.314519 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.315228 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.321218 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.321918 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.327162 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.327857 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.333848 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.334540 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.340611 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.341296 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.347725 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.348431 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.356334 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.357048 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.364192 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.364897 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.372058 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.372753 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.378553 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.379242 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.385069 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.385764 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.391789 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.392472 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.398882 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.399564 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.405447 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.406131 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.412655 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.413336 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.418982 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.419665 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.425413 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.426096 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.430812 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.431496 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.436591 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.437275 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.443193 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.443874 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.449064 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.449743 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.454970 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.455657 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.461236 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.461915 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.468151 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.468873 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.486335 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.487917 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.499994 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.501149 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.512272 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.513005 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.525544 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.526582 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.534731 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.535459 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.543385 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.544126 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.552110 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.552850 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.559815 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.560516 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.570216 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.570921 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.577775 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.578498 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.586329 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.587028 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.593833 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.594556 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.603376 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.604076 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.610967 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.611671 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.618901 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.619596 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.626888 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.627594 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.637650 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.638341 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.644901 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.645588 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.652853 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.653551 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.660032 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.660744 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.668674 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.669384 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.676818 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.677524 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.687356 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.688055 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.695209 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.695923 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.703313 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.704030 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.710337 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.711071 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.719044 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.719760 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.726451 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.727142 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.737470 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.738166 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.744968 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.745663 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.754173 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.754881 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.762061 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.762755 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.768857 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.769551 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.775581 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.776265 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.781647 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.782341 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.788439 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.789144 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.795589 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.796275 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.802448 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.803136 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.808985 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.809758 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.816529 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.817227 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.823143 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.823833 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.829984 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.830677 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.836545 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.837229 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.842862 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.843541 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.848768 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.849462 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.855278 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.855962 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.862310 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.862993 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.869331 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.870018 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.875986 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.876678 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.882202 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.882891 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.888761 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.889468 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.895820 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.896507 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.902318 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.903010 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.908623 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.909300 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.915071 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.915786 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.921784 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.922487 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.928032 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.928737 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.933640 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.934325 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.939985 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.940675 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.946445 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.947129 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.952299 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.952984 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.958494 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.959173 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.964792 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.965482 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.970729 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.971429 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.976707 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.977387 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.982862 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.983548 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.989403 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.990083 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:54.995709 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:54.996393 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.001479 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.002157 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.008271 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.008980 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.015969 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.016649 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.023051 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.023752 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.030112 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.030843 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.038551 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.039245 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.045630 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.046339 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.053009 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.053694 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.059216 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.059901 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.066579 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.067284 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.073158 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.073873 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.080118 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.080811 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.088204 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.088913 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.095623 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.096319 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.104874 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.105564 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.112628 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.113328 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.120434 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.121152 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.126662 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.127350 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.133440 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.134125 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.139758 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.140458 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.147384 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.148079 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.155771 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.156457 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.163244 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.163933 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.170764 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.171494 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.177717 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.178428 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.187078 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.187783 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.193941 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.194633 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.202435 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.203121 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.208613 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.209297 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.215834 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.216526 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.222448 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.223136 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.229243 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.229938 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.237708 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.238415 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.243983 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.244678 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.253398 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.254093 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.260663 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.261366 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.269356 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.270041 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.276379 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.277117 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.284612 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.285332 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.294090 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.294824 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.303164 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.303862 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.310132 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.310832 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.316879 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.317569 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.323267 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.323967 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.329890 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.330578 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.337181 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.337922 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.346608 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.347309 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.355072 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.355756 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.362639 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.363343 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.370726 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.371439 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.378237 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.378965 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.387550 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.388275 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.394155 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.394868 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.403724 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.404471 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.409304 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.410053 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.417835 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.418560 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.426710 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.427412 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.436460 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.437161 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.443907 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.444615 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.451683 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.452399 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.456992 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.457734 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.463469 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.464170 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.470242 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.470935 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.477314 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.478024 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.486073 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.486761 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.493060 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.493744 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.498973 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.499650 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.505724 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.506417 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.512748 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.513432 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.519827 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.520510 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.526839 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.527558 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.535954 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.536687 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.546847 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.547564 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.554044 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.554764 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.563057 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.563786 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.570739 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.571488 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.580008 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.580729 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.588558 11898994 incremental_pipeline.cc:282] Finding good initial image pair
I20250420 00:31:55.589275 11898994 incremental_pipeline.cc:286] => No good initial image pair found.
I20250420 00:31:55.591017 11898994 timer.cc:91] Elapsed time: 0.350 [minutes]

COLMAP reconstruction completed successfully!
Results are in: /Users/wentaojiang/Documents/3DGS/20250420_spectra_physics_navigator_J40-X30SC/sparse/0
```


## 20250502 bluefors
2025-05-02T19:32:50-07:00
Got data from BF1 from [LINQS](https://linqs.stanford.edu/).

2025-05-02T19:43:49-07:00 started colmap 1.5 min ago.
2025-05-02T20:19:42-07:00 Done 5 min ago. Not bad.
2025-05-02T20:22:41-07:00
hmm only two pictures...
Going to rerun colmap
Installing using brew: `brew install colmap`
2025-05-02T20:30:58-07:00
Reading: https://colmap.github.io/tutorial.html
![[Screenshot 2025-05-02 at 20.33.10.png]]

2025-05-02T21:12:16-07:00
No good match. Redo extraction and matching
![[Pasted image 20250502211222.png]]

Matching:
![[Pasted image 20250502211325.png]]

A lot of warnings:
![[Pasted image 20250502211347.png]]

2025-05-02T22:34:19-07:00
Took almost one hour:
![[Pasted image 20250502223424.png]]
Ugh the reconstruction still sucks:
![[Pasted image 20250502223521.png]]

2025-05-02T22:38:47-07:00
Seems like there are too many key points:
![[Pasted image 20250502223853.png]]

2025-05-02T22:48:56-07:00
Redo extraction and matching:
![[Pasted image 20250502224919.png]]
![[Pasted image 20250502224903.png]]

Ughhhhh
![[Pasted image 20250502224944.png]]

Is this still too many?
![[Pasted image 20250502225043.png]]

There are quite some matching pairs:
![[Pasted image 20250502225157.png]]
Some matches do not make sense:
![[Pasted image 20250502225238.png]]

2025-05-02T22:54:40-07:00
Gave up for now.



## 20250504 wirebodner
2025-05-04T16:55:39-07:00
Started colmap. Using gradio instead of colmap gui.
2025-05-04T16:57:46-07:00
sequential only found two pairs. Running exhaustive now.












# I should actually sit down and learn webgl
2025-04-22T23:17:43-07:00
Reading
- https://webgl2fundamentals.org/webgl/lessons/webgl-fundamentals.html
2025-04-24T23:52:54-07:00
This is kind of boring, I just finished reading one article about resizing the canvas...


# Capturing rig
2025-04-24T23:54:08-07:00
Also called dolly.
These look tempting:
- https://www.walmart.com/ip/Shootvilla-Half-Circle-Video-Camera-Slider-Track-180-Curve-for-professional-Load-10-kg/7076707360
- https://www.walmart.com/ip/SHOOTVILLA-Curve-360-Curved-Circular-Camera-Slider/7114967174
- I don't wanna pay freaking 500 bucks for this.
- Let's try sendcutsend it
![[Pasted image 20250424235536.png]]

![[Pasted image 20250424235545.png]]

Seems like we will need three main parts:
- the arc / rail, the support, and the cart.
- I can't tell about the mounting point
- How do we do the other DoF, gooseneck? Would it be too big? Feels like it would.

For the cart:
- 625 series ball bearings
- mounting: Standard 1/4"-20 or 3/8"-16 screw (depending on usage) for attaching camera, phone mount, or ball head.
- How would I lock the cart?
- cam levers:
- ![[Pasted image 20250425001303.png]]


2025-04-25T00:08:27-07:00
I guess you can just buy these:
![[Pasted image 20250425000834.png]]
- I don't need 20 pcs of it...

For the other DOF, Open Scan is a good example:
![[Pasted image 20250425001039.png]]
- https://openscan.eu/pages/openscan-mini








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
















