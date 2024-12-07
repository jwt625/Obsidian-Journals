2024-12-06T23:15:13-08:00

https://github.com/microsoft/TRELLIS

Following the installation steps.

2024-12-06T23:33:17-08:00
Had to add the following alias to `setup.sh`
```
alias python="python3"

alias python3.9="/usr/bin/python3"

alias python3.11="/usr/local/bin/python3"

alias python3.13="/opt/homebrew/bin/python3.13"
```

Also had to pip3 install torch.

2024-12-06T23:33:55-08:00
Ok the setup seems to run:
- `. ./setup.sh --new-env --basic --xformers --flash-attn --diffoctreerast --spconv --mipgaussian --kaolin --nvdiffrast`

2024-12-06T23:39:58-08:00
Now running the example.py.
Had to
- `pip install tqdm`
- `pip install easydict`
- `pip install torchvision`
- `pip install rembg`
- `pip install onnxruntime`
```
pip install torch
pip install tqdm
pip install utils3d
```

Jesus christ why:
![[Pasted image 20241206234221.png]]
Claude asked to create a new env. I'm not doing that.
Did `pip install numpy==2.0.0`.

New error:
![[Pasted image 20241206235319.png]]
```
pip install plyfile
pip install utils3d
```

New error
![[Pasted image 20241206235456.png]]
`pip install trimesh`

2024-12-06T23:57:30-08:00
Hmm maybe it has to be using an nvidia card??
![[Pasted image 20241207001312.png]]
Ok yes...








