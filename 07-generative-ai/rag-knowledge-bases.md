# RAG (Retrieval Augmented Generation) & Bedrock Knowledge Bases

## 🤔 What Is RAG?

An open-book exam for LLMs. Instead of relying on what the model memorized during training, it searches your data first, then answers based on what it found.

```
Without RAG:
  You: "What did the president say yesterday?"
  LLM: "I don't know, my training data ended months ago" ❌

With RAG:
  You: "What did the president say yesterday?"
  RAG: → searches your news database → finds the article
  LLM: "The president announced a new policy on..." ✅
```

---

## 🔧 How RAG Works (Simple Version)

```
Step 1: User asks a question
        "What's our refund policy?"
              ↓
Step 2: Search your database/knowledge base
        → Finds: "Refund policy document: 30 days with receipt..."
              ↓
Step 3: Stuff the search result INTO the prompt
        New prompt becomes:
        "What's our refund policy? 
         Consider this document: '30 days with receipt...'"
              ↓
Step 4: LLM reads the augmented prompt → generates answer
        "Your refund policy allows returns within 30 days 
         with a valid receipt."
```

```
RAG = search your data + inject into prompt + let LLM answer

That's it. It's just a search result being rephrased by AI.
```

---

## 🗄️ Vector Database — The Search Engine Behind RAG

RAG needs to search your data. A vector database makes that search smart.

```
Regular database search:
  Query: "refund policy"
  → Searches for exact words "refund" and "policy"
  → Might miss: "return items within 30 days" (no matching words)

Vector database search (semantic search):
  Query: "refund policy"
  → Converts query to embedding vector
  → Finds documents with SIMILAR MEANING
  → Finds: "return items within 30 days" ✅ (same meaning!)
```

```
Regular search: matches WORDS
Vector search:  matches MEANING (using embeddings you already learned)
```

How documents get into the vector database:

```
Your documents → split into chunks → each chunk converted to embedding
→ stored in vector database

Query comes in → converted to embedding → find closest chunks
→ return most relevant chunks → inject into prompt
```

---

## ✅ Pros of RAG

```
→ Cheaper and faster than fine-tuning (no model retraining)
→ Easy to update (just update the database, no retraining)
→ Keeps data current (new docs added anytime)
→ Semantic search (finds by meaning, not just keywords)
→ Reduces hallucinations (gives model real data to work with)
→ Technically not "training" on the data (legal/licensing benefit)
```

---

## ❌ Cons of RAG

```
→ Increases tokens per request (search results added to prompt)
→ Sensitive to prompt template (how you inject data matters)
→ Non-deterministic (hard to test, different answers each time)
→ Can still hallucinate (less, but still possible)
→ Very sensitive to chunk quality:
    → If chunks don't contain coherent thoughts → bad results
    → Fixed-size chunking can split sentences mid-thought
→ Expensive vector storage and computation
→ At the end of the day: a fancy search engine with AI rephrasing
```

---

## 🥊 RAG vs Fine-Tuning

```
┌──────────────────┬──────────────────────┬──────────────────────┐
│                  │     Fine-Tuning      │        RAG           │
├──────────────────┼──────────────────────┼──────────────────────┤
│ Data goes where  │ Baked INTO model     │ Stays in database    │
│ Setup cost       │ Expensive            │ Cheaper              │
│ Per-request cost │ Cheap (no extra      │ More tokens per      │
│                  │ tokens needed)       │ request              │
│ Update data      │ Retrain model (slow) │ Update database      │
│                  │                      │ (instant)            │
│ Best for         │ Style, tone,         │ Live data, docs,     │
│                  │ permanent knowledge  │ frequently changing  │
│ Speed to deploy  │ Hours/days           │ Minutes/hours        │
└──────────────────┴──────────────────────┴──────────────────────┘
```

```
Data changes often?        → RAG
Data is static + need tone? → Fine-tuning
Best results?              → Both together
```

---

## 🏗️ RAG in Bedrock = Knowledge Bases

In Bedrock, RAG is called "Knowledge Bases."

```
You create a Knowledge Base:
  → Point it to your data in S3
  → Bedrock chunks it and creates embeddings
  → Stores in a vector database
  → Ready to search

User asks a question:
  → Bedrock searches Knowledge Base
  → Injects results into prompt
  → Foundation model answers

API calls:
  → retrieve: just search the knowledge base
  → retrieve_and_generate: search + generate answer
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│            RAG & KNOWLEDGE BASES — QUICK RECALL              │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  RAG = search external data + inject into prompt + answer    │
│  Bedrock name: Knowledge Bases                               │
│                                                              │
│  How: question → search vector DB → inject result → LLM     │
│                                                              │
│  Vector database:                                            │
│  → Stores document chunks as embeddings                      │
│  → Semantic search (by meaning, not keywords)                │
│                                                              │
│  Pros: cheap setup, easy to update, reduces hallucinations   │
│  Cons: more tokens per request, sensitive to chunk quality,  │
│         can still hallucinate, expensive vector storage       │
│                                                              │
│  RAG vs Fine-tuning:                                         │
│  → RAG: data changes often, quick setup                      │
│  → Fine-tuning: permanent knowledge, saves tokens long-term  │
│                                                              │
│  EXAM TIPS:                                                  │
│  "Keep LLM answers current?"        → RAG                    │
│  "Search company docs with LLM?"    → Knowledge Base (RAG)   │
│  "Reduce hallucinations?"           → RAG (but not eliminate)│
│  "Update data without retraining?"  → RAG                    │
│  "Semantic search?"                 → Vector database         │
│  "Bedrock name for RAG?"            → Knowledge Bases         │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
