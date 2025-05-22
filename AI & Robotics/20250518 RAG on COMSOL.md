2025-05-18T22:05:58-07:00

Where are the docs:
![[Pasted image 20250518220614.png]]

2025-05-18T22:28:45-07:00

`pip install pymupdf langchain faiss-cpu openai`

2025-05-18T22:34:58-07:00
Also need `pip install -U langchain-community`.
Hell yea:
![[Pasted image 20250518223531.png]]

2025-05-18T22:39:53-07:00
I do not have openAI's API key for `OpenAIEmbeddings`.
Checking if deepseek has text embedding.
- https://www.reddit.com/r/LocalLLaMA/comments/1iyun6z/using_deepseek_r1_for_rag_dos_and_donts/
![[Pasted image 20250518224047.png]]
- https://github.com/skypilot-org/skypilot/tree/master/llm/rag
- https://blog.skypilot.co/deepseek-rag/

2025-05-18T22:55:20-07:00
Fuck big models:
![[Pasted image 20250518225523.png]]
 Using small model
 - `pip install sentence-transformers`
 - `pip install sentence-transformers accelerate`

2025-05-18T23:04:10-07:00
Keep getting credential related error hmm.
- general, even get the error when I try the example:
```python
from sentence_transformers import SentenceTransformer

model = SentenceTransformer("sentence-transformers/all-MiniLM-L6-v2")

sentences = [
    "That is a happy person",
    "That is a happy dog",
    "That is a very happy person",
    "Today is a sunny day"
]
embeddings = model.encode(sentences)

similarities = model.similarity(embeddings, embeddings)
print(similarities.shape)
# [4, 4]
```


2025-05-18T23:41:56-07:00
Still cannot figure out the fucking issue with the token. WTF.


# Continue on cluster
2025-05-21T22:19:01-07:00
Also going to switch to vLLM.
`pip install vllm transformers`

2025-05-21T22:28:23-07:00
Creating a venv.
```
pip install numpy scipy pandas scikit-learn
pip install sentence-transformers
pip install langchain langchain-community vllm faiss-cpu  # or faiss-gpu if needed

```

`python -m ipykernel install --user --name=rag-env --display-name "Python (rag-env)"`

2025-05-21T22:35:26-07:00
ok it seems to be fetching.

![[Pasted image 20250521223553.png]]

2025-05-21T22:40:32-07:00
GPU is now running with 13% utilization.
2025-05-21T22:53:09-07:00
Still going. Feels too long?
![[Pasted image 20250521225432.png]]
ok >10 min is bullshit.

2025-05-21T23:01:09-07:00
Turns out embedding is very fast, the FAISS index construction is slow.
Trying chromaDB
`pip install chromadb`

Old:
```python
# Build vectorstore (can be swapped with Chroma if needed)
vectorstore = FAISS.from_documents(documents, embeddings)
# Save to local folder
vectorstore.save_local("faiss_index")
```

New:
```python
from langchain.vectorstores import Chroma

vectorstore = Chroma.from_documents(documents, embedding=embeddings, persist_directory="chroma_db")
vectorstore.persist()
```

Hmm should add a progress bar.
![[Pasted image 20250521230641.png]]
![[Pasted image 20250521230652.png]]
Wow this is going to take a while damn.

2025-05-21T23:14:49-07:00
Still slow even after trying batching.
Going to switch away from langChain.
`pip install transformers torch faiss-gpu tqdm`

```python
import torch
from transformers import AutoTokenizer, AutoModel
from torch.utils.data import DataLoader
import numpy as np
import faiss
from tqdm import tqdm
import os

# === CONFIG ===
MODEL_NAME = "Alibaba-NLP/gte-Qwen2-7B-instruct"
BATCH_SIZE = 64
CHUNK_SIZE = 512
EMBED_DIM = 4096  # Depends on model
DEVICE_COUNT = torch.cuda.device_count()

# === Load model/tokenizer on all devices ===
tokenizer = AutoTokenizer.from_pretrained(MODEL_NAME)
model = AutoModel.from_pretrained(MODEL_NAME, torch_dtype=torch.float16).half().eval()

# === Read and chunk your documents ===
from langchain.docstore.document import Document
from langchain.text_splitter import RecursiveCharacterTextSplitter

with open("extracted_texts.txt", "r", encoding="utf-8") as f:
    texts = f.read().split("=== Document ")[1:]
    texts = [doc.split("\n", 1)[1] for doc in texts]

splitter = RecursiveCharacterTextSplitter(chunk_size=CHUNK_SIZE, chunk_overlap=100)
documents = [chunk for text in texts for chunk in splitter.split_text(text)]

# === Embed with batching on one GPU (parallelize later) ===
def embed_batch(batch_texts):
    inputs = tokenizer(batch_texts, return_tensors="pt", padding=True, truncation=True, max_length=CHUNK_SIZE)
    inputs = {k: v.cuda() for k, v in inputs.items()}
    with torch.no_grad():
        outputs = model(**inputs, output_hidden_states=True)
        embeddings = outputs.last_hidden_state.mean(dim=1)
        return embeddings.cpu().numpy()

# === Loop over batches and embed ===
all_embeddings = []
for i in tqdm(range(0, len(documents), BATCH_SIZE), desc="Embedding"):
    batch = documents[i:i + BATCH_SIZE]
    batch_texts = [doc for doc in batch]
    emb = embed_batch(batch_texts)
    all_embeddings.append(emb)

all_embeddings = np.vstack(all_embeddings).astype("float32")

# === Build FAISS GPU index ===
res = faiss.StandardGpuResources()
index = faiss.IndexFlatL2(EMBED_DIM)
gpu_index = faiss.index_cpu_to_gpu(res, 0, index)  # Use GPU 0 or loop over all

gpu_index.add(all_embeddings)

# === Save index and docs ===
faiss.write_index(faiss.index_gpu_to_cpu(gpu_index), "faiss_index.index")
np.save("documents.npy", np.array(documents, dtype=object))

```
2025-05-21T23:18:40-07:00
ok this seems much faster.
![[Pasted image 20250521231854.png]]

faiss-cpu also exists and is messing around stuff...
```
pip uninstall numpy faiss-gpu -y
pip install numpy --upgrade --force-reinstall
pip install faiss-gpu --upgrade --force-reinstall

```
did not work.
rm -rf and reinstalling
```bash
pip install --upgrade pip setuptools wheel
pip install numpy  # install this first to avoid faiss breakage
pip install faiss-gpu
pip install torch transformers tqdm

```

Add new env to ipynb kernel: `python -m ipykernel install --user --name=rag-env2 --display-name "Python (rag-env2)"`

`jupyter kernelspec uninstall rag-env`

2025-05-21T23:56:42-07:00
I'm building faiss from source... Ran into quite some error messages. 4o is pretty capable.
![[Pasted image 20250521235753.png]]





