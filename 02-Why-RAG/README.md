# Why RAG?

## 📖 Introduction

Large Language Models (LLMs) have transformed the way we interact with artificial intelligence. They can understand questions, generate text, summarize information, write code, and perform many other language-based tasks.

However, an LLM has limitations when it needs to answer questions using **specific, private, current, or external information**.

**Retrieval-Augmented Generation (RAG)** addresses these challenges by connecting an LLM with an external knowledge source.

The fundamental idea is:

```text
LLM
+
External Knowledge
+
Retrieval
↓
More Context-Aware Responses
```

---

# ❓ Why Do We Need RAG?

RAG is needed because a standalone LLM does not automatically have access to every piece of information required to answer a user's question.

Consider the following situation:

```text
User:
"What is our company's leave policy?"

        ↓

General LLM

        ↓

Does not automatically know
the company's private policy
```

A RAG system can connect the LLM to the company's documents:

```text
Company Documents
       ↓
Knowledge Base
       ↓
RAG Retrieval
       ↓
Relevant Leave Policy
       ↓
LLM
       ↓
Answer
```

---

# ⚠️ Limitations of Standalone LLMs

Understanding the limitations of traditional LLM-based systems helps explain why RAG is useful.

## 1. Knowledge Cutoff

LLMs are trained on datasets collected during a particular period.

Therefore, the model may not know information that became available after its training data was collected.

For example:

```text
Model Training
      ↓
Knowledge Available
      ↓
New Information
      ↓
Model may not know it
```

RAG allows an application to retrieve newer information from an external knowledge source.

```text
New Information
      ↓
External Knowledge Base
      ↓
RAG Retrieval
      ↓
LLM
```

### Key Point

RAG allows the knowledge used by an application to be updated independently of the model's original training data.

---

# 🔐 2. Lack of Access to Private Data

General-purpose LLMs do not automatically have access to an organization's private information.

Examples include:

* Company policies
* Internal documentation
* Employee handbooks
* Product manuals
* Internal reports
* Customer support documents
* Organization-specific procedures

For example:

```text
Company Knowledge
       │
       ├── HR Policies
       ├── Financial Reports
       ├── Product Documents
       └── Internal Procedures
```

A RAG system can be designed to retrieve relevant information from these authorized sources.

```text
Private Documents
       ↓
Knowledge Base
       ↓
Retriever
       ↓
Relevant Information
       ↓
LLM
       ↓
Response
```

---

# 🤥 3. Hallucinations

An LLM can sometimes generate information that appears plausible but is incorrect or unsupported.

This is commonly called an **AI hallucination**.

For example:

```text
User:
"What does the company policy say about remote work?"

LLM:
Generates an answer based on its learned patterns.

Problem:
The answer may not represent the actual company policy.
```

With RAG:

```text
User Question
      ↓
Retrieve Company Policy
      ↓
Relevant Policy Information
      ↓
LLM + Retrieved Context
      ↓
Answer
```

The retrieved information can provide evidence for the response.

### Important

RAG **does not eliminate hallucinations completely**.

It can help ground responses in retrieved information, but the quality of the answer still depends on:

* Source quality
* Retrieval quality
* Chunking
* Embeddings
* Prompt design
* LLM behavior

---

# 🔄 4. Frequently Changing Information

Some information changes frequently.

Examples include:

* Product documentation
* Company policies
* Regulations
* Internal procedures
* Knowledge bases
* Technical documentation

Retraining an entire LLM every time this information changes is generally impractical.

RAG provides an alternative approach:

```text
Updated Information
       ↓
Update Knowledge Base
       ↓
Retrieve New Information
       ↓
LLM
       ↓
Updated Response
```

The external knowledge source can be maintained separately from the model.

---

# 📚 5. Domain-Specific Knowledge

General-purpose LLMs are trained on broad datasets.

However, some applications require specialized knowledge.

Examples:

```text
Healthcare
   ↓
Medical Documents

Legal
   ↓
Legal Documents

Finance
   ↓
Financial Reports

Education
   ↓
Course Materials

Engineering
   ↓
Technical Documentation
```

RAG allows an LLM application to retrieve information from a domain-specific knowledge base.

---

# 🔎 6. Need for Relevant Context

An LLM may have broad knowledge but still need specific information to answer a particular question accurately.

RAG adds a retrieval step:

```text
User Question
      ↓
Search Knowledge Base
      ↓
Find Relevant Information
      ↓
Provide Context to LLM
      ↓
Generate Answer
```

Instead of providing the entire knowledge base to the model, the system can retrieve information that is relevant to the user's query.

---

# 📌 7. Need for Source Attribution

Many applications require users to understand where an answer came from.

Examples include:

* Research
* Legal analysis
* Enterprise knowledge systems
* Technical documentation
* Education

A RAG system can be designed to return source information along with the generated answer.

For example:

```text
Answer:
Employees are eligible for 20 days of annual leave.

Source:
Employee_Handbook.pdf
Page 15
```

This can make the response easier to verify.

> Source attribution depends on how the RAG application is designed; RAG itself does not automatically guarantee citations.

---

# 🧠 8. Keeping Knowledge Separate from the Model

One important idea behind RAG is the separation between:

```text
Model Knowledge
       +
External Knowledge
```

The LLM provides language understanding and generation capabilities.

The external knowledge source provides application-specific information.

```text
                 ┌───────────────┐
                 │      LLM      │
                 │               │
                 │ Understanding │
                 │ + Generation  │
                 └───────┬───────┘
                         │
                         │
                  RAG Application
                         │
                         ▼
                 ┌───────────────┐
                 │External Data  │
                 │               │
                 │ Documents     │
                 │ Databases     │
                 │ Knowledge Base│
                 └───────────────┘
```

This separation makes it possible to update application knowledge without necessarily changing the underlying LLM.

---

# 🆚 Without RAG vs With RAG

## Without RAG

```text
                 User Query
                     ↓
                    LLM
                     ↓
              Generated Answer
```

The model primarily relies on its learned knowledge and the information included in the current conversation or prompt.

---

## With RAG

```text
                 User Query
                     ↓
                Query Processing
                     ↓
                 Retrieval
                     ↓
             Relevant Information
                     ↓
               LLM + Context
                     ↓
              Generated Answer
```

The key difference is the **retrieval step**.

---

# 🎯 Main Reasons to Use RAG

| Problem                     | How RAG Helps                                           |
| --------------------------- | ------------------------------------------------------- |
| Knowledge cutoff            | Retrieves information from external sources             |
| Private data                | Connects applications to authorized knowledge bases     |
| Changing information        | Allows external knowledge to be updated                 |
| Domain-specific information | Retrieves specialized documents                         |
| Context requirements        | Provides relevant information to the LLM                |
| Source attribution          | Can return retrieved sources                            |
| Grounding                   | Provides evidence that can guide the generated response |

---

# 💡 Simple Real-World Example

Imagine a university has thousands of documents:

```text
University Knowledge Base
│
├── Academic Regulations
├── Examination Rules
├── Course Information
├── Attendance Policy
├── Fee Structure
└── Student Handbook
```

A student asks:

> "What is the minimum attendance requirement?"

### Without RAG

```text
Student Question
       ↓
      LLM
       ↓
Possible Answer
```

The answer may not reflect the university's actual policy.

### With RAG

```text
Student Question
       ↓
Search University Documents
       ↓
Find Attendance Policy
       ↓
Retrieve Relevant Section
       ↓
LLM + Retrieved Context
       ↓
Answer Based on Policy
```

This is one of the key reasons RAG is useful.

---

# 🏢 Where is RAG Especially Useful?

RAG can be useful in applications that need access to external or specialized knowledge.

### Enterprise

```text
Company Documents
       ↓
Enterprise RAG
       ↓
Employee Questions
```

### Education

```text
Books + Course Materials
       ↓
Educational RAG
       ↓
Student Questions
```

### Research

```text
Research Papers
       ↓
Research RAG
       ↓
Research Questions
```

### Customer Support

```text
Product Documentation
       ↓
Support RAG
       ↓
Customer Questions
```

### Technical Documentation

```text
Technical Documentation
       ↓
Documentation RAG
       ↓
Developer Questions
```

---

# 🔑 RAG Does Not Replace the LLM

RAG and LLMs have different roles.

```text
External Knowledge
       ↓
    Retrieval
       ↓
Relevant Context
       ↓
      LLM
       ↓
Language Understanding
       +
Reasoning / Generation
       ↓
Final Response
```

The retriever finds useful information.

The LLM uses that information to generate a natural-language response.

---

# 🧩 RAG as a Bridge

RAG can be viewed as a bridge between an LLM and external knowledge.

```text
┌──────────────────┐
│ External         │
│ Knowledge        │
│                  │
│ Documents        │
│ Databases        │
│ Websites         │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│    Retrieval     │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Relevant Context │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│       LLM        │
└────────┬─────────┘
         │
         ▼
      Response
```

---

# ⭐ Key Takeaways

RAG is useful because it allows LLM applications to:

* Access external knowledge
* Work with private and domain-specific information
* Use updated information
* Retrieve relevant context
* Potentially reduce unsupported responses
* Provide source information when implemented
* Keep application knowledge separate from model parameters

However:

> **RAG is not a complete solution to hallucinations or incorrect answers.**

The quality of a RAG system depends heavily on the quality of its retrieval process and knowledge sources.

---

# 📌 In Simple Words

Without RAG:

> **"Ask the model what it knows."**

With RAG:

> **"Find the relevant information first, then ask the model to answer using that information."**

```text
Traditional LLM
     ↓
"What do you know?"

RAG
     ↓
"Find the relevant information
and use it to answer."
```

---

## ➡️ Next Topic

Now that we understand **why RAG is needed**, the next step is to understand:

> **How does RAG actually work?**

Continue to:

**[03 - How RAG Works](../03-How-RAG-Works/)**

---

⭐ **RAG is important because it connects the language capabilities of LLMs with external knowledge, enabling applications to provide more relevant and knowledge-grounded responses.**
