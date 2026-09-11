# How RAG Works

## 📖 Introduction

**Retrieval-Augmented Generation (RAG)** works by combining two major capabilities:

1. **Retrieval** — finding relevant information from an external knowledge source.
2. **Generation** — using a Large Language Model (LLM) to generate an answer using the retrieved information.

The basic idea is:

```text
User Question
      ↓
Retrieve Relevant Information
      ↓
Add Retrieved Information as Context
      ↓
LLM Generates Answer
```

RAG therefore allows an LLM application to use information from an external knowledge base while generating a response.

---

# 🔄 RAG Process at a Glance

A typical RAG system works in two main phases:

```text
                RAG SYSTEM
                    │
        ┌───────────┴───────────┐
        │                       │
        ▼                       ▼
Knowledge Preparation       Query Processing
        │                       │
        ▼                       ▼
   Documents                User Query
        │                       │
        ▼                       ▼
     Chunking              Query Embedding
        │                       │
        ▼                       ▼
    Embeddings              Retrieval
        │                       │
        ▼                       ▼
  Vector Database        Relevant Chunks
                                │
                                ▼
                         Context + Query
                                │
                                ▼
                               LLM
                                │
                                ▼
                         Generated Answer
```

---

# 🏗️ Two Main Stages of RAG

## Stage 1: Knowledge Preparation

Before users ask questions, the external knowledge needs to be prepared for retrieval.

```text
Documents
    ↓
Document Loading
    ↓
Text Extraction
    ↓
Text Cleaning
    ↓
Text Chunking
    ↓
Embedding Generation
    ↓
Vector Database
```

This stage is sometimes called the **indexing** or **knowledge ingestion** stage.

---

## Stage 2: Query and Answer Generation

When a user asks a question, the system retrieves relevant information and provides it to the LLM.

```text
User Query
    ↓
Query Processing
    ↓
Query Embedding
    ↓
Similarity Search
    ↓
Relevant Chunks
    ↓
Context Construction
    ↓
LLM
    ↓
Generated Answer
```

---

# 📚 Step 1: Collect Knowledge Sources

The first step is to identify the external information that the RAG system should use.

Possible knowledge sources include:

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

For example:

```text
Knowledge Base
│
├── Employee Handbook
├── Leave Policy
├── Work From Home Policy
├── Benefits Policy
└── Company Guidelines
```

These documents become the source of information for the RAG system.

---

# 📥 Step 2: Document Loading

The documents need to be loaded into the RAG pipeline.

Different document formats may require different loaders.

For example:

```text
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
```

The goal is to convert different sources into a form that can be processed by the system.

---

# 🧹 Step 3: Text Extraction and Cleaning

Documents may contain unnecessary content such as:

* Headers
* Footers
* Repeated text
* Formatting artifacts
* Empty spaces
* Unwanted characters

The extracted text may therefore need preprocessing.

```text
Raw Document
      ↓
Text Extraction
      ↓
Cleaning
      ↓
Usable Text
```

Good preprocessing can improve the quality of later retrieval.

---

# ✂️ Step 4: Text Chunking

Large documents are divided into smaller sections called **chunks**.

For example:

```text
Large Document
      │
      ├── Chunk 1
      │
      ├── Chunk 2
      │
      ├── Chunk 3
      │
      ├── Chunk 4
      │
      └── Chunk 5
```

### Why is chunking needed?

Sending an entire large document to an LLM for every question can be inefficient.

Instead, the system retrieves only the portions that are relevant to the question.

### Example

Suppose a 100-page document contains information about:

```text
Pages 1–20   → Introduction
Pages 21–40  → Leave Policy
Pages 41–60  → Benefits
Pages 61–80  → Work From Home
Pages 81–100 → Security Policy
```

If the user asks:

> What is the company's leave policy?

The system should retrieve the relevant chunks rather than the entire 100-page document.

---

# 🔢 Step 5: Generate Embeddings

After chunking, each chunk is converted into a numerical representation called an **embedding**.

```text
Text Chunk
    ↓
Embedding Model
    ↓
Vector
```

A simplified representation could look like:

```text
"Employees receive annual leave."

        ↓

[0.21, -0.43, 0.76, 0.18, ...]
```

The actual embedding contains many dimensions.

The purpose of embeddings is to represent the semantic meaning of text mathematically.

---

# 🗄️ Step 6: Store Embeddings

The generated embeddings are stored in a **vector database or vector store**.

```text
Document Chunk
      ↓
Embedding
      ↓
Vector Database
```

The vector store also commonly keeps information that allows the original text and metadata to be associated with the vector.

Examples of vector technologies include:

* FAISS
* Chroma
* Pinecone
* Weaviate
* Milvus

---

# ❓ Step 7: User Asks a Question

Now the system is ready to answer questions.

For example:

> How many days of annual leave are employees entitled to?

The question becomes the input to the retrieval stage.

```text
User Question
      ↓
"How many days of annual leave
 are employees entitled to?"
```

---

# 🔢 Step 8: Convert the Query into an Embedding

The user's question is converted into an embedding using an embedding model.

```text
User Query
    ↓
Embedding Model
    ↓
Query Vector
```

The query vector can then be compared with the vectors stored in the vector database.

---

# 🔍 Step 9: Retrieve Relevant Information

The system performs a similarity search between the query embedding and the stored document embeddings.

```text
Query Vector
     │
     ▼
Vector Database
     │
     ├── Chunk 1 → Low similarity
     ├── Chunk 2 → High similarity
     ├── Chunk 3 → Medium similarity
     ├── Chunk 4 → Low similarity
     └── Chunk 5 → High similarity
```

The most relevant chunks are selected.

For example:

```text
Top-K Results

1. Leave Policy — Chunk 12
2. Employee Handbook — Chunk 35
3. Benefits Policy — Chunk 8
```

The value of **K** determines how many results are retrieved.

---

# 📦 Step 10: Build the Context

The retrieved chunks are combined with the user's question.

```text
User Question
      +
Retrieved Information
      ↓
Context
```

For example:

```text
Question:
How many days of annual leave are employees entitled to?

Retrieved Context:
Employees are entitled to 20 days of annual leave
per calendar year according to the company leave policy.
```

This information is then provided to the LLM.

---

# 🧠 Step 11: Send Context to the LLM

The LLM receives:

```text
User Question
      +
Retrieved Context
      ↓
      LLM
```

A simplified prompt might conceptually look like:

```text
Use the following context to answer the question.

Context:
Employees are entitled to 20 days of annual leave
per calendar year.

Question:
How many days of annual leave are employees entitled to?
```

The LLM then generates the answer.

---

# ✍️ Step 12: Generate the Final Answer

The LLM uses the retrieved context to generate a natural-language response.

```text
Retrieved Context
       +
User Question
       ↓
      LLM
       ↓
Generated Answer
```

Example:

> Employees are entitled to 20 days of annual leave per calendar year.

If the system is designed to provide citations, it may also show:

```text
Source:
Employee Leave Policy
Page 5
```

---

# 🔁 Complete RAG Workflow

Putting everything together:

```text
                  KNOWLEDGE PREPARATION
                          
Documents
    ↓
Document Loading
    ↓
Text Extraction
    ↓
Text Cleaning
    ↓
Text Chunking
    ↓
Embedding Model
    ↓
Vector Database
    │
    │
    │
    │
    ▼
────────────────────────────────────────────
                  QUERY TIME
────────────────────────────────────────────
    ▲
    │
User Query
    ↓
Query Embedding
    ↓
Similarity Search
    ↓
Relevant Chunks
    ↓
Context Construction
    ↓
Query + Context
    ↓
LLM
    ↓
Generated Answer
```

---

# 🔍 Retrieval vs Generation

It is important to understand that retrieval and generation are different processes.

## Retrieval

The retrieval component answers:

> **"Which information is relevant to this question?"**

```text
Query
 ↓
Search Knowledge Base
 ↓
Relevant Information
```

## Generation

The generation component answers:

> **"How should I formulate the answer using this information?"**

```text
Question + Context
        ↓
       LLM
        ↓
Generated Answer
```

Together:

```text
Retrieval
    +
Generation
    ↓
    RAG
```

---

# 🧩 Why Each Step Matters

| Step                 | Purpose                                     |
| -------------------- | ------------------------------------------- |
| Document Loading     | Bring external data into the system         |
| Text Extraction      | Obtain usable text                          |
| Cleaning             | Remove unnecessary content                  |
| Chunking             | Divide large content into manageable pieces |
| Embeddings           | Represent semantic meaning numerically      |
| Vector Storage       | Store and search document representations   |
| Query Embedding      | Represent the user's question               |
| Retrieval            | Find relevant information                   |
| Context Construction | Prepare information for the LLM             |
| Generation           | Produce the final response                  |

---

# 📌 Example: University RAG System

Imagine a university has:

```text
University Knowledge Base
│
├── Academic Regulations.pdf
├── Examination Rules.pdf
├── Attendance Policy.pdf
├── Course Handbook.pdf
└── Student Guidelines.pdf
```

A student asks:

> What is the minimum attendance requirement?

The RAG system performs:

```text
Student Question
       ↓
Query Embedding
       ↓
Search Vector Database
       ↓
Retrieve Attendance Policy
       ↓
Relevant Context
       ↓
LLM
       ↓
Answer
```

The system does not need to provide every university document to the LLM.

It retrieves the relevant information first.

---

# ⚡ RAG Is Not Just a Search Engine

RAG is more than simply searching for documents.

A search system might return:

```text
Relevant Document
```

A RAG system goes further:

```text
User Query
     ↓
Retrieve Information
     ↓
Understand Context
     ↓
Generate Natural-Language Response
```

Therefore, RAG combines:

```text
Information Retrieval
          +
Language Understanding
          +
Language Generation
```

---

# 🔐 Where Does the Knowledge Come From?

The external knowledge used by RAG can come from different sources.

```text
              External Knowledge
                     │
       ┌─────────────┼─────────────┐
       ↓             ↓             ↓
   Documents      Databases     Websites
       │             │             │
       └─────────────┼─────────────┘
                     ↓
                RAG System
                     ↓
                    LLM
```

The knowledge source depends on the requirements of the application.

---

# 🎯 Key Factors Affecting RAG Performance

The quality of a RAG system depends on multiple stages.

```text
Source Data Quality
        ↓
Chunking Quality
        ↓
Embedding Quality
        ↓
Retrieval Quality
        ↓
Context Quality
        ↓
Prompt Quality
        ↓
LLM Quality
        ↓
Final Answer Quality
```

A powerful LLM alone does not guarantee a good RAG system.

---

# ⚠️ What Happens When Retrieval Fails?

Consider:

```text
User Question
      ↓
Poor Retrieval
      ↓
Irrelevant Context
      ↓
LLM
      ↓
Poor Answer
```

This is why retrieval is one of the most important components of RAG.

A useful principle is:

> **Better retrieval generally provides better context for generation.**

However, retrieval quality is only one part of the overall system.

---

# 🧠 Simple Mental Model

You can remember RAG using three questions:

### 1. What information do I need?

**Retrieval**

### 2. What information should I give the model?

**Augmentation**

### 3. How should the answer be written?

**Generation**

```text
What information?
       ↓
   RETRIEVAL

Give it to the model
       ↓
  AUGMENTATION

Generate the response
       ↓
   GENERATION
```

---

# ⭐ Key Takeaways

* RAG combines **retrieval** and **generation**.
* External documents are prepared before users ask questions.
* Documents are usually processed and divided into chunks.
* Chunks are converted into embeddings.
* Embeddings are stored in a vector database or vector store.
* A user's question is also converted into an embedding.
* Similarity search retrieves relevant chunks.
* Retrieved information is added to the prompt as context.
* The LLM generates the final response using the question and retrieved context.
* Retrieval quality has a major impact on the final answer.
* RAG can connect LLMs with external, private, domain-specific, or changing information.

---

# 🔑 RAG in One Diagram

```text
                 DOCUMENTS
                     ↓
                PROCESSING
                     ↓
                  CHUNKS
                     ↓
                EMBEDDINGS
                     ↓
              VECTOR DATABASE
                     │
                     │
                     ▼
USER QUERY → RETRIEVAL → RELEVANT CONTEXT
                              │
                              ▼
                       CONTEXT + QUERY
                              │
                              ▼
                             LLM
                              │
                              ▼
                         FINAL ANSWER
```

---

## ➡️ Next Topic

Now that we understand **how RAG works**, the next step is to understand the structure of a RAG system in more detail.

Continue to:

**[04 - RAG Architecture](../04-RAG-Architecture/)**

---

⭐ **In simple terms: RAG retrieves relevant information from an external knowledge source, adds it to the user's query as context, and uses an LLM to generate a response based on that context.**
