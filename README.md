# Code Report

## Overview

The code sets up an end-to-end pipeline that uses **Llama-Index** (previously GPT Index) for document indexing and retrieval, **ChromaDB** as the vector store, and **Ollama's LLM** for natural language queries. The system is designed to handle retrieval-augmented generation (RAG) tasks, where a language model retrieves information from a database or index and generates relevant responses.

---

## Installing Required Libraries

- **llama-index**: For creating document indexes and querying them.
- **huggingface**: For using Hugging Face’s pre-trained embeddings.
- **chromadb**: A vector database for storing document embeddings.
- **ollama**: For interacting with the Ollama LLM.

---

## Loading Documents

The `SimpleDirectoryReader` class is used to load the documents from the file system. This function converts the document into a list of strings for embedding and indexing.

---

## Embedding Setup

The `HuggingFaceEmbedding` class from `llama_index.embeddings` is used to obtain embeddings (vector representations) for each document chunk. These embeddings help in indexing and similarity-based retrieval tasks.

---

## Setting up ChromaDB

ChromaDB is initialized as a vector database for storing document embeddings and metadata.

---

## Saving Embeddings to Disk

The document embeddings are saved to disk using Chroma's `PersistentClient` via `ChromaVectorStore`. The `StorageContext` class creates a context object that includes the storage mechanisms for the embeddings.

---

## Creating an Index

The document embeddings are indexed using `VectorStoreIndex`. This is the primary index for querying, enabling efficient retrieval from the Chroma vector store.

---

## Loading Index from Disk

The index can be reloaded from disk by retrieving the persisted embeddings and document metadata from ChromaDB. This allows for future queries without re-indexing.

---

## Starting Ollama Server

In the Linux shell, run the following commands to start the Ollama server:

```bash
curl https://ollama.ai/install.sh | sh
ollama serve &
ollama run llama3.1
```

---

## Setting up Ollama LLM

Ollama is the language model used to process queries. We use the **llama3.1** model, and a request timeout of 420 seconds is set.

---

## Key Functionalities of Each Function

- **SimpleDirectoryReader:** Loads documents from a specified path for indexing.
- **HuggingFaceEmbedding:** Generates vector embeddings for each document chunk using a Hugging Face model.
- **ChromaVectorStore:** Manages the vector embeddings and stores them persistently in ChromaDB.
- **VectorStoreIndex:** Creates an index from the document embeddings, enabling similarity-based retrieval.
- **Ollama:** Interfaces with Ollama's LLM to process queries and generate responses based on the document index.
- **query_engine.query():** Executes a query by passing it through the LLM and retrieves results based on the indexed data.

---

# RAG Pipeline

```mermaid
graph TD;
    A[Data Ingestion] --> B[Embedding];
    B --> C[Vector Store];
    C --> D[Indexing];
    D --> E[Querying & Retrieval];
    E --> F[Deployment];
    
    A -->|Load data using SimpleDirectoryReader| B;
    B -->|Convert data to vectors with embeddings| C;
    C -->|Store embeddings using ChromaVectorStore| D;
    D -->|Create index with VectorStoreIndex| E;
    E -->|Query index and generate responses using Ollama LLM| F;
    F -->|Deploy on server and use API| A;
