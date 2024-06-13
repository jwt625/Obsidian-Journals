
https://github.com/roboflow/supervision

2024-06-12 22:17 On Wentao's home desktop (2020)
The install is taking forever:
![[Pasted image 20240612221819.png]]

Also noticed I need Python >=3.8.
22:21 Switched to Python 3.11.4 (base conda) and installed supervision successfully. Took 21 s.
`ultralytics` is not identified. Installing it.
22:25 Installed. Restart kernel. Getting the following error from the install:
```
ERROR: Cannot uninstall 'TBB'. It is a distutils installed project and thus we cannot accurately determine which files belong to it which would lead to only a partial uninstall.
```
Following chat GPT for debug:
`pip install --upgrade pip setuptools
`
Getting the following error:
```
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts. conda-repo-cli 1.0.41 requires requests_mock, which is not installed. conda-repo-cli 1.0.41 requires clyent==1.2.1, but you have clyent 1.2.2 which is incompatible. conda-repo-cli 1.0.41 requires nbformat==5.4.0, but you have nbformat 5.7.0 which is incompatible. conda-repo-cli 1.0.41 requires requests==2.28.1, but you have requests 2.31.0 which is incompatible.
```

22:30 Removed TBB manually using folder from 
```
import tbb
print(tbb.__file__)
```

Now running `pip install ultralytics --ignore-installed TBB`
22:37 Took 6 min 20 s, seems done. Reinstall tbb `pip install tbb`
Ok seems like I could test the YOLO model.
22:42 Got the detection results. Trying to figure out how to plot it.
22:50 Found it is better to use their own annotator instead of trying to use chatGPT:
- https://supervision.roboflow.com/latest/notebooks/quickstart/#boxannotator
![[Pasted image 20240612225048.png]]
- this is a picture I took from the Paly flea market.



