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

