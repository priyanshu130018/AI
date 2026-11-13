# RAG

This folder contains practical Retrieval-Augmented Generation notebooks and
supporting examples.

## Notebooks

- `chunking.ipynb` - chunking strategies
- `embeddings.ipynb` - embeddings and similarity
- `faiss.ipynb` - FAISS indexing and search
- `chroma_db.ipynb` - Chroma persistence, metadata, filtering, and reranking
- `searching.ipynb` - L2, cosine, and Euclidean retrieval
- `01_basic-rag.ipynb` - basic RAG
- `02_rag-using-langchain.ipynb` - RAG with LangChain
- `03_advanced-rag.ipynb` - advanced RAG techniques
- `simple_rag/rag.ipynb` - a small end-to-end RAG example
- `RAG_All_Components_Workflow_Revision.ipynb` - a consolidated RAG workflow

## Typical pipeline

```text
Documents -> Chunking -> Embeddings -> Vector Store -> Retrieval -> Answer
```

Install the shared dependencies from the repository root before running the
notebooks:

```powershell
pip install -r requirements.txt
```

Generated vector indexes, Chroma stores, databases, and local environment files
are excluded by the repository `.gitignore`.