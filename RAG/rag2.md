## Building a perfect rag in 2025

**Chunking:** Honestly, the fancy semantic chunking approaches are overhyped. Most of the time, simple sliding window (500-1000 tokens, 100-200 overlap) beats complex hierarchical stuff. The key is preserving context boundaries - don't split mid-sentence or mid-paragraph if you can help it.

**Embedding:** text-embedding-3-large is still king for most use cases. It's pricey but worth it. For budget builds, all-MiniLM-L6-v2 punches way above its weight. BGE models are solid too if you need something in between.

**Retrieval:** Depends on your scale tbh. Pinecone if you want zero ops overhead, Weaviate if you need more control, or just pgvector if you're already on Postgres (seriously underrated). For hybrid search, combining dense + sparse (BM25) retrieval usually beats pure vector search.

**Reranking:** This is where you get the biggest bang for your buck. Cohere's rerank models are fantastic - they'll often fix mediocre retrieval. Cross-encoder models work well too but they're slower.

**Orchestration:** LlamaIndex and LangChain are the obvious choices, but they come with a lot of bloat. Sometimes a simple custom pipeline is cleaner.
