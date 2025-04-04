
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








