
2025-07-08T21:59:41-07:00

Had to download RealESRGAN

```
cd /home/ubuntu/GitHub/Hunyuan3D-2.1 && mkdir -p hy3dpaint/ckpt && wget https://github.com/xinntao/Real-ESRGAN/releases/download/v0.1.0/RealESRGAN_x4plus.pth -P hy3dpaint/ckpt
```


```
 cd /home/ubuntu/GitHub/Hunyuan3D-2.1 && source venv/bin/activate && python h100_optimized_example.py --image ./images/MFG_CONSMP001-SMD-G-T.jpg --profile maximum
InPaint Function CAN NOT BE Imported!!!
Torchvision version: 0.20.1+cu124
torchvision.transforms.functional_tensor not found, applying compatibility fix...
Applied compatibility fix: created functional_tensor mock module
✅ Applied torchvision compatibility fix
🖥️  GPU: NVIDIA H100 80GB HBM3
💾 Total VRAM: 79.2GB
🎉 H100 80GB detected - Ultimate Hunyuan3D-2.1 performance!
✅ Enabled Flash Attention
🚀 Applied H100 80GB performance optimizations
🎛️  Using maximum profile: Maximum quality - full H100 potential
📁 Output directory: /home/ubuntu/GitHub/Hunyuan3D-2.1/output
📷 Loading image: ./images/MFG_CONSMP001-SMD-G-T.jpg
🏗️  Initializing shape generation pipeline (H100 optimized)...
2025-07-09 04:58:07,383 - hy3dgen.shapgen - INFO - Try to load model from local path: /home/ubuntu/.cache/hy3dgen/tencent/Hunyuan3D-2.1/hunyuan3d-dit-v2-1
2025-07-09 04:58:07,388 - hy3dgen.shapgen - INFO - Loading model from /home/ubuntu/.cache/hy3dgen/tencent/Hunyuan3D-2.1/hunyuan3d-dit-v2-1/model.fp16.ckpt
using moe
using moe
using moe
using moe
using moe
using moe
PointCrossAttentionEncoder INFO: pc_sharpedge_size is zero
🎯 Generating 3D shape...
Diffusion Sampling:: 100%|███████████████████████████████████████████████████████████| 50/50 [00:05<00:00,  9.73it/s]
Volume Decoding: 100%|██████████████████████████████████████████████████████████| 7134/7134 [00:08<00:00, 806.13it/s]
⏱️  Shape generation completed in 15.8s
💾 Saving untextured mesh to: output/untextured_mesh.glb
✅ Shape generation completed!
🎨 Initializing texture generation (H100 Profile: Maximum quality - full H100 potential)...
📊 Settings: 12 views, 768px resolution
🔧 Render: 2048px, Texture: 4096px
preprocessor_config.json: 100%|█████████████████████████████████████████████████████| 342/342 [00:00<00:00, 4.69MB/s]
config.json: 100%|██████████████████████████████████████████████████████████████████| 554/554 [00:00<00:00, 7.28MB/s]
README.md: 1.80kB [00:00, 12.8MB/s]                                                        | 0.00/554 [00:00<?, ?B/s]
scheduler_config.json: 100%|████████████████████████████████████████████████████████| 387/387 [00:00<00:00, 5.43MB/s]
config.json: 100%|██████████████████████████████████████████████████████████████████| 613/613 [00:00<00:00, 8.46MB/s]
model_index.json: 100%|█████████████████████████████████████████████████████████████| 617/617 [00:00<00:00, 9.34MB/s]
attn_processor.py: 35.1kB [00:00, 89.0MB/s]                                                | 0.00/617 [00:00<?, ?B/s]
merges.txt: 525kB [00:00, 23.2MB/s]                                                      | 0.00/1.36G [00:00<?, ?B/s]
vocab.json: 1.06MB [00:00, 41.5MB/s]                                                     | 0.00/1.26G [00:00<?, ?B/s]
config.json: 100%|██████████████████████████████████████████████████████████████████| 911/911 [00:00<00:00, 13.6MB/s]
special_tokens_map.json: 100%|██████████████████████████████████████████████████████| 460/460 [00:00<00:00, 1.04MB/s]
tokenizer_config.json: 100%|████████████████████████████████████████████████████████| 807/807 [00:00<00:00, 2.81MB/s]
modules.py: 45.2kB [00:00, 131MB/s]                                                        | 0.00/911 [00:00<?, ?B/s]
model.py: 25.5kB [00:00, 107MB/s]                                                          | 0.00/460 [00:00<?, ?B/s]
config.json: 100%|██████████████████████████████████████████████████████████████████| 553/553 [00:00<00:00, 8.23MB/s]
diffusion_pytorch_model.bin: 100%|█████████████████████████████████████████████████| 335M/335M [00:01<00:00, 210MB/s]
model.safetensors: 100%|█████████████████████████████████████████████████████████| 1.26G/1.26G [00:05<00:00, 222MB/s]
pytorch_model.bin: 100%|█████████████████████████████████████████████████████████| 1.36G/1.36G [00:06<00:00, 211MB/s]
diffusion_pytorch_model.bin: 100%|███████████████████████████████████████████████| 3.93G/3.93G [00:20<00:00, 189MB/s]
Fetching 19 files: 100%|█████████████████████████████████████████████████████████████| 19/19 [00:21<00:00,  1.11s/it]
Loading pipeline components...: 100%|██████████████████████████████████████████████████| 6/6 [00:08<00:00,  1.46s/it]
preprocessor_config.json: 100%|█████████████████████████████████████████████████████| 436/436 [00:00<00:00, 4.62MB/s]
config.json: 100%|██████████████████████████████████████████████████████████████████| 548/548 [00:00<00:00, 6.08MB/s]
Xet Storage is enabled for this repo, but the 'hf_xet' package is not installed. Falling back to regular HTTP download. For better performance, install the package with: `pip install huggingface_hub[hf_xet]` or `pip install hf_xet`
model.safetensors: 100%|█████████████████████████████████████████████████████████| 4.55G/4.55G [00:12<00:00, 366MB/s]
Models Loaded.
🖌️  Generating texture...
```

2025-07-08T22:03:15-07:00
the fuck
`
❌ Error: name 'meshVerticeInpaint' is not defined`

```
cd /home/ubuntu/GitHub/Hunyuan3D-2.1/hy3dpaint/DifferentiableRenderer && source ../../venv/bin/activate && bash compile_mesh_painter.sh
```

2025-07-08T22:17:49-07:00
Working!

```

🖌️  Generating texture...
Loaded image from: 'output/textured_mesh.jpg'
Loaded image from: 'output/textured_mesh_metallic.jpg'
Loaded image from: 'output/textured_mesh_roughness.jpg'
OBJ import of 'textured_mesh.glb' took 12.7 ms
05:18:37 | ERROR: Draco mesh compression is not available because library could not be found at /home/ubuntu/GitHub/Hunyuan3D-2.1/4.0/python/lib/python3.10/site-packages/libextern_draco.so
05:18:37 | INFO: Starting glTF 2.0 export
05:18:37 | INFO: Extracting primitive: textured_mesh
05:18:37 | WARNING: More than one shader node tex image used for a texture. The resulting glTF sampler will behave like the first shader node tex image.
05:18:37 | INFO: Primitives created: 1
05:18:37 | INFO: Finished glTF 2.0 export in 0.4498422145843506 s

⏱️  Texture generation completed in 63.2s
✅ Texture generation completed!
🎉 Final textured mesh: output/textured_mesh.glb
📊 Peak GPU memory usage: 23.5GB / 79.2GB (29.6%)

🎊 Generation completed successfully!
📂 Check 'output' for your 3D models
Error: Not freed memory blocks: 28208, total unfreed memory 66.191013 MB
```
Input:
![[MFG_CONSMP001-SMD-G-T.jpg]]
Output:
![[Pasted image 20250708221751.png]]
Damn not bad at all.
