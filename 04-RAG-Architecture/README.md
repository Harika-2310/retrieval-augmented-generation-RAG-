# RAG Architecture

## 📖 Introduction

**Retrieval-Augmented Generation (RAG) architecture** describes how different components work together to retrieve relevant information from an external knowledge source and use it to generate a response with a Large Language Model (LLM).

A RAG system can be viewed as a pipeline connecting:

```text id="f4r8xa"
External Knowledge
       ↓
Data Processing
       ↓
Knowledge Storage
       ↓
Retrieval
       ↓
Context
       ↓
Large Language Model
       ↓
Generated Response
```

The architecture can vary depending on the application, but the fundamental idea remains the same:

> **Retrieve relevant information → provide it as context → generate a response.**

---

# 🏗️ High-Level RAG Architecture

A typical RAG architecture can be divided into two major parts:

1. **Knowledge Ingestion / Indexing**
2. **Query / Retrieval and Generation**

```text id="x3g7tq"
                    RAG ARCHITECTURE
                          
        ┌─────────────────────────────────┐
        │       KNOWLEDGE INGESTION       │
        └─────────────────────────────────┘
                       │
                       ▼
                  Data Sources
                       │
                       ▼
                  Data Loading
                       │
                       ▼
                 Preprocessing
                       │
                       ▼
                    Chunking
                       │
                       ▼
                  Embeddings
                       │
                       ▼
                Vector Storage
                       │
                       │
                       ▼
        ┌─────────────────────────────────┐
        │          QUERY TIME             │
        └─────────────────────────────────┘
                       ▲
                       │
                  User Query
                       │
                       ▼
                Query Processing
                       │
                       ▼
                   Retrieval
                       │
                       ▼
              Relevant Documents
                       │
                       ▼
                Context Building
                       │
                       ▼
                 Prompt + Context
                       │
                       ▼
                      LLM
                       │
                       ▼
                Generated Answer
```

---

# 🔹 Part 1: Knowledge Ingestion

The first part of the architecture prepares external information so that it can later be searched.

```text id="8g3p4d"
Data Sources
     ↓
Data Loading
     ↓
Preprocessing
     ↓
Chunking
     ↓
Embedding
     ↓
Vector Storage
```

This process is often called **indexing** or **knowledge ingestion**.

---

# 📚 1. Data Sources

The RAG system starts with one or more sources of knowledge.

Examples include:

* PDF documents
* Word documents
* Text files
* Websites
* Databases
* APIs
* Research papers
* Product documentation
* Company documents
* Knowledge bases

Architecture:

```text id="j8d5nf"
              DATA SOURCES
                   │
       ┌───────────┼───────────┐
       ↓           ↓           ↓
   Documents   Databases    Websites
       │           │           │
       └───────────┼───────────┘
                   ↓
              RAG System
```

The choice of data source depends on the application.

---

# 📥 2. Data Loading

Data needs to be brought into the RAG pipeline.

Different types of data may require different loading methods.

```text id="e5w0sl"
PDF
 ↓
PDF Loader

DOCX
 ↓
Document Loader

TXT
 ↓
Text Loader

Web Page
 ↓
Web Loader

Database
 ↓
Database Connector
```

The goal is to convert different data sources into a format that can be processed consistently.

---

# 🧹 3. Preprocessing

Raw data may contain unnecessary or inconsistent information.

Preprocessing can include:

* Text extraction
* Cleaning
* Removing unnecessary characters
* Removing duplicate content
* Formatting
* Metadata extraction
* Normalization

```text id="l1l4ak"
Raw Data
   ↓
Text Extraction
   ↓
Cleaning
   ↓
Structured Content
```

Good preprocessing can improve downstream retrieval.

---

# ✂️ 4. Chunking

Large documents are divided into smaller pieces called **chunks**.

```text id="a8v3a2"
Large Document
       │
       ├── Chunk 1
       ├── Chunk 2
       ├── Chunk 3
       ├── Chunk 4
       └── Chunk 5
```

### Why is chunking important?

If documents are too large, retrieving an entire document for every query can be inefficient.

Chunking allows the system to retrieve smaller, relevant portions.

For example:

```text id="4j34d8"
100-Page Document
       ↓
Relevant Section
       ↓
Relevant Chunk
       ↓
LLM
```

Chunking strategy has a direct impact on retrieval quality.

---

# 🔢 5. Embedding Model

Each chunk is converted into a numerical vector using an **embedding model**.

```text id="h4a5j7"
Text Chunk
    ↓
Embedding Model
    ↓
Vector Representation
```

Conceptually:

```text id="6f7k2m"
"Machine learning is a branch of AI."

              ↓

[0.12, -0.45, 0.71, 0.23, ...]
```

Embeddings represent semantic information in numerical form.

This allows the system to compare the meaning of queries and documents.

---

# 🗄️ 6. Vector Store / Vector Database

The generated embeddings are stored in a vector store or vector database.

```text id="8o6p8e"
Document Chunk
      ↓
Embedding
      ↓
Vector Store
```

A vector store allows the system to efficiently search for vectors that are similar to a query vector.

Examples include:

* FAISS
* Chroma
* Pinecone
* Weaviate
* Milvus

The vector store may also contain metadata and references to the original documents.

---

# 🔹 Part 2: Query and Retrieval

After the knowledge base has been prepared, the system can process user questions.

```text id="c2y4k8"
User Query
    ↓
Query Processing
    ↓
Query Embedding
    ↓
Retrieval
    ↓
Relevant Chunks
```

---

# ❓ 7. User Query

The process begins when a user asks a question.

Example:

```text id="y8d5l1"
"What is Retrieval-Augmented Generation?"
```

The question becomes the input to the retrieval pipeline.

---

# 🔢 8. Query Embedding

The user query is converted into an embedding using an embedding model.

```text id="0l2m5v"
User Query
    ↓
Embedding Model
    ↓
Query Vector
```

The query vector is then compared with vectors stored in the vector database.

---

# 🔍 9. Retriever

The **retriever** searches the knowledge base for information relevant to the user's query.

```text id="q5j7l3"
Query Vector
     ↓
Vector Store
     ↓
Similarity Search
     ↓
Relevant Chunks
```

For example:

```text id="u7c2x5"
Query
 ↓
Search
 ↓
Top-K Relevant Chunks
```

The retriever may use techniques such as:

* Vector similarity search
* Keyword search
* Hybrid search
* Metadata filtering

---

# 📊 10. Similarity Search

Similarity search determines which stored chunks are most relevant to the query.

A simplified process is:

```text id="p8k3s1"
Query Vector
     │
     ├── Chunk A → Similarity: High
     ├── Chunk B → Similarity: Low
     ├── Chunk C → Similarity: High
     ├── Chunk D → Similarity: Medium
     └── Chunk E → Similarity: Low
```

The system selects the most relevant chunks.

Common similarity measures include:

* Cosine similarity
* Dot product
* Euclidean distance

---

# 📦 11. Context Building

The retrieved chunks are assembled into useful context for the LLM.

```text id="v7g9s2"
Retrieved Chunk 1
       +
Retrieved Chunk 2
       +
Retrieved Chunk 3
       ↓
    Context
```

The context is then combined with the user's question.

```text id="z8l3w4"
User Query
     +
Retrieved Context
     ↓
Prompt
```

---

# 🧠 12. Large Language Model

The LLM receives the user question together with the retrieved context.

```text id="q9f2b6"
User Query
     +
Retrieved Context
     ↓
     LLM
     ↓
Generated Response
```

The LLM's role is to:

* Understand the question
* Interpret the retrieved information
* Follow instructions
* Generate a natural-language response

---

# ✍️ 13. Response Generation

The final answer is generated using the retrieved context.

```text id="z4d1n7"
Query
  +
Relevant Context
  ↓
 LLM
  ↓
Final Answer
```

A RAG application may also return information about the sources used.

For example:

```text id="n5r7p2"
Answer:
RAG retrieves relevant information from an
external knowledge source before generating
a response.

Source:
RAG Documentation
```

Source attribution depends on the implementation.

---

# 🧩 Complete RAG Architecture

The complete architecture can be represented as:

```text id="k1f8q3"
                       KNOWLEDGE SOURCES
                              │
              ┌───────────────┼───────────────┐
              ↓               ↓               ↓
          Documents       Databases       Websites
              │               │               │
              └───────────────┼───────────────┘
                              ↓
                       Document Loading
                              ↓
                         Preprocessing
                              ↓
                           Chunking
                              ↓
                         Embeddings
                              ↓
                     Vector Database
                              │
                              │
                              │
                              ▼
                    ┌──────────────────┐
                    │     RETRIEVER    │
                    └────────┬─────────┘
                             ▲
                             │
                        User Query
                             │
                             ▼
                      Query Embedding
                             │
                             ▼
                     Similarity Search
                             │
                             ▼
                    Relevant Information
                             │
                             ▼
                      Context Building
                             │
                             ▼
                     Query + Context
                             │
                             ▼
                            LLM
                             │
                             ▼
                     Generated Answer
```

---

# 🔀 RAG Architecture as Two Pipelines

A useful way to understand the architecture is to divide it into two pipelines.

## Pipeline 1: Indexing Pipeline

```text id="k8z1n4"
Documents
    ↓
Load
    ↓
Process
    ↓
Chunk
    ↓
Embed
    ↓
Store
```

This pipeline prepares the knowledge.

---

## Pipeline 2: Retrieval Pipeline

```text id="s7v5p1"
User Query
    ↓
Embed Query
    ↓
Search
    ↓
Retrieve
    ↓
Build Context
    ↓
LLM
    ↓
Answer
```

This pipeline handles user questions.

---

# 🔄 Indexing vs Query Time

| Indexing / Ingestion Time | Query Time               |
| ------------------------- | ------------------------ |
| Collect documents         | Receive user query       |
| Load documents            | Process query            |
| Extract text              | Generate query embedding |
| Clean text                | Search vector store      |
| Chunk text                | Retrieve relevant chunks |
| Generate embeddings       | Build context            |
| Store vectors             | Send context to LLM      |
| Prepare knowledge base    | Generate response        |

This separation is important because the knowledge preparation process does not necessarily need to happen every time a user asks a question.

---

# 🧱 Core Components of RAG Architecture

The major components can be summarized as:

```text id="r7y2v1"
┌─────────────────────────────┐
│       1. Data Sources       │
└──────────────┬──────────────┘
               ↓
```
