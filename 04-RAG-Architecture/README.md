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

🧱 Core Components of RAG Architecture

The major components can be summarized as:

┌─────────────────────────────┐
│       1. Data Sources       │
└──────────────┬──────────────┘
               ↓
┌─────────────────────────────┐
│       2. Data Loading       │
└──────────────┬──────────────┘
               ↓
┌─────────────────────────────┐
│      3. Preprocessing       │
└──────────────┬──────────────┘
               ↓
┌─────────────────────────────┐
│        4. Chunking          │
└──────────────┬──────────────┘
               ↓
┌─────────────────────────────┐
│       5. Embeddings         │
└──────────────┬──────────────┘
               ↓
┌─────────────────────────────┐
│    6. Vector Database       │
└──────────────┬──────────────┘
               ↓
┌─────────────────────────────┐
│        7. Retriever         │
└──────────────┬──────────────┘
               ↓
┌─────────────────────────────┐
│      8. Context Builder     │
└──────────────┬──────────────┘
               ↓
┌─────────────────────────────┐
│          9. LLM             │
└──────────────┬──────────────┘
               ↓
┌─────────────────────────────┐
│      10. Final Answer       │
└─────────────────────────────┘
🔗 Role of Each Component
Component	Main Responsibility
Data Sources	Provide external knowledge
Data Loader	Load information into the pipeline
Preprocessor	Clean and prepare data
Chunker	Divide documents into smaller sections
Embedding Model	Convert text into vectors
Vector Database	Store and search vector representations
Retriever	Find relevant information
Context Builder	Prepare retrieved information for the LLM
LLM	Understand context and generate response
Response Layer	Present the generated answer to the user
⚙️ Basic RAG Architecture vs Advanced RAG Architecture
Basic RAG

A basic architecture may look like:

Documents
    ↓
Chunking
    ↓
Embeddings
    ↓
Vector Database
    ↓
Retrieval
    ↓
LLM
    ↓
Answer
Advanced RAG

More sophisticated systems may introduce additional components:

User Query
    ↓
Query Transformation
    ↓
Hybrid Retrieval
    ↓
Filtering
    ↓
Reranking
    ↓
Context Selection
    ↓
Prompt Construction
    ↓
LLM
    ↓
Answer
    ↓
Source Attribution

The architecture depends on the application's requirements.

🔄 Hybrid Retrieval Architecture

Some RAG systems combine different retrieval methods.

                 User Query
                     │
             ┌───────┴───────┐
             ↓               ↓
       Keyword Search    Vector Search
             │               │
             └───────┬───────┘
                     ↓
              Combine Results
                     ↓
                  Reranking
                     ↓
             Relevant Context
                     ↓
                    LLM
Keyword Search

Useful when exact terms matter.

Vector Search

Useful when semantic similarity matters.

Hybrid Search

Combines both approaches.

📈 Reranking in RAG

Initial retrieval may return several potentially relevant chunks.

A reranker can then reorder those results according to their relevance to the query.

User Query
     ↓
Initial Retrieval
     ↓
Candidate Chunks
     ↓
Reranker
     ↓
Best Chunks
     ↓
LLM

Reranking is an optional component used in more advanced RAG architectures.

🧠 Context Window Consideration

The LLM can process only a certain amount of information within its context.

Therefore, a RAG system should avoid sending unnecessary retrieved information.

Knowledge Base
     ↓
Many Documents
     ↓
Retrieve Relevant Chunks
     ↓
Select Useful Context
     ↓
LLM

The goal is to provide relevant context, rather than simply providing as much information as possible.

🎯 Architecture Design Goals

A good RAG architecture generally aims to achieve:

1. High Retrieval Relevance

Retrieve information that is actually useful for the question.

2. Good Context Quality

Provide the LLM with clear and relevant context.

3. Efficient Search

Retrieve information without unnecessary processing.

4. Reliable Responses

Generate answers grounded in useful source information.

5. Scalability

Support increasing amounts of data and users.

6. Maintainability

Allow components such as the knowledge base or retrieval system to be updated independently.

⚠️ Common Architecture Challenges

RAG architecture also introduces several challenges.

Poor Chunking

Incorrect chunk sizes can cause important information to be split or irrelevant information to be combined.

Poor Retrieval

If the retriever selects irrelevant chunks, the LLM receives poor context.

Duplicate Information

Multiple retrieved chunks may contain overlapping information.

Too Much Context

Retrieving too many chunks can introduce noise.

Poor Source Data

Incorrect or outdated documents can lead to incorrect answers.

Latency

Each additional processing stage can increase response time.

🌍 Example Architecture: Company Knowledge Assistant

Consider an organization with:

Company Knowledge
│
├── HR Policies
├── Technical Documentation
├── Product Manuals
├── Employee Handbook
└── Internal Guidelines

The architecture could be:

Company Documents
       ↓
Document Processing
       ↓
Chunking
       ↓
Embeddings
       ↓
Vector Database
       │
       │
       ▼
Employee Question
       ↓
Query Embedding
       ↓
Retriever
       ↓
Relevant Company Information
       ↓
Context
       ↓
LLM
       ↓
Employee Answer

This demonstrates how a RAG architecture connects organizational knowledge with an LLM.

🆚 RAG Architecture vs Traditional LLM Architecture
Traditional LLM
User
 ↓
LLM
 ↓
Response
RAG
                External Knowledge
                       ↓
                 Retrieval System
                       ↓
User → Query → Relevant Context
                       ↓
                      LLM
                       ↓
                    Response

The major architectural difference is the addition of an external knowledge and retrieval layer.

🔑 Key Takeaways
RAG architecture connects an LLM with external knowledge.
It generally contains an ingestion/indexing pipeline and a query/retrieval pipeline.
Documents are loaded, processed, chunked, embedded, and stored.
A user's query is converted into a representation suitable for retrieval.
The retriever finds relevant information.
Retrieved information is used as context for the LLM.
The LLM generates the final response.
Advanced architectures may include hybrid search, filtering, reranking, and query transformation.
Good architecture depends on retrieval quality, data quality, context quality, and system requirements.
🧠 RAG Architecture in One Line
Knowledge → Index → Retrieve → Context → LLM → Answer

Or simply:

Store knowledge → retrieve relevant information → give it to the LLM → generate an answer.

