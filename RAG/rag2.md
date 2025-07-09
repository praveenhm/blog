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


####Agentic RAG
"Agentic retrieval layer": This is the "research assistant" part. It's a layer of logic, powered by a Large Language Model (LLM), that sits before the final answer generation. Its only job is to be smart about retrieving information.

"Spinoff multiple queries": Just like our assistant, the agent breaks down a complex user query ("Compare A and B") into multiple, simpler sub-queries ("Find A", "Find B", "Find context C").

"Evaluate them, do additional retrievals": The agent looks at the results of its first search. If the information is poor or incomplete, it can decide to try again with a different query. For example, if "Tesla Model S battery" returned nothing, it might try "Tesla vehicle battery specifications" instead.

"Access to semantic and keyword search tools": This is the agent's toolkit.
Semantic Search (Vector Search): Finds documents based on meaning and context. Great for conceptual questions. ("What are the philosophical implications of AI?")
Keyword Search (like BM25): Finds documents based on exact word matches. Crucial for specific names, acronyms, or codes. ("Find documents mentioning 'Project X-734'.")

"Not at the mercy of a combined ranking score": Standard hybrid search runs both a vector search and a keyword search simultaneously and then uses a formula (e.g., 60% vector score + 40% keyword score) to create a final ranked list. This formula is static. Agentic RAG is smarter. The agent decides which tool is best. For a query with a product code, it might choose to rely 100% on the keyword search tool. For a conceptual query, it might rely 100% on the semantic search tool. It has flexibility.
