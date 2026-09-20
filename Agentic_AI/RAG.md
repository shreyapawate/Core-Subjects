Yes — this video is **“What is RAG? | Completely Explained in 15 Minutes” by Apna College**. ([Telegram][1])

Since your **BMC Helix interview is tomorrow**, I’ll turn it into **technical-interview notes**, with extra emphasis on how RAG connects to **Agentic AI, LLMs, embeddings, vector databases, and enterprise applications**.

# RAG — Interview Notes

## 1. What is RAG?

**RAG = Retrieval-Augmented Generation**

RAG is a technique where we **retrieve relevant information from an external knowledge source and provide it to the LLM as context before generating the answer.**

### Simple definition to say in interview

> **“RAG is a technique that combines information retrieval with LLM generation. Instead of relying only on the knowledge stored in the model's parameters, we retrieve relevant information from an external knowledge base and provide it as context to the LLM to generate a more accurate and grounded response.”**

---

# 2. Why do we need RAG?

An LLM has a major limitation:

### LLM's knowledge is not automatically your company's knowledge.

Suppose a company has:

```text
company_policy.pdf
employee_handbook.pdf
leave_policy.pdf
insurance_policy.pdf
HR_guidelines.pdf
```

You ask:

> "How many casual leaves can an employee take?"

A general LLM may not know the company's specific policy.

So we use RAG:

```text
Company Documents
       ↓
Knowledge Base
       ↓
Retrieve relevant information
       ↓
LLM
       ↓
Answer
```

---

# 3. LLM vs RAG

### Without RAG

```text
User Question
      ↓
     LLM
      ↓
Answer
```

The LLM relies mainly on its learned knowledge.

### With RAG

```text
User Question
      ↓
Retriever
      ↓
Relevant Documents
      ↓
LLM + Retrieved Context
      ↓
Answer
```

This allows the system to answer using **external/private/current knowledge**.

---

# 4. Complete RAG architecture

This is the most important diagram to remember:

```text
                 USER
                   ↓
              QUESTION
                   ↓
              RETRIEVER
                   ↓
        ┌─────────────────────┐
        │   KNOWLEDGE BASE    │
        │                     │
        │ Documents           │
        │ Vector Database     │
        └─────────────────────┘
                   ↓
           Relevant Chunks
                   ↓
          ┌─────────────────┐
          │      LLM        │
          │                 │
          │ Question        │
          │      +          │
          │ Context         │
          └─────────────────┘
                   ↓
               ANSWER
```

---

# 5. How RAG works

RAG has **two major phases**:

### Phase 1 — Indexing

Prepare the knowledge base.

### Phase 2 — Retrieval + Generation

Answer the user's question.

---

# 6. Phase 1 — Indexing

Suppose we have:

```text
company_policy.pdf
```

### Step 1 — Load documents

```text
PDF
DOCX
TXT
Web pages
Database
```

↓

### Step 2 — Split into chunks

Large documents are divided into smaller pieces.

Example:

```text
Document
   ↓
Chunk 1
Chunk 2
Chunk 3
Chunk 4
...
```

Why?

Because sending the entire document to the LLM every time is inefficient.

---

# 7. What is chunking?

**Chunking = breaking a large document into smaller pieces of text.**

Example:

```text
100-page document
       ↓
500 chunks
```

Each chunk might contain:

```text
300–1000 tokens
```

depending on the application.

### Why chunk?

Because:

* LLM context is limited
* Retrieval becomes more precise
* Less irrelevant information is sent to the LLM
* Reduces token usage
* Improves retrieval efficiency

---

# 8. Chunk overlap

Sometimes we use **overlapping chunks**.

Example:

```text
Chunk 1:
A B C D E

Chunk 2:
        D E F G H

Chunk 3:
                G H I J K
```

The overlapping portion helps preserve context that might otherwise be lost at chunk boundaries.

### Interview question

**Why do we use overlap?**

> To preserve contextual continuity between chunks, especially when important information spans a chunk boundary.

---

# 9. Embeddings

This is **extremely important for your interview.**

An embedding model converts text into a **numerical vector representation** that captures semantic meaning.

Example:

```text
"I want to take leave"
            ↓
Embedding Model
            ↓
[0.21, -0.43, 0.87, 0.15, ...]
```

The resulting vector represents the semantic meaning of the text.

---

# 10. Why do we need embeddings?

Because computers need a way to compare the **meaning** of pieces of text mathematically.

For example:

```text
"I want to take a vacation"
```

and

```text
"What is the leave policy?"
```

use different words but have related meaning.

Their embeddings should therefore be relatively close in vector space.

---

# 11. Embedding model vs LLM

You asked this previously, and this distinction is **very important**.

### Embedding model

Purpose:

> Convert text into vectors representing semantic meaning.

```text
Text
 ↓
Embedding Model
 ↓
Vector
```

Used for:

* Semantic search
* RAG
* Similarity search
* Recommendation systems
* Clustering

### LLM

Purpose:

> Understand/generate language and perform reasoning/generation.

```text
Prompt + Context
       ↓
      LLM
       ↓
Generated answer
```

### Interview answer

> **“An embedding model converts data into numerical vector representations for similarity-based retrieval, while an LLM is primarily used for understanding context, reasoning and generating natural-language responses.”**

---

# 12. Vector Database

After generating embeddings, we need to store them.

That's where a **vector database** comes in.

```text
Document
   ↓
Chunks
   ↓
Embedding Model
   ↓
Vectors
   ↓
Vector Database
```

Examples:

* FAISS
* Pinecone
* Weaviate
* Milvus
* Chroma
* pgvector

---

# 13. What does the vector database do?

It allows us to perform **similarity search**.

Suppose the user asks:

> "How many casual leaves do I get?"

The query is converted into an embedding:

```text
Question
   ↓
Embedding Model
   ↓
Query Vector
```

The vector database searches for chunks whose vectors are most similar.

```text
Query Vector
     ↓
Vector DB
     ↓
Top-K similar chunks
```

---

# 14. Similarity Search

Common similarity/distance methods include:

### Cosine similarity

Measures the angle between vectors.

```text
similarity ≈ how closely vectors point in the same direction
```

### Euclidean distance

Measures straight-line distance between vectors.

### Dot product

Measures vector alignment and is commonly used in retrieval systems.

For interview purposes:

> **Cosine similarity is a common method for measuring semantic similarity between embeddings.**

---

# 15. Full indexing pipeline

Memorize this:

```text
Documents
    ↓
Document Loader
    ↓
Chunking
    ↓
Embedding Model
    ↓
Vector Embeddings
    ↓
Vector Database
```

This is the **offline/indexing stage**.

---

# 16. Query pipeline

Now the user asks:

> "What is the company's work-from-home policy?"

```text
User Question
      ↓
Embedding Model
      ↓
Query Embedding
      ↓
Vector Database
      ↓
Similarity Search
      ↓
Top-K Relevant Chunks
      ↓
Prompt + Retrieved Context
      ↓
LLM
      ↓
Answer
```

This is the **retrieval + generation stage**.

---

# 17. What exactly happens inside RAG?

Let's take:

> "Can I work from home on Friday?"

### Step 1

User sends question.

### Step 2

Question gets converted to an embedding.

### Step 3

Vector DB searches for semantically similar chunks.

It might return:

```text
Chunk 27:
Employees may work remotely up to 2 days per week...

Chunk 42:
Remote work must be approved by the manager...
```

### Step 4

These chunks are added to the LLM prompt.

Conceptually:

```text
SYSTEM:
Answer using the provided context.

CONTEXT:
Employees may work remotely up to 2 days per week.
Remote work must be approved by the manager.

QUESTION:
Can I work from home on Friday?
```

### Step 5

LLM generates:

> "Yes, employees may work remotely up to two days per week, subject to manager approval."

---

# 18. RAG does NOT train the LLM

This is another **very important interview point**.

Many beginners say:

> "We train the LLM on our company documents using RAG."

That's not technically correct.

RAG usually does:

```text
Company documents
      ↓
Embedding
      ↓
Vector DB
```

Then at query time:

```text
Question
   +
Retrieved documents
   ↓
LLM
```

The model's parameters are **not necessarily modified**.

### RAG vs Fine-tuning

**RAG:**

```text
External knowledge → retrieve → provide as context
```

**Fine-tuning:**

```text
Training data
     ↓
Model training
     ↓
Updated model parameters
```

---

# 19. RAG vs Fine-tuning

| RAG                             | Fine-tuning                             |
| ------------------------------- | --------------------------------------- |
| Adds external context           | Changes model parameters                |
| Good for changing knowledge     | Good for behavior/style/task adaptation |
| Easier to update documents      | Training required                       |
| Can provide source context      | Doesn't inherently provide sources      |
| Useful for enterprise knowledge | Useful for specialized behavior         |

### Interview answer

> “I would generally use RAG when the main requirement is giving an LLM access to changing or private knowledge. Fine-tuning is more appropriate when I want to change the model's behavior, style or ability to perform a specialized task.”

---

# 20. Why RAG reduces hallucination

LLMs can hallucinate.

RAG provides **grounding information**.

Instead of:

```text
Question
   ↓
LLM guesses
```

we have:

```text
Question
   ↓
Retrieve evidence
   ↓
LLM uses evidence
   ↓
Answer
```

But remember:

> **RAG reduces hallucination; it does not guarantee zero hallucinations.**

This is an excellent interview statement.

---

# 21. RAG and citations

A good enterprise RAG system can maintain metadata:

```text
chunk
document_name
page_number
section
source_url
```

Then the answer can include:

> According to the Employee Handbook, employees are eligible for...

```text
Source:
Employee Handbook.pdf
Page 17
```

This makes the answer more **traceable and trustworthy**.

---

# 22. Basic RAG vs Advanced RAG

### Basic RAG

```text
Question
 ↓
Embedding
 ↓
Vector DB
 ↓
Top-K chunks
 ↓
LLM
```

### Advanced RAG

Can include:

```text
Query
 ↓
Query rewriting
 ↓
Hybrid search
 ↓
Metadata filtering
 ↓
Reranking
 ↓
Context compression
 ↓
LLM
 ↓
Citations
```

---

# 23. Hybrid Search

Instead of relying only on vector similarity, combine:

### Semantic search

Uses embeddings.

### Keyword search

Uses exact terms.

Example:

User searches:

> `INC12345`

A keyword search can be better because `INC12345` is an exact identifier.

But:

> "laptop access problem"

may benefit from semantic search.

So:

```text
Keyword Search
       +
Vector Search
       ↓
Combined Results
```

This is called **hybrid retrieval**.

---

# 24. Reranking

Suppose retrieval gives:

```text
Top 10 chunks
```

Not all are equally relevant.

A **reranker** evaluates the retrieved chunks against the query and reorders them.

```text
Vector DB
   ↓
Top 20 candidates
   ↓
Reranker
   ↓
Best 5 chunks
   ↓
LLM
```

This can improve retrieval quality.

---

# 25. Top-K

**K = number of documents/chunks retrieved.**

For example:

```text
Top-K = 5
```

means retrieve the 5 most relevant chunks.

But:

> More chunks ≠ always better.

Too many irrelevant chunks can:

* Increase token usage
* Add noise
* Reduce answer quality
* Increase latency

---

# 26. Context Window

An LLM can only process a limited amount of context at once.

Therefore:

```text
Huge document
      ↓
Retrieve relevant information
      ↓
Send only useful context
      ↓
LLM
```

This is one of the major reasons RAG is useful.

---

# 27. RAG + Agentic AI

This is **especially important for your BMC Helix interview**.

Traditional RAG:

```text
Question
 ↓
Retrieve
 ↓
LLM
 ↓
Answer
```

Agentic RAG:

```text
User
 ↓
Agent
 ↓
Decide what information is needed
 ↓
Choose retrieval/tool
 ↓
Retrieve information
 ↓
Evaluate result
 ↓
Maybe search again
 ↓
Generate answer
```

The agent can decide:

> "I don't have enough information. I need another search."

That's much more flexible.

---

# 28. RAG + MCP

Connect this with the previous lecture.

```text
                  AI Agent
                     ↓
                  MCP
              ┌──────┴──────┐
              ↓             ↓
        RAG/Search      External Tools
              ↓             ↓
        Vector DB         APIs
              ↓             ↓
          Documents      ITSM/CRM
```

For BMC Helix, you could imagine:

```text
User:
"How should I resolve this VPN incident?"

             ↓

          AI Agent
             ↓
       ┌─────┴──────┐
       ↓            ↓
      RAG          MCP
       ↓            ↓
Company KB      ITSM tools
       ↓            ↓
Troubleshooting  Ticket data
       └─────┬──────┘
             ↓
            LLM
             ↓
      Final response/action
```

This is a **very strong architecture to discuss in an Agentic AI interview**.

---

# 29. Your AI Ticket Management project + RAG

Your project currently uses Gemini for:

```text
Ticket
 ↓
Gemini
 ↓
Required Skills
Priority
Moderator Notes
```

You could extend it using RAG.

Suppose a ticket says:

> "VPN authentication fails after password reset."

Your RAG system could search:

```text
Company troubleshooting documents
        ↓
Relevant VPN troubleshooting chunks
        ↓
Gemini
```

Then Gemini can produce:

```text
Likely issue:
VPN authentication configuration

Recommended troubleshooting:
1. Verify credentials
2. Re-authenticate VPN
3. Check MFA configuration
...
```

And with MCP:

```text
RAG → retrieve troubleshooting knowledge

MCP → execute ticket operations
```

So you can tell the interviewer:

> **“RAG can provide the agent with domain-specific knowledge, while MCP can provide tools for taking actions in external systems.”**

---

# 30. Your Agentic Text-to-SQL project + RAG

This is another excellent connection to your resume.

Your project:

```text
User question
      ↓
Agent
      ↓
Schema understanding
      ↓
SQL generation
      ↓
MySQL
```

RAG can help retrieve:

```text
Database schema
Table descriptions
Column descriptions
Example SQL queries
Business rules
```

Example:

> "Show customers who purchased more than ₹1 lakh."

The agent retrieves relevant schema information:

```text
customers
orders
payments
```

Then generates SQL.

This is a strong **Agentic RAG + Text-to-SQL** use case.

---

# 31. RAG limitations

Interviewers may ask:

**"What are the problems with RAG?"**

Mention:

### 1. Poor chunking

Bad chunks → bad retrieval.

### 2. Poor embeddings

Bad embeddings → irrelevant results.

### 3. Retrieval failure

Relevant document may not be retrieved.

### 4. Context overload

Too much retrieved information can confuse the LLM.

### 5. Hallucination

RAG doesn't eliminate hallucination.

### 6. Outdated documents

If the knowledge base isn't updated, answers can still be outdated.

### 7. Security

Sensitive company information must be protected.

### 8. Latency

```text
Embedding
 ↓
Search
 ↓
Reranking
 ↓
LLM
```

adds latency compared with a simple LLM call.

---

# 32. How to improve RAG

If interviewer asks:

> **"How would you improve a RAG system?"**

Give this sequence:

```text
Better document processing
        ↓
Better chunking
        ↓
Better embeddings
        ↓
Hybrid retrieval
        ↓
Metadata filtering
        ↓
Reranking
        ↓
Context compression
        ↓
Good prompt
        ↓
Citations
        ↓
Evaluation
```

That's a strong answer.

---

# 33. RAG Evaluation

You should know the basic idea.

We evaluate:

### Retrieval quality

Did we retrieve the correct information?

### Answer quality

Did the LLM generate the correct answer?

Important concepts:

* Context relevance
* Context recall
* Faithfulness
* Answer relevance
* Groundedness

A useful interview statement:

> **“A RAG system should be evaluated separately for retrieval quality and generation quality, because a wrong answer can originate either from retrieving the wrong context or from the LLM incorrectly using correct context.”**

---

# 34. RAG interview questions

### Q1. What is RAG?

> Retrieval-Augmented Generation combines retrieval from an external knowledge source with LLM generation. Relevant information is retrieved and provided to the LLM as context.

### Q2. Why use RAG?

> To provide LLMs with private, domain-specific or frequently changing information without necessarily retraining the model.

### Q3. What is an embedding?

> A numerical vector representation of data that captures semantic meaning and allows similarity-based search.

### Q4. Why use a vector database?

> To efficiently store and retrieve embeddings based on similarity.

### Q5. What is chunking?

> Breaking documents into smaller pieces so relevant information can be retrieved efficiently and supplied to the LLM.

### Q6. What is chunk overlap?

> Repeating a portion between consecutive chunks to preserve context across chunk boundaries.

### Q7. RAG vs fine-tuning?

> RAG provides external knowledge at inference time, while fine-tuning changes model parameters to adapt behavior or specialize the model.

### Q8. Does RAG eliminate hallucination?

> No. It can reduce hallucination by grounding responses in retrieved information, but incorrect retrieval or incorrect generation can still produce hallucinations.

### Q9. What is hybrid search?

> Combining semantic vector search with keyword-based search to improve retrieval across both conceptual queries and exact terms.

### Q10. What is reranking?

> A second-stage model reorders retrieved candidates according to their relevance to the query.

---

# 🔥 35. MOST IMPORTANT: Learn this architecture

For tomorrow, I would memorize this:

```text
                 USER
                   ↓
              USER QUERY
                   ↓
              ┌─────────┐
              │  AGENT  │
              └────┬────┘
                   ↓
           Need external knowledge?
                   ↓
             QUERY EMBEDDING
                   ↓
              VECTOR DB
                   ↓
              TOP-K CHUNKS
                   ↓
               RERANKER
                   ↓
          RELEVANT CONTEXT
                   ↓
          ┌────────────────┐
          │      LLM       │
          │ Query + Context│
          └───────┬────────┘
                  ↓
             FINAL ANSWER
```

And if actions are required:

```text
                    AGENT
                  ↙       ↘
                RAG       MCP
                 ↓         ↓
             Knowledge   Tools
                Base      APIs
                 ↓         ↓
                  └───┬────┘
                      ↓
                     LLM
                      ↓
              Answer / Action
```

## ⭐ 10 things to memorize before BMC Helix

1. **RAG = Retrieval-Augmented Generation**
2. RAG = **Retrieve → Augment → Generate**
3. **Embedding model ≠ LLM**
4. Embeddings convert text → vectors
5. Vector DB stores/searches vectors
6. **Chunking** breaks documents into retrievable pieces
7. **RAG does not necessarily train/fine-tune the LLM**
8. **RAG ≠ MCP**
9. **RAG gives knowledge; MCP gives capabilities/tools**
10. **Agentic RAG can decide when/how to retrieve and potentially use tools**

### One answer that ties everything together

If BMC asks:

> **“How would you build an AI assistant for an enterprise?”**

A strong answer is:

> **“I would use an LLM as the reasoning and generation layer. For company-specific knowledge, I would build a RAG pipeline where documents are chunked, converted into embeddings and stored in a vector database. At query time, the user's question is embedded, relevant chunks are retrieved and optionally reranked, and the LLM generates a grounded answer using that context. For actions such as creating or updating ITSM tickets, I could expose those capabilities through tools, potentially using MCP as a standardized integration layer. An agent can then decide whether it needs knowledge from RAG, an external tool, or both.”**

That answer connects **LLM + embeddings + vector DB + RAG + Agentic AI + MCP + enterprise automation** — exactly the cluster you should be prepared to discuss tomorrow.

[1]: https://t.me/s/aman_dhattarwal?after=2090&utm_source=chatgpt.com "Apna College – Telegram"
