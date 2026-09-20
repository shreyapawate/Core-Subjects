Yes. I checked the video reference and the material associated with it. This lecture is about **how LLMs work internally — tokenization, embeddings, positional information, attention/transformers, decoding, probabilities and generation**. The linked material also covers the transition from RNNs to Transformers and the encoder/decoder concepts. ([DEV Community][1])

For your **BMC Helix interview tomorrow**, don't study this like a deep ML-theory lecture. Focus on being able to **explain the LLM pipeline clearly and connect it to Agentic AI, RAG and tool calling**.

# LLMs — Interview Notes

## 1. What is an LLM?

**LLM = Large Language Model**

An LLM is a neural network trained on a very large amount of text to learn patterns in language and generate text.

Examples:

* GPT
* Gemini
* Claude
* Llama
* Mistral

### Interview answer

> **“An LLM is a neural network trained on large-scale text data to understand patterns in language and generate text. During inference, it predicts the next token based on the context provided to it.”**

The most important phrase:

> **LLM = next-token prediction at its core.**

---

# 2. High-Level LLM Pipeline

Understand this entire flow:

```text
User Input
    ↓
Tokenization
    ↓
Token IDs
    ↓
Token Embeddings
    ↓
Positional Information
    ↓
Transformer Layers
    ↓
Self-Attention
    ↓
Feed-Forward Networks
    ↓
Output Representations
    ↓
Linear Layer
    ↓
Logits
    ↓
Softmax
    ↓
Token Probabilities
    ↓
Next Token
    ↓
Repeat
    ↓
Final Response
```

This is one of the most important diagrams for your interview.

---

# 3. Tokenization

LLMs don't directly process raw sentences.

First, text is converted into **tokens**.

Example:

```text
"I love AI"
```

could become something conceptually like:

```text
["I", "love", "AI"]
```

Modern tokenizers often use **subword tokens**, so a word may be split into multiple pieces.

For example:

```text
unbelievable
```

could potentially become:

```text
["un", "believ", "able"]
```

The exact splitting depends on the tokenizer.

### Why tokenization?

Neural networks operate on numbers.

So:

```text
Text
 ↓
Tokens
 ↓
Token IDs
 ↓
Vectors
```

---

# 4. Token ID

Each token is mapped to an integer from the model's vocabulary.

Conceptually:

```text
"I"       →  40
"love"    →  982
"AI"      →  3142
```

These numbers themselves **don't contain semantic meaning**.

They are simply identifiers used to look up embeddings.

---

# 5. Embeddings

This connects directly to the RAG lecture you just studied.

A token ID is converted into a vector.

```text
Token ID
   ↓
Embedding lookup
   ↓
Vector
```

For example:

```text
AI
 ↓
[0.12, -0.45, 0.83, ...]
```

The actual vector has many dimensions.

### Important distinction

**Embedding = numerical representation**

It allows the neural network to process language mathematically.

---

# 6. Embedding in LLM vs Embedding Model in RAG

This is a **very good interview question**.

Both involve vectors, but their purpose can differ.

### LLM token embeddings

Used inside the model to convert tokens into representations that the Transformer can process.

```text
Token
 ↓
Token Embedding
 ↓
Transformer
```

### Dedicated embedding model

Used to convert text into vectors for tasks such as:

* Semantic search
* RAG
* Similarity
* Retrieval

```text
Document
 ↓
Embedding Model
 ↓
Vector
 ↓
Vector DB
```

### Interview answer

> “Both produce vector representations, but token embeddings are part of an LLM's internal processing pipeline, whereas dedicated embedding models are commonly used to create vectors for retrieval and semantic search.”

---

# 7. Why can't Transformers just use embeddings?

Because embeddings don't inherently tell the model **where a token occurs in the sequence**.

Consider:

```text
Dog bites man.
```

vs

```text
Man bites dog.
```

The same words are present, but their order changes the meaning.

Therefore, Transformers need **positional information**.

---

# 8. Positional Encoding / Positional Information

Positional information tells the model where each token occurs.

Conceptually:

```text
Token        Position
---------------------
The             1
cat             2
sat             3
there           4
```

The model combines token information with positional information.

```text
Token Embedding
       +
Positional Information
       ↓
Transformer Input
```

### Interview answer

> **“Positional encoding provides information about token order because the self-attention mechanism itself does not inherently encode sequence position.”**

---

# 9. Why Transformers were revolutionary

Before Transformers, models such as **RNNs/LSTMs** were widely used for sequence processing.

They process information sequentially.

```text
Token 1
  ↓
Token 2
  ↓
Token 3
  ↓
Token 4
```

This creates difficulties with:

* Long-range dependencies
* Parallelization
* Training speed

Transformers introduced **self-attention**, allowing tokens to directly interact with other tokens within the sequence. ([DEV Community][1])

---

# 10. Self-Attention

This is the **most important concept from this lecture**.

Self-attention allows each token to determine which other tokens are important when processing it.

Example:

> "The dog chased the ball because it was moving."

What does **"it"** refer to?

The model needs to examine other words and determine their relationships.

Self-attention helps create these contextual relationships.

---

# 11. Query, Key, Value

This is one of the most frequently asked Transformer questions.

Self-attention uses:

### Query (Q)

> **What am I looking for?**

### Key (K)

> **What information do I represent?**

### Value (V)

> **What information should I provide if I'm relevant?**

Memory trick:

```text
Q = What do I need?
K = What do you contain?
V = What information do you give me?
```

---

# 12. Attention Calculation

At a simplified level:

```text
Q × Kᵀ
   ↓
Attention Scores
   ↓
Scaling
   ↓
Softmax
   ↓
Attention Weights
   ↓
Weighted V
   ↓
Output
```

The commonly presented formula is:

$$
Attention(Q,K,V)
=
softmax\left(\frac{QK^T}{\sqrt{d_k}}\right)V
$$

### Interview-level explanation

You don't need to derive the formula tomorrow.

Say:

> “The query and keys are compared to calculate attention scores. The scores are scaled and passed through softmax to obtain attention weights. These weights are then used to create a weighted combination of the value vectors.”

That's enough for most software/AI interviews.

---

# 13. Why divide by √dk?

This is a common follow-up.

If the dimensionality of the key vectors becomes large, the dot products can become large.

Large values can make softmax extremely peaked.

Scaling by:

$$
\sqrt{d_k}
$$

helps keep the values in a more stable range.

### Interview answer

> “The scaling factor helps prevent excessively large dot products and keeps the softmax gradients more stable.”

---

# 14. Self-Attention Example

Consider:

> **"The cat drank the milk because it was thirsty."**

When processing **"it"**, the model can assign different attention weights to surrounding words.

Conceptually:

```text
it
 ↓
cat       HIGH
drank     LOW
milk      LOW
thirsty   HIGH
```

The exact attention values are learned by the model; this is just an illustration.

The important idea:

> **Attention helps the model determine relationships between tokens.**

---

# 15. Multi-Head Attention

Instead of having one attention mechanism, Transformers use multiple attention heads.

```text
             Input
               ↓
       ┌───────┼───────┐
       ↓       ↓       ↓
     Head 1   Head 2   Head 3 ...
       ↓       ↓       ↓
       └───────┼───────┘
               ↓
          Concatenate
               ↓
        Linear Projection
```

Different heads can learn different relationships.

For example, conceptually:

```text
Head 1 → grammatical relationships
Head 2 → positional relationships
Head 3 → semantic relationships
Head 4 → long-range dependencies
```

Don't say that every head is explicitly assigned one of these roles. Rather:

> **Different heads can learn different patterns/relationships.**

---

# 16. Why Multi-Head Attention?

Because a single attention operation may not capture all relationships effectively.

Multiple heads allow the model to process different representation subspaces in parallel.

### Interview answer

> “Multi-head attention allows the model to attend to different aspects of the input simultaneously by performing attention in multiple representation subspaces.”

---

# 17. Transformer Block

A simplified Transformer block looks like:

```text
Input
  ↓
Multi-Head Self-Attention
  ↓
Add & Normalize
  ↓
Feed-Forward Network
  ↓
Add & Normalize
  ↓
Output
```

Modern architectures can contain additional details, but this is the core conceptual structure.

---

# 18. Feed-Forward Network

After attention, the representation goes through a **feed-forward neural network**.

Conceptually:

```text
Attention Output
      ↓
Linear Layer
      ↓
Activation
      ↓
Linear Layer
      ↓
Output
```

The feed-forward network applies nonlinear transformations independently to each token position.

### Interview answer

> “Self-attention allows tokens to interact with each other, while the feed-forward network further transforms the representation at each position.”

---

# 19. Residual Connections

Transformer blocks use residual/skip connections.

Conceptually:

```text
Input ───────────────┐
                     ↓
               Add / Residual
                     ↑
Input → Attention → Output
```

Why?

They help information and gradients flow through deep networks.

---

# 20. Layer Normalization

Transformers also use normalization around their sublayers.

Purpose:

* Stabilize training
* Improve optimization
* Keep activations well behaved

You don't need to memorize the mathematical formula for tomorrow unless the role specifically demands deep ML.

---

# 21. Encoder vs Decoder

This is important because people often incorrectly say:

> "GPT has an encoder and decoder."

That's not correct.

There are different Transformer architectures.

### Encoder-only

Example:

**BERT**

Used mainly for understanding/representation tasks.

```text
Input
 ↓
Encoder
 ↓
Representation
```

### Decoder-only

Examples:

* GPT-style models
* Many modern generative LLMs

Used for autoregressive generation.

```text
Input tokens
 ↓
Decoder-only Transformer
 ↓
Next-token prediction
```

### Encoder-decoder

Examples include architectures used for sequence-to-sequence tasks such as translation.

```text
Input
 ↓
Encoder
 ↓
Decoder
 ↓
Output
```

### Interview trap

**Q: Does GPT use an encoder and decoder?**

Answer:

> **“GPT-style models are decoder-only Transformers. The original Transformer architecture had both encoder and decoder components.”**

---

# 22. Decoder-only LLM

For a GPT-style model:

```text
Prompt
  ↓
Tokens
  ↓
Embeddings
  ↓
Transformer blocks
  ↓
Logits
  ↓
Next-token probabilities
  ↓
Select next token
  ↓
Repeat
```

This is called **autoregressive generation**.

---

# 23. Autoregressive Generation

The model generates one token at a time.

Example:

```text
Input:
"What is AI?"

Step 1:
"What is AI? AI"

Step 2:
"What is AI? AI is"

Step 3:
"What is AI? AI is a"

Step 4:
"What is AI? AI is a technology"
```

Each newly generated token becomes part of the context for predicting the next one.

---

# 24. Logits

After the Transformer processes the input, a final linear layer produces **logits**.

Suppose the vocabulary contains:

```text
apple
banana
car
dog
```

The model might produce:

```text
apple     → 2.1
banana    → 1.2
car       → 0.4
dog       → -0.8
```

These raw scores are called **logits**.

They are not probabilities yet.

---

# 25. Softmax

Softmax converts logits into a probability distribution.

Conceptually:

```text
Logits
  ↓
Softmax
  ↓
Probabilities
```

Example:

```text
apple     0.65
banana    0.25
car       0.08
dog       0.02
```

The probabilities sum to approximately 1.

---

# 26. How does the LLM choose the next token?

The model produces probabilities.

Then a **decoding strategy** determines which token to select.

Common strategies:

### Greedy decoding

Select the highest-probability token.

```text
apple = 0.65
banana = 0.25

→ choose apple
```

### Sampling

Randomly sample according to probabilities.

### Temperature

Controls how concentrated or spread out the distribution is.

---

# 27. Temperature

Temperature controls the randomness of sampling.

### Low temperature

More deterministic.

```text
Temperature ↓
     ↓
More predictable output
```

### High temperature

More randomness.

```text
Temperature ↑
     ↓
More varied output
```

Important:

> **Temperature does not make the model more intelligent.**

It changes the sampling distribution.

---

# 28. Top-K and Top-P

These are useful to know.

### Top-K

Only consider the K most probable tokens.

Example:

```text
Top-K = 5
```

Only the 5 highest-probability candidates are considered.

### Top-P / nucleus sampling

Select the smallest set of tokens whose cumulative probability reaches a threshold P.

Example:

```text
Top-P = 0.9
```

The candidate set dynamically changes depending on the probability distribution.

---

# 29. Training vs Inference

This is a **must-know distinction**.

### Training

The model learns its parameters.

```text
Huge Dataset
     ↓
Tokens
     ↓
Model
     ↓
Prediction
     ↓
Compare with target
     ↓
Loss
     ↓
Backpropagation
     ↓
Update weights
```

Repeated many times.

### Inference

The trained model is used to generate an answer.

```text
User Prompt
     ↓
Trained Model
     ↓
Prediction
     ↓
Output
```

Normally, the model's weights aren't updated during ordinary inference.

---

# 30. Loss Function

During training, the model predicts the next token.

Suppose the correct token is:

```text
"cat"
```

but model predicts:

```text
dog = 0.6
cat = 0.2
```

The model receives a high loss.

Training tries to reduce that loss.

For language models, **cross-entropy loss** is commonly used.

---

# 31. Backpropagation

Backpropagation calculates how the loss changes with respect to the model's parameters.

Then an optimizer updates the weights.

Simplified:

```text
Prediction
    ↓
Loss
    ↓
Backpropagation
    ↓
Gradients
    ↓
Optimizer
    ↓
Updated weights
```

You don't need to explain all calculus unless asked.

---

# 32. Pretraining

During pretraining, the model learns general language patterns from huge datasets.

It learns things such as:

* Grammar
* Syntax
* Semantic relationships
* Facts/patterns present in training data
* Code patterns
* General language structures

At the core, many autoregressive LLMs learn by predicting the next token.

---

# 33. Fine-Tuning

After pretraining, a model can be adapted for specific tasks or behaviors.

```text
Base Model
     ↓
Fine-tuning
     ↓
Specialized Model
```

Examples:

* Instruction following
* Domain-specific behavior
* Classification
* Style adaptation

Remember the distinction from RAG:

```text
Fine-tuning → changes model parameters

RAG → supplies external information at inference time
```

---

# 34. LLM + RAG

Now connect today's lecture to your previous lecture.

### LLM alone

```text
User
 ↓
LLM
 ↓
Answer
```

### LLM + RAG

```text
User
 ↓
Question
 ↓
Retriever
 ↓
Vector DB
 ↓
Relevant Context
 ↓
LLM
 ↓
Answer
```

The LLM is still doing the generation.

RAG supplies **external context**.

---

# 35. LLM + Agent

Now connect it to Agentic AI.

A normal LLM:

```text
Question
 ↓
LLM
 ↓
Answer
```

An agent:

```text
Goal
 ↓
LLM
 ↓
Reason / Decide
 ↓
Choose Tool
 ↓
Execute
 ↓
Observe Result
 ↓
LLM
 ↓
Next Action
 ↓
Final Answer
```

This is why **LLM ≠ Agent**.

---

# 36. LLM + RAG + MCP + Agent

This is the architecture I want you to be able to explain in your BMC Helix interview:

```text
                         USER
                           ↓
                        AGENT
                           ↓
                         LLM
                    ↙      ↓      ↘
                  RAG    Reason    MCP
                   ↓              ↓
              Vector DB         Tools
                   ↓              ↓
              Company KB       ITSM APIs
                    \             /
                     \           /
                       Agent
                         ↓
                    Final Answer
                    /        \
              Information    Action
```

### Example

User:

> "Why was my incident rejected, and can you reopen it?"

Agent can:

**RAG**

Retrieve the company's incident-management policy.

**MCP/tool**

Call:

```text
get_incident()
update_incident()
```

**LLM**

Explain the policy and communicate the result.

This is an excellent enterprise **Agentic AI** example.

---

# 37. Most Important Interview Distinctions

Memorize this table:

| Concept             | Main purpose                                                    |
| ------------------- | --------------------------------------------------------------- |
| **LLM**             | Understand/generate language                                    |
| **Embedding model** | Convert data into vectors for semantic representation/retrieval |
| **Vector DB**       | Store/search vectors                                            |
| **RAG**             | Give LLM external knowledge                                     |
| **Agent**           | Decide and execute multi-step tasks                             |
| **Tool calling**    | Allow model/agent to invoke actions                             |
| **MCP**             | Standardize AI ↔ tools/resources integration                    |
| **Fine-tuning**     | Adapt model parameters/behavior                                 |

---

# 38. Very Important Interview Questions

### Q1. How does an LLM generate text?

> The input is tokenized and converted into embeddings with positional information. Transformer layers process the sequence using self-attention and feed-forward networks. A final layer produces logits over the vocabulary, which are converted into probabilities, and a decoding strategy selects the next token. This process repeats autoregressively.

---

### Q2. What is self-attention?

> Self-attention allows each token to assign different importance to other tokens in the input sequence so that the model can capture contextual relationships.

---

### Q3. What are Q, K and V?

> Query represents what a token is looking for, Key represents what each token offers for matching, and Value contains the information that is aggregated according to the attention weights.

---

### Q4. Why do we need positional encoding?

> Because attention alone doesn't inherently represent the order of tokens. Positional information allows the model to distinguish different token positions.

---

### Q5. What is multi-head attention?

> It performs multiple attention operations in parallel over different representation subspaces, allowing the model to capture different relationships in the sequence.

---

### Q6. What are logits?

> Logits are the raw scores produced by the model for each possible next token before they are converted into probabilities.

---

### Q7. Why is softmax used?

> Softmax converts the logits into a probability distribution over possible next tokens.

---

### Q8. What is temperature?

> Temperature controls the randomness of token sampling. Lower values make outputs more deterministic, while higher values make sampling more diverse.

---

### Q9. What is hallucination?

> Hallucination occurs when an LLM generates information that is incorrect, unsupported or fabricated.

---

### Q10. How can you reduce hallucination?

Mention:

```text
RAG
 ↓
Grounding
 ↓
Good prompts
 ↓
Source citations
 ↓
Tool verification
 ↓
Output validation
 ↓
Evaluation
```

Don't claim that any one technique completely eliminates hallucinations.

---

# 39. BMC Helix-style interview scenario

### Interviewer:

> "Suppose we want an AI assistant for IT support. How would you design it?"

### Your answer:

> "I would use an LLM as the reasoning and language-generation layer. For company-specific information, I would implement RAG by processing IT documentation into chunks, generating embeddings and storing them in a vector database. When a user asks a question, the system retrieves relevant documentation and provides it to the LLM as context. For actions such as creating or updating incidents, I would expose appropriate tools, potentially through MCP. An agent could then decide whether it needs to retrieve information, invoke an ITSM tool, or perform multiple steps before giving the user a final response."

**This is the answer you should practice.**

---

# 🔥 40. Last-Minute Revision Sheet

If you only have **30 minutes**, memorize this:

```text
LLM
= Large Language Model
= predicts next token

PIPELINE

Text
 ↓
Tokenization
 ↓
Token IDs
 ↓
Embeddings
 ↓
Positional Information
 ↓
Transformer
 ↓
Self-Attention
 ↓
Feed Forward
 ↓
Logits
 ↓
Softmax
 ↓
Next Token
 ↓
Repeat
```

### Transformer

```text
Self-Attention
    ↓
Q, K, V
    ↓
Attention Scores
    ↓
Softmax
    ↓
Weighted Values
```

### Multi-head

```text
Multiple attention heads
        ↓
Different representation subspaces
        ↓
Combine
```

### Generation

```text
Logits
 ↓
Softmax
 ↓
Probabilities
 ↓
Decoding
 ↓
Next Token
```

### Training

```text
Dataset
 ↓
Prediction
 ↓
Loss
 ↓
Backpropagation
 ↓
Weight Update
```

### RAG

```text
Documents
 ↓
Chunks
 ↓
Embeddings
 ↓
Vector DB

Question
 ↓
Embedding
 ↓
Retrieve
 ↓
Context
 ↓
LLM
 ↓
Answer
```

### Agentic AI

```text
Goal
 ↓
LLM
 ↓
Reason
 ↓
Tool
 ↓
Observe
 ↓
Reason
 ↓
Action
```

### MCP

```text
Agent
 ↓
MCP Client
 ↓
MCP Server
 ↓
Tools / Resources / Prompts
 ↓
External Systems
```

## ⭐ One mental model for your interview

Think of the whole thing as:

> **LLM = Brain**
> **RAG = Knowledge**
> **Embedding = Mathematical representation of meaning**
> **Vector DB = Knowledge retrieval system**
> **Agent = Decision-maker/orchestrator**
> **Tools = Hands/actions**
> **MCP = Standardized connection to those tools**

If you can explain that mental model **and then technically drill down into Q/K/V, embeddings, vector search, RAG and tool calling**, you'll be able to handle a large portion of an Agentic AI technical discussion.

[1]: https://dev.to/piyushpk/how-gpt-works-42h8?utm_source=chatgpt.com "How GPT Works Behind The Scene - DEV Community"
