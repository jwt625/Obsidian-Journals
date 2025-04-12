2025-04-11T20:27:01-07:00

Reference:
- https://github.com/microsoft/OmniParser
- 2025-04-11T21:07:53-07:00 seems like there is a newer one:
- https://huggingface.co/microsoft/OmniParser-v2.0/blob/main/README.md



```shell
cd OmniParser
conda create -n "omni" python==3.12
conda activate omni
pip install -r requirements.txt
```

2025-04-11T20:31:23-07:00
Installing quite some shit.
```
Successfully installed PyJWT-2.10.1 Shapely-2.1.0 accelerate-1.6.0 aiofiles-23.2.1 aiohappyeyeballs-2.6.1 aiohttp-3.11.16 aiosignal-1.3.2 albucore-0.0.23 albumentations-2.0.5 altair-5.5.0 annotated-types-0.7.0 anthropic-0.49.0 anyio-3.7.1 astor-0.8.1 attrs-25.3.0 azure-core-1.33.0 azure-identity-1.21.0 beautifulsoup4-4.13.3 blinker-1.9.0 boto3-1.37.33 botocore-1.37.33 cachetools-5.5.2 certifi-2025.1.31 cffi-1.17.1 cfgv-3.4.0 charset-normalizer-3.4.1 click-8.1.8 comtypes-1.4.10 contourpy-1.3.1 cryptography-44.0.2 cycler-0.12.1 cython-3.0.12 dashscope-1.23.1 decorator-5.2.1 defusedxml-0.7.1 dill-0.3.9 distlib-0.3.9 distro-1.9.0 easyocr-1.7.2 einops-0.8.0 fastapi-0.115.12 ffmpy-0.5.0 filelock-3.18.0 fire-0.7.0 fonttools-4.57.0 frozenlist-1.5.0 fsspec-2025.3.2 gitdb-4.0.12 gitpython-3.1.44 google-auth-2.38.0 gradio-5.13.2 gradio-client-1.6.0 groq-0.22.0 h11-0.14.0 httpcore-1.0.8 httpx-0.28.1 huggingface-hub-0.30.2 identify-2.6.9 idna-3.10 imageio-2.37.0 iniconfig-2.1.0 jinja2-3.1.6 jiter-0.9.0 jmespath-1.0.1 jsonschema-4.22.0 jsonschema-specifications-2024.10.1 kiwisolver-1.4.8 lazy-loader-0.4 lmdb-1.6.2 lxml-5.3.2 markdown-it-py-3.0.0 markupsafe-2.1.5 matplotlib-3.10.1 mdurl-0.1.2 mouseinfo-0.1.3 mpmath-1.3.0 msal-1.32.0 msal-extensions-1.3.1 multidict-6.4.3 narwhals-1.34.1 networkx-3.4.2 ninja-1.11.1.4 nodeenv-1.9.1 numpy-1.26.4 openai-1.3.5 opencv-contrib-python-4.11.0.86 opencv-python-4.11.0.86 opencv-python-headless-4.11.0.86 opt-einsum-3.3.0 orjson-3.10.16 packaging-24.2 paddleocr-2.10.0 paddlepaddle-3.0.0 pandas-2.2.3 pillow-11.1.0 platformdirs-4.3.7 pluggy-1.5.0 pre-commit-3.8.0 propcache-0.3.1 protobuf-5.29.4 psutil-7.0.0 py-cpuinfo-9.0.0 pyarrow-19.0.1 pyasn1-0.6.1 pyasn1-modules-0.4.2 pyautogui-0.9.54 pyclipper-1.3.0.post6 pycparser-2.22 pydantic-2.11.3 pydantic-core-2.33.1 pydeck-0.9.1 pydub-0.25.1 pygetwindow-0.0.9 pygments-2.19.1 pymsgbox-1.0.9 pyobjc-core-11.0 pyobjc-framework-Cocoa-11.0 pyobjc-framework-quartz-11.0 pyparsing-3.2.3 pyperclip-1.9.0 pyrect-0.2.0 pyscreeze-1.0.1 pytest-8.3.3 pytest-asyncio-0.23.6 python-bidi-0.6.6 python-dateutil-2.9.0.post0 python-docx-1.1.2 python-multipart-0.0.20 pytweening-1.2.0 pytz-2025.2 pyyaml-6.0.2 rapidfuzz-3.13.0 referencing-0.36.2 regex-2024.11.6 requests-2.32.3 rich-14.0.0 rpds-py-0.24.0 rsa-4.9 rubicon-objc-0.5.0 ruff-0.6.7 s3transfer-0.11.4 safehttpx-0.1.6 safetensors-0.5.3 scikit-image-0.25.2 scipy-1.15.2 screeninfo-0.8.1 seaborn-0.13.2 semantic-version-2.10.0 shellingham-1.5.4 simsimd-6.2.1 six-1.17.0 smmap-5.0.2 sniffio-1.3.1 soupsieve-2.6 starlette-0.46.1 streamlit-1.44.1 stringzilla-3.12.3 supervision-0.18.0 sympy-1.13.1 tenacity-9.1.2 termcolor-3.0.1 tifffile-2025.3.30 timm-1.0.15 tokenizers-0.21.1 toml-0.10.2 tomlkit-0.13.2 torch-2.6.0 torchvision-0.21.0 tornado-6.4.2 tqdm-4.67.1 transformers-4.51.2 typer-0.15.2 typing-extensions-4.13.2 typing-inspection-0.4.0 tzdata-2025.2 uiautomation-2.0.24 ultralytics-8.3.70 ultralytics-thop-2.0.14 urllib3-2.4.0 uvicorn-0.34.0 virtualenv-20.30.0 websocket-client-1.8.0 websockets-14.2 yarl-1.19.0
```

2025-04-11T20:35:32-07:00
Downloading model:
```shell
for f in icon_detect/{train_args.yaml,model.pt,model.yaml} icon_caption/{config.json,generation_config.json,model.safetensors}; do huggingface-cli download microsoft/OmniParser-v2.0 "$f" --local-dir weights; done
   mv weights/icon_caption weights/icon_caption_florence
```

2025-04-11T20:37:26-07:00
ok moment of truth:
`python gradio_demo.py`
More shit is being installed:
![[Pasted image 20250411203823.png]]

Error
![[Pasted image 20250411203945.png]]

Also why is it using a public url?

It uses cuda but let's try mps.

Seems to import yolo fine.

2025-04-11T20:50:08-07:00
ok the actual parse part is not working
```python
  

dino_labled_img, label_coordinates, parsed_content_list = get_som_labeled_img(image_path, som_model, BOX_TRESHOLD = BOX_TRESHOLD,

output_coord_in_ratio=True, ocr_bbox=ocr_bbox,draw_bbox_config=draw_bbox_config,

caption_model_processor=caption_model_processor, ocr_text=text,use_local_semantics=True,

iou_threshold=0.7, scale_img=False, batch_size=128)
```

The OCR  before this section worked.

Going to try vLLM as suggested by hugging face.
![[Pasted image 20250411205320.png]]
2025-04-11T20:54:13-07:00
- `pip install vllm`
- ok it is installing another whole bunch of crap
```
Successfully installed airportsdata-20250224 blake3-1.0.4 cloudpickle-3.1.1 compressed-tensors-0.9.2 datasets-3.5.0 depyf-0.18.0 dill-0.3.8 diskcache-5.6.3 dnspython-2.7.0 email-validator-2.2.0 fastapi-cli-0.0.7 fsspec-2024.12.0 gguf-0.10.0 hf-xet-1.0.3 httptools-0.6.4 interegular-0.3.3 lark-1.2.2 llguidance-0.7.14 lm-format-enforcer-0.10.11 mistral_common-1.5.4 msgspec-0.19.0 multiprocess-0.70.16 openai-1.72.0 outlines-0.1.11 outlines_core-0.1.26 partial-json-parser-0.2.1.1.post5 prometheus-fastapi-instrumentator-7.1.0 prometheus_client-0.21.1 pycountry-24.6.1 python-dotenv-1.1.0 python-json-logger-3.3.0 rich-toolkit-0.14.1 sentencepiece-0.2.0 tiktoken-0.9.0 torchaudio-2.6.0 uvloop-0.21.0 vllm-0.8.3 watchfiles-1.0.5 xxhash-3.5.0
```

Tokenizer not working with this simple test:
```python
tokenizer = AutoTokenizer.from_pretrained("microsoft/OmniParser")
print(tokenizer)
```

how the fuck am I supposed to use a model from huggingface??
- https://www.youtube.com/watch?v=ntz160EnWIc
- ok let's try this ancient example from 2022.
- seems to run fine:
```python
from transformers import pipeline
classifier = pipeline("sentiment-analysis")
```
- it even detected and is using mps.
![[Pasted image 20250411211110.png]]
holy shit how is this so easy.

Ok checking Pmniparser videos
- https://www.youtube.com/watch?v=SO67lDhkvJg

Need huggingface-cli login. Need token.

Damn last time I generated a token was for stable-diffusion
![[Pasted image 20250411212403.png]]

2025-04-11T21:26:22-07:00
logged in.
Downloading:
- first part is the same as before: 
```shell
 for f in icon_detect/{train_args.yaml,model.pt,model.yaml} icon_caption/{config.json,generation_config.json,model.safetensors}; 
 do huggingface-cli download microsoft/OmniParser-v2.0 "$f" --local-dir weights; done
```

2025-04-11T21:31:32-07:00
ok trying `python3 gradio_demo.py` again.
- ok missed this rename step: `mv weights/icon_caption weights/icon_caption_florence`

2025-04-11T21:36:35-07:00
Getting the same "bool" is not iterable error.

Fucking cuda man, changed to mps:
![[Pasted image 20250411213759.png]]
- ugh ok still the same errors.

2025-04-11T21:41:57-07:00
ok quickly read the gradio_demo.py. It is basically calling the process function, in which it is calling the same two functions `check_ocr_box` and `get_som_labeled_img` that are used in the ipynb.

2025-04-11T21:50:47-07:00
Hell yeah fixed it!!!!
- `inputs["pixel_values"] = inputs["pixel_values"].half()`
- added to the `utils.py`
![[Pasted image 20250411215114.png]]

Time for comsol.
- it is so freaking slow though
- damn taking way longer than the word example (~< 1 min)

2025-04-11T22:00:23-07:00
it fucking froze my M4 pro mini. Pathetic.
```
image size: (1474, 928) WARNING ⚠️ NMS time limit 2.050s exceeded 0: 832x1280 250 icons, 50678.0ms Speed: 8479.7ms preprocess, 50678.0ms inference, 2422.7ms postprocess per image at shape (1, 3, 832, 1280) len(filtered_boxes): 239 90
```

Going to run on just a subset...
- let's try the teams example first.
	- still taking more than 2 min and computer is getting glitchy...
- going to try this smaller comsol
![[Screenshot 2025-04-11 at 22.04.54.png]]
2025-04-11T22:08:28-07:00
ok it worked:
![[Pasted image 20250411220830.png]]







