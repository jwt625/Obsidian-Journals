2025-04-11T20:27:01-07:00

Reference:
- https://github.com/microsoft/OmniParser

```
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
```
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
```
  

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


