
# STT

2025-05-25T20:00:14-07:00
Already got a chat cli tool to ask ai questions. Should also get voice input working.

`brew install portaudio`
`pip3 install pyaudio webrtcvad openai-whisper`

2025-05-25T20:20:26-07:00
Working:
```bash
wentaojiang@Wentaos-Mac-mini ~ % wspr -q
Hello, how many days are there in a week?
wentaojiang@Wentaos-Mac-mini ~ % wspr -q | chat
There are 7 days in a week. The 7 days are: Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, and Sunday.
wentaojiang@Wentaos-Mac-mini ~ % 
```

Holy hell how did this only take 20 min.

ok now going to continue with [[20250518 RAG on COMSOL]].

# STT + RAG

2025-05-25T20:43:04-07:00

2025-05-26T00:24:26-07:00
Back from dinner a while ago.
Still working on it.

`pip install sentence-transformers`

2025-05-26T12:23:07-07:00
1. Process PDFs: python smart_pdf_processor.py
2. Generate embeddings: python smart_embedding_generator.py
3. Test/search: python test_smart_rag.py

![[Pasted image 20250526122333.png]]
Based on the grep results, test_smart_rag.py uses these data files:
## Input Files (from PDF processing):
1. chunks_metadata.json - Metadata for all chunks (source file, pages, chunk IDs, etc.)
2. chunk_texts.npy - Array of chunk texts for embedding
## Generated/Used Files (for embeddings):
1. smart_embeddings.npy - Generated embeddings (if they don't exist, creates them)
2. smart_faiss_index.index - FAISS search index (if it doesn't exist, creates it)
## File Flow:

### First Run (no embeddings exist):
- Reads: chunks_metadata.json, chunk_texts.npy
- Generates: smart_embeddings.npy, smart_faiss_index.index

### Subsequent Runs (embeddings exist):
- Reads: chunks_metadata.json, smart_faiss_index.index
- Skips: Embedding generation (uses existing files)

2025-05-26T12:28:54-07:00
Copying all four data files and will upload to huggingface.
`{chunks_metadata.json,chunk_texts.npy,smart_embeddings.npy,smart_faiss_index.index}`


Working pretty okay:
![[Pasted image 20250526185718.png]]
![[Pasted image 20250526185729.png]]
- this is `python smart_rag_with_llm.py` in `faissgpu` python env.





