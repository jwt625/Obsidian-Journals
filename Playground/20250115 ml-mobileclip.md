
# Install
2025-01-15T22:04:25-08:00

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




















































