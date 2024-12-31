2024/05/17, 2:30 pm
Following https://github.com/facebookresearch/segment-anything

Got it installed on macOS laptop
![[Pasted image 20240517142842.png]]


Installing onnx and onnxruntime... Done.
Installing torch, torchvision, and cv2. Cannot find cv2.
Turns out it should be `pip install opencv-python`

Get most of the ipynotebook running.

5:20 pm, trying to figure out what ONNX is: https://onnx.ai/index.html

2024-07-02T20:37:02-07:00
Revisit. Should I figure it out on the windows desktop, or keep using the macbook?
Maybe I'll stay on the macbook.

Seems like check point already downloaded: `sam_vit_h_4b8939.pth`.
2024-07-02T21:11:22-07:00
I'm really suspecting jekyll fucked up the ipynb for some reason. It definitely ran before on my macbook:
![[Pasted image 20240702211151.png]]
- fixed it. Well let's find out later if this breaks the jekyll.

2024-07-02T21:13:29-07:00
- ran `pip install git+https://github.com/facebookresearch/segment-anything.git`
- Seems to have installed it fine:
![[Pasted image 20240702211402.png]]

Why is the status always pending when I run section in the jupyter notebook??
- going to copy all the shit out into a single python script.
- `no module named torch`
- WTF??
- going to reinstall it.
- It has to be a path issue

Rerunning all the old installs
- `pip install torch`
- `pip install torchvision`
- `pip install opencv-python`
- `pip install matplotlib`
- `pip install onnxruntime`
- `pip install onnx`
Complaining about cv2:
![[Pasted image 20240702212400.png]]

- actually this is an old error. See above.

2024-07-02T21:32:51-07:00
Ok we are finally back to the point where all the imports work.
- including the segment anything stuff!
- not sure why the syntax highlight is still not happy.
![[Pasted image 20240702213325.png]]
Going to try the "generating masks for an entire image":
```
from segment_anything import SamAutomaticMaskGenerator, sam_model_registry
sam = sam_model_registry["<model_type>"](checkpoint="<path/to/checkpoint>")
mask_generator = SamAutomaticMaskGenerator(sam)
masks = mask_generator.generate(<your_image>)
```
- WTF should the model_type be??
- asked chatGPT. It should be whatever the checkpoint uses.
- it is also in the example jupyter notebook. You blind fuck.

2024-07-02T21:50:00-07:00
Ok the moment of truth.
- fuck, of course there is going to be some error.
- `'str' object has no attribute 'shape'`
Ok going to try whatever chatGPT recommend:
```
import cv2
from segment_anything import SamAutomaticMaskGenerator, sam_model_registry

model_type = "vit_b"
checkpoint_path = "<path/to/checkpoint>"

sam = sam_model_registry[model_type](checkpoint=checkpoint_path)
mask_generator = SamAutomaticMaskGenerator(sam)

# Load image using OpenCV
image_path = "signal-2024-07-02-214900_002.jpeg"
image = cv2.imread(image_path)
image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)  # Convert BGR to RGB

# Generate masks
masks = mask_generator.generate(image)

# Print or use the masks as needed
print(masks)

```

- damn it got the correct image name from that error message.
2024-07-02T21:56:40-07:00
- seems like it has been running. At least 1 min now.
- ok let's find out what the fuck it generated:
![[Pasted image 20240702215728.png]]
- need a bit of back and forth to let chatGPT figure out the structure of the returned mask object.
- hmm why is the plot taking more than a few sec...

2024-07-02T22:03:28-07:00
Fuck yeaaaahh!!! It is working!!!
![[Pasted image 20240702220338.png]]

2024-07-02T22:37:14-07:00
Seems like the masks are sorted by area, which is nice. Going to just plot the top 20, and see if it is better/faster to plot.
Why is it sooo slow...
2024-07-02T22:45:07-07:00
![[Pasted image 20240702224508.png]]

Going to also try on the batteries.
2024-07-02T22:48:21-07:00 Started ~ 30 s ago.
2024-07-02T22:50:37-07:00 Done.



# 20241108, SAM2

2024-11-08T21:35:54-08:00

https://github.com/facebookresearch/sam2

Ugh need an older version:
- https://www.python.org/downloads/macos/

Following Claude and got 3.11:
```
python3.11 -m venv torch_env
source torch_env/bin/activate

pip3 install torch torchvision torchaudio

pip install ipykernel
python -m ipykernel install --user --name=torch_env
```


Installing opencv:
```
pip install opencv-python
```


2024-11-08T22:37:14-08:00
Ok now continuing SAM2 installation:
![[Pasted image 20241108223736.png]]
Finished fine:
![[Pasted image 20241108223844.png]]


As well as
```
pip install -e ".[notebooks]"
```

Done.


2024-11-08T22:39:46-08:00

Downloading checkpoint:
```
cd checkpoints && \
./download_ckpts.sh && \
cd ..
```

![[Pasted image 20241108224053.png]]

## Static image
2024-11-08T22:41:43-08:00

https://github.com/facebookresearch/sam2/blob/main/notebooks/image_predictor_example.ipynb
2024-11-08T22:51:38-08:00
Ran thru the whole notebook, everything is working!
Should read thru it tmw.


# Installing SAM2 on new mac mini

2024-12-30T21:18:21-08:00

```bash
git clone https://github.com/facebookresearch/sam2.git && cd sam2
pip3 install -e .
```

Seems to run fine.

Damn I really need the NAS to access all the pictures.

`pip install -e ".[notebooks]"`

2024-12-30T21:21:50-08:00

Downloading check point...

2024-12-30T21:31:49-08:00
Done. Running example note book `automatic_mask_generator_example.ipynb`.

It took 38 s on the example image:
![[Pasted image 20241230213338.png]]
- it also complained about torch is using CPU.
- hmm...

The next step ran out of memory..

2024-12-30T21:44:23-08:00
Testing the predictor, seems to run much faster:
![[Pasted image 20241230214443.png]]

Good to know a mask from previous iteration could be used again.



