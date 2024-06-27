
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

# Revisit, going to try more technical object detection

2024-06-26T21:28:48-07:00
Revisit this, now vscode-jupyter is not running:
![[Pasted image 20240626212923.png]]
- click on [here](https://github.com/microsoft/vscode-jupyter/wiki/Failure-to-start-kernel-due-to-failures-in-importing-modules-from-files)
Restart VS code.
Nope same error.
Tried `pip install <xyz> -U --force-reinstall`. Did not work.
Going to try installing supervision on the microsoft store python 3.12.4.

Installing
- ipykernel... Done
- `pip install opencv-python`... Done
- `pip install supervision`... Done
![[Pasted image 20240626213650.png]]
- trying `pip install --user supervision`... Same error.

2024-06-26T21:44:54-07:00
If I just run the whole script without using ipynb, I get the following error:
```
Exception has occurred: ImportError

- 

version conflict: 'd:\\Users\\Wentao\\anaconda3\\Lib\\site-packages\\psutil\\_psutil_windows.cp311-win_amd64.pyd' C extension module was built for another version of psutil (5.9.0 instead of 5.9.8); you may try to 'pip uninstall psutil', manually remove d:\Users\Wentao\anaconda3\Lib\site-packages\psutil\_psutil_windows.cp311-win_amd64.pyd or clean the virtual env somehow, then reinstall

File "C:\Users\Wentao\Documents\GitHub\PlayGround\20240612_supervision\main.py", line 9, in <module> from ultralytics import YOLO ImportError: version conflict: 'd:\\Users\\Wentao\\anaconda3\\Lib\\site-packages\\psutil\\_psutil_windows.cp311-win_amd64.pyd' C extension module was built for another version of psutil (5.9.0 instead of 5.9.8); you may try to 'pip uninstall psutil', manually remove d:\Users\Wentao\anaconda3\Lib\site-packages\psutil\_psutil_windows.cp311-win_amd64.pyd or clean the virtual env somehow, then reinstall
```

What did I change??
- It cannot be the ORB-SLAM, it is in a virtual machine.
Makes no sense, there is no change in the PlayGround repo:
![[Pasted image 20240626214641.png]]

Unless it is the jekyll shit...

2024-06-26T21:53:16-07:00
Tried running pip in cmd in a different folder, and it installed supervision, no access denied error... WTF is this. I hate windows.
![[Pasted image 20240626215410.png]]

Now `ultralytics`.
- `pip install ultralytics`
- it is running. A lot of warnings.
- ok seems fine
![[Pasted image 20240626215711.png]]

2024-06-26T22:46:42-07:00
Trying to shrink the bounding boxes, found that shrinking just one does not work:
- `detections[0].xyxy = detections[0].xyxy*0.8` does not work
- `detections.xyxy[0] = detections.xyxy[0]*0.8` this one works

2024-06-26T23:16:05-07:00
Looked around trying to find hardware and scientific specific datasets.
ChatGPT had some recommendation:
- DeepPCB Dataset: https://labelbox.com/datasets/deeppcb-printed-circuit-board-pcb-defect-dataset/
Also found one from roboflow:
- https://universe.roboflow.com/resistancecapacitancediode-rauxj/electronic-ckryv
- downloaded zip. Will try to test it out

# Test pretrained specialized model

2024-06-26T23:17:49-07:00
Trying to play with the electronics dataset
- https://universe.roboflow.com/resistancecapacitancediode-rauxj/electronic-ckryv

Worked on the web (kind of):
![[Pasted image 20240626233049.png]]

I got the zip file. Let's see...
Ok found the instructions
- https://inference.roboflow.com/quickstart/explore_models/#run-a-private-fine-tuned-model
- ok this tutorial is not helpful, it assumes you already have a model setup and trained in the workspace and project




