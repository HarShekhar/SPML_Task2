## REPORT

**How will relevant information be retrieved efficiently?**
You embedded all 55k+ chunks using MiniLM-L6-v2 (384-dim vectors) and stored them in a FAISS flat index. At query time, the query gets embedded and FAISS does L2 similarity search in milliseconds across all vectors. No database server needed, fully in-memory.

**How will answers remain grounded in evidence?**
The prompt explicitly instructs the model: "Answer using ONLY the evidence provided below. Do not use outside knowledge." The retrieved chunks are injected directly into the prompt, so the model has no reason to go beyond them.

**How will hallucinations be prevented?**
Two ways — first, the grounded prompt restricts the model to retrieved context only. Second, citations are generated inline so the evaluator (or user) can verify each claim against the source chunks shown in the UI.

**How will citations be generated?**
Each chunk carries metadata (source, focus_area) from ingestion time. The model is prompted to cite using [Source: name] inline. The UI also independently shows all retrieved chunks with their source and topic, so citations are verifiable even if the model's inline citations are imperfect.

**How will confidence be estimated?**
Based on retrieval distance scores — lower L2 distance between query embedding and chunk embeddings means higher semantic similarity, which maps to higher confidence. It normalize distances to a 0-1 score and bucket into High/Medium/Low. It also show unique source count as a trust signal — more independent sources agreeing = more trustworthy.

**How will uncertainty be communicated?**
The confidence label and score are shown separately in the UI alongside the answer. The prompt also instructs the model to say "I'm not confident" or "evidence is insufficient" when retrieved chunks are weak — the model handles this in natural language.

**How will unsafe requests be handled?**
Keyword-based safety layer runs before retrieval — so the system never even queries the knowledge base for emergency or harmful requests. Emergency queries get redirected to call 911/112. Harmful requests get a flat refusal. Safety check is the first thing that runs in query_assistant().

**How will vague queries be processed?**
FAISS retrieval handles this naturally — even vague queries produce an embedding and retrieve the closest chunks semantically. The model then generates the best answer it can from those chunks and flags uncertainty if the evidence is weak (reflected in a lower confidence score).

**How will misspelled queries be handled?**
Sentence transformers are somewhat robust to typos since they're trained on noisy text — a misspelled query still produces a meaningful embedding close to the correct one.

**How will retrieval quality be evaluated?**
You can evaluate it by checking whether retrieved chunks are topically relevant to the query. A more rigorous approach would use RAGAS — a RAG evaluation framework that scores context relevance, answer faithfulness, and answer relevance automatically.