# GenAI Concepts - 13-Notebook Course

This is the **GenAI/RAG track only**. Agentic AI is intentionally separated into a later track.

## Exact sequence

1. Document Loader
2. Prompt Template
3. Output Parser
4. Text Splitter
5. Vector DB + Embeddings
6. Retrieval
7. Basic RAG
8. RAG Using LangChain
9. Advanced RAG
10. RAG Evaluation
11. RAG Improvement
12. Multi-Modal RAG
13. Recommendation RAG

Each numbered notebook is written as **notes + code + evaluation + improvement + practice**.


## Source mapping

The course notebooks are:

- `01_doc-loader.ipynb` → document loading and metadata
- `02_prompt-template.ipynb` → prompt templates
- `03_output-parser.ipynb` → structured output parsing
- `04_text-splitter.ipynb` → chunking strategies
- `05_vector-db.ipynb` → embeddings and vector databases
- `06_retrieval.ipynb` → similarity search and retrieval
- `07_basic-rag.ipynb` → end-to-end basic RAG
- `08_rag-using-langchain.ipynb` → RAG with LangChain
- `09_advanced-rag.ipynb` → advanced retrieval and reranking
- `10_rag-evaluation.ipynb` → RAG evaluation
- `11_rag-improvement.ipynb` → improving RAG quality
- `12_multimodal-rag.ipynb` → multimodal retrieval-augmented generation
- `13_recommendation-rag.ipynb` → recommendation-focused RAG

`00_genai_course_index.ipynb` is an index for the course and is not part of the
13-notebook sequence. Agentic AI is deliberately excluded because it belongs to
a separate track.


## Learning architecture

```text
Raw documents
   ↓
Document Loader
   ↓
Text Splitter
   ↓
Embeddings
   ↓
Vector DB
   ↓
Retrieval
   ↓
Basic RAG
   ↓
LangChain RAG
   ↓
Advanced RAG
   ↓
Evaluation
   ↓
Improvement
   ├── Multimodal RAG
   └── Recommendation RAG
```


## Recommended environment

```bash
pip install -U -r requirements.txt
```

For model-based examples, configure environment variables for the provider/model you actually have access to. The notebooks avoid hard-coding a specific future model name.


## Current evaluation note

Ragas currently documents RAG metrics such as Context Precision, Context Recall, Response Relevancy, Faithfulness, Multimodal Faithfulness, and Multimodal Relevance. Use those alongside deterministic retrieval metrics such as Recall@K and MRR.

References:
- https://docs.ragas.io/en/latest/concepts/metrics/available_metrics/
- https://reference.langchain.com/python/langchain/overview