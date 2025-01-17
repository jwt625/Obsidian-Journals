
# Install
2025-01-15T22:04:25-08:00
Ref: https://github.com/apple/ml-mobileclip


Not going to use conda.

```
python3 -m venv clipenv
source clipenv/bin/activate
git clone https://github.com/apple/ml-mobileclip.git
pip install -e .

```

Error:
```
ERROR: Ignored the following yanked versions: 0.1.6, 0.1.7, 0.1.8, 0.1.9, 0.2.0, 0.2.1, 0.2.2, 0.2.2.post2, 0.2.2.post3, 0.15.0

ERROR: Could not find a version that satisfies the requirement torchvision==0.14.1 (from mobileclip) (from versions: 0.15.1, 0.15.2, 0.16.0, 0.16.1, 0.16.2, 0.17.0, 0.17.1, 0.17.2, 0.18.0, 0.18.1, 0.19.0, 0.19.1, 0.20.0, 0.20.1)

ERROR: No matching distribution found for torchvision==0.14.1
```

Ignore this.

2025-01-15T22:09:11-08:00
Downloading model: `source get_pretrained_models.sh   # Files will be downloaded to `checkpoints` directory.`
Error:
```
get_pretrained_models.sh:7: command not found: wget get_pretrained_models.sh:8: command not found: wget get_pretrained_models.sh:9: command not found: wget get_pretrained_models.sh:10: command not found: wget get_pretrained_models.sh:11: command not found: wget
```

Installing `wget`:
- `brew install wget`
- done
2025-01-15T22:10:37-08:00
Downloading:
![[Pasted image 20250115221040.png]]

Seems it ran fine.


Extra packages:
- pip install torch
- pip install pillow
- pip install torchvision
- pip install timm
- pip install open_clip_torch

2025-01-15T22:32:37-08:00
Got the test script running:

![[Pasted image 20250115223243.png]]

Ok what does this script do and what is next?

Damn it works:
![[Pasted image 20250115224237.png]]
![[Pasted image 20250115224245.png]]
- The image is me holding a compound bow

# Processing and searching
2025-01-15T23:14:44-08:00
Done processing:
![[Pasted image 20250115231504.png]]
- tested one folder from 2019

## Search
`pip install scikit-learn`

2025-01-15T23:22:39-08:00

Yes search is also working!!
![[Pasted image 20250115232226.png]]

Going to index all folders under photos.
2025-01-15T23:31:09-08:00
Started. Going to see how long it takes.
There's probably more than 10k images. Right now it seems to be doing ~ 3 images per second. It will take ~ 1 h.
2025-01-16T00:39:43-08:00
Broke a while ago because of a file opening error. Added try catch and restarted.

2025-01-16T08:44:47-08:00
Done indexing. Search is now ~ 11s on all 14k photons from the last 10 years.



















































