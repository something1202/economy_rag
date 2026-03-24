# Ask The Economist — RAG Pipeline

A Retrieval-Augmented Generation (RAG) system that answers questions grounded in articles from The Economist. It combines a knowledge graph with vector-based chunk retrieval, reranking, and streamed LLM generation to produce referenced, Economist-style responses.

**Article time scope:** 2025-10-01 ~ 2025-12-31

> **Note:** This repository is NOT the submission report. The submission report is a PDF file sent via email.

## Demo

A live demo is available at: https://cuneatic-unflurried-lily.ngrok-free.dev/

The user can input a query into the search box. Audio input is also supported — click the microphone icon to record your question.

![Audio input](Fig%201.%20Can%20use%20audio%20input.png)
*Fig 1. Audio input via the microphone button.*

The system will retrieve relevant entities and articles, then stream a referenced answer with a knowledge graph visualization:

![Use case](Fig%202.%20Usecase.png)
*Fig 2. Example response with inline references and knowledge graph.*

Once a response is generated, users can click the **Read Aloud** button to have the answer read back using text-to-speech (TTS). This is powered by Open WebUI's built-in TTS integration, making the system fully accessible for hands-free use.

![Read Aloud](Fig%203.%20TTS%20integration%20for%20Read%20Aloud.png)
*Fig 3. TTS integration — the "Read Aloud" button converts the response to speech.*

## Architecture

The pipeline executes five steps for each query:

1. **Query Reformulation** — Resolves conversational references so the query stands alone.
2. **Knowledge Graph Retrieval** — Queries LightRAG entity/relationship vector stores and expands via Neo4j n-hop traversal.
3. **KG Reranking** — Filters entities and relationships by relevance using an LLM-based reranker.
4. **Chunk Retrieval & Reranking** — Retrieves article chunks from a vector store, reranks them, and applies a recency boost.
5. **Answer Generation** — Streams a response with inline citations, adapting tone to the dominant Economist section (e.g. Leaders, Finance & Economics, Culture).

## Tech Stack

- **Framework:** FastAPI + Uvicorn
- **RAG Engine:** LightRAG (graph-based RAG)
- **Graph Database:** Neo4j
- **LLM / Embeddings:** OpenRouter (OpenAI-compatible API)
- **Reranking:** LLM-based reranker with KG-entity and document-chunk modes
- **Frontend:** Open WebUI (connected via OpenAI-compatible `/v1/chat/completions` proxy)

## License

[MIT](LICENSE)
