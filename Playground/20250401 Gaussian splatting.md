
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


