
# Installation

2025-02-25T09:22:42-08:00
Following https://github.com/behretj/LostFound

Got conda...

2025-02-25T09:28:20-08:00
![[Pasted image 20250225092823.png]]

Keep getting this error... Tried fixing all bashrc and zshrc in vail. Going to not use conda. Fuck conda.

```
brew update
brew install pyenv
brew install pyenv-virtualenv

```

`pyenv install 3.10.12`

2025-02-26T00:18:40-08:00
`pyenv virtualenv 3.10.12 lost_found`
`pyenv activate lost_found`
`pip install torch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2 --extra-index-url https://download.pytorch.org/whl/cu117`


2025-02-26T00:24:07-08:00

Model weights download did not work:
```
mkdir -p thirdparty/detector/models/res101_handobj_100K/pascal_voc
cd thirdparty/detector/models/res101_handobj_100K/pascal_voc
gdown https://drive.google.com/uc?id=1H2tWsZkS7tDF8q1-jdjx6V9XrK25EDbE
cd ../../../../..
```

![[Pasted image 20250226002408.png]]

Did it manually.

CoTracker2 weights download with their code in readme worked.

2025-02-26T00:25:36-08:00
Moment of truth:
`python run_demo.py `

Nope a bunch of packages not working. pip installed shit into a different python, instead of this lost_found python environment.
Rerun prev installation code in vscode and interactive window after choosing the kernel.

![[Pasted image 20250226002833.png]]

The following two did not run properly:
```
# install and build dependencies for co-tracker
cd thirdparty/cotracker && pip install -e . && cd ../..

# install and build dependencies for hand-object-detector
cd thirdparty/detector/lib && python setup.py build develop && cd ../../..
```
- complaining about directory not found...

This run fine:
![[Pasted image 20250226003053.png]]

Wow, amazing error when importing numpy:
![[Pasted image 20250226003330.png]]

oh it's actually about cv2.

pip in outside terminal vs pip in vscode terminal:
![[Pasted image 20250226003441.png]]

![[Pasted image 20250226003449.png]]

Going to rerun the above install that failed.
![[Pasted image 20250226003530.png]]


Done.

2025-02-26T00:38:37-08:00

Same error when importing cv2.
Resolved with `pip install "numpy<2"`

![[Pasted image 20250226003954.png]]

Seems like the detector lib folder and dependencies are missing...

Maybe the model should be downloaded from somewhere?

Seems like the detector is from a different repo:
- https://github.com/behretj/LostFound/tree/main/thirdparty/detector
- ok make sense because it is third party





