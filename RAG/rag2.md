## Building a perfect rag in 2025

**Chunking:** Chonkie, Honestly, the fancy semantic chunking approaches are overhyped. Most of the time, simple sliding window (500-1000 tokens, 100-200 overlap) beats complex hierarchical stuff. The key is preserving context boundaries - don't split mid-sentence or mid-paragraph if you can help it.

**Embedding:** text-embedding-3-large is still king for most use cases. It's pricey but worth it. For budget builds, all-MiniLM-L6-v2 punches way above its weight. BGE models are solid too if you need something in between.

**Retrieval:** Depends on your scale tbh. Pinecone if you want zero ops overhead, Weaviate if you need more control, or just pgvector if you're already on Postgres (seriously underrated). For hybrid search, combining dense + sparse (BM25) retrieval usually beats pure vector search.
Any vector database with an agentic retrieval layer (spinoff multiple queries, evaluate them, do additional retrievals based on the context, etc.). Tried GraphRAG but was too slow/expensive.
Agentic RAG uses something similar to hybrid search underneath. It has access to semantic and keyword search tools, and uses tool use in an agentic way to get relevant results. I found it *much* better than hybrid search because you're not at the mercy of a combined ranking score. For some queries you want to index more on the keyword piece and vice versa.

The agentic layer is outside of the vector database, the vector db just finds the most relevant chunks. The agentic layer does the querying, evaluation, and modifies the queries if it makes sense.


**Reranking:** This is where you get the biggest bang for your buck. Cohere's rerank models are fantastic - they'll often fix mediocre retrieval. Cross-encoder models work well too but they're slower.

**Orchestration:** LlamaIndex and LangChain are the obvious choices, but they come with a lot of bloat. Sometimes a simple custom pipeline is cleaner.


**HyDe**
HyDE Query
Fine Tuned LLM ( Optional) , built using continued Pretraining : https://docs.unsloth.ai/basics/continued-pretraining
User query converted into HyDE Query, then sent to FineTuned LLM, then the output is sent to Vector DB (PostgreSQL) for cosine similarity -> Reranker -> LLM model (Deepseek,Geminie etc)
