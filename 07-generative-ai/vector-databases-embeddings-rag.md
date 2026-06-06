# Vector Databases & Embeddings in RAG

## 🔑 Quick Recap: What RAG Needs

RAG needs to search your data and find the most RELEVANT results. The quality of that search = the quality of the answer. Vector databases make that search smart.

---

## 📐 Embeddings (Review)

You already know this — embeddings are vectors (lists of numbers) that capture MEANING.

```
"potato"    = [0.2, 0.8, 0.1, ...]   
"rhubarb"   = [0.3, 0.7, 0.2, ...]   ← close to potato (both food)
"starship"  = [0.9, 0.1, 0.8, ...]   ← far from potato (not food)

Similar meaning = close vectors
Different meaning = far vectors
```

In practice these vectors are hundreds or thousands of dimensions — not just 3 numbers. But the concept is the same.

### How embeddings are computed:

```
→ Use an embedding model (like Amazon Titan Embeddings)
→ Feed it text → get back a vector
→ This is already built into transformers (tokenization → embedding)
→ Cheap and fast to compute
```

---

## 🗄️ Vector Database

Stores your text chunks alongside their embedding vectors. Enables semantic search.

```
What's stored:

  Chunk 1: "Returns accepted within 30 days..."  → [0.2, 0.8, 0.1, ...]
  Chunk 2: "Free shipping on orders over $50..."  → [0.5, 0.3, 0.7, ...]
  Chunk 3: "Contact support at help@..."          → [0.1, 0.6, 0.4, ...]

  Text + its embedding vector, side by side.
```

### How search works:

```
User asks: "How do I return an item?"
                ↓
Convert question to embedding vector: [0.25, 0.75, 0.15, ...]
                ↓
Find closest vectors in the database (K nearest neighbors):
  Chunk 1: distance = 0.05  ← very close! (about returns) ✅
  Chunk 2: distance = 0.82  ← far (about shipping)
  Chunk 3: distance = 0.71  ← far (about contact)
                ↓
Return Chunk 1 as the most relevant result
                ↓
Inject into prompt → LLM answers based on it
```

```
Vector search = "which stored chunk is closest in MEANING to my question?"

That's it. Just measuring distance between vectors.
Same concept as KNN (K-Nearest Neighbors) that you learned earlier.
```

### Distance metric:

```
Usually cosine similarity:
  → Measures the angle between two vectors
  → Closer angle = more similar meaning
  → Standard choice for semantic search
```

### Optimization:

```
Millions of chunks? Don't search ALL of them.
Vector databases use tricks to narrow down the search space.
You don't do a linear scan through everything.
```

---

## 🔧 Database Options for RAG

### Not just vector databases — any database can work:

```
Vector databases (semantic search):
  → OpenSearch ← Bedrock's default choice (Amazon product)
  → Elasticsearch
  → Pinecone ← most popular purpose-built vector DB
  → Weaviate, Chroma, Milvus, Qdrant

Traditional databases (added vector support):
  → PostgreSQL (pgvector)
  → MongoDB
  → Redis
  → Cassandra

Graph databases (for relationships):
  → Neptune
  → Neo4j
  → Good for: recommendations, knowledge graphs

Text search (old school):
  → OpenSearch with TF-IDF
  → Traditional keyword matching
  → Still works fine for many cases
```

### Hybrid approach:

```
Combine traditional search + semantic search
→ Keyword matching catches exact terms
→ Semantic search catches similar meanings
→ Together = better results than either alone
```

---

## 🔄 Full RAG Flow (End to End)

```
SETUP (once):
  Your documents (PDFs, text, etc.)
       ↓
  Split into chunks
       ↓
  Each chunk → embedding model → vector
       ↓
  Store chunk + vector in vector database


RUNTIME (every query):
  User question: "Tell me about returns"
       ↓
  Question → embedding model → query vector
       ↓
  Search vector database (find K nearest chunks)
       ↓
  Top results: ["Returns accepted within 30 days..."]
       ↓
  Build prompt: "Answer this question considering: [results]"
       ↓
  LLM generates answer
       ↓
  User gets response
```

---

## ⚠️ What Affects Quality

```
→ Chunk size: too big = irrelevant noise, too small = missing context
→ K value: how many results to include (too many = noise, too few = missing info)
→ Prompt template: how you inject results into the prompt
→ Embedding model quality: better embeddings = better search
→ Chunk boundaries: splitting mid-sentence = bad results
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│     VECTOR DATABASES & RAG — QUICK RECALL                    │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  Embeddings: vectors that capture meaning                    │
│  → Similar text = close vectors                              │
│  → Computed by embedding models (Titan, etc.)                │
│                                                              │
│  Vector database: stores text + embedding vectors            │
│  → Search by meaning (semantic search)                       │
│  → Uses KNN / cosine similarity                              │
│                                                              │
│  Bedrock default: OpenSearch (Amazon product)                │
│  Popular standalone: Pinecone                                │
│  Others: PostgreSQL, MongoDB, Redis (all added vector support│
│                                                              │
│  RAG flow: question → embed → search → inject → LLM → answer│
│                                                              │
│  Quality depends on:                                         │
│  → Chunk size and boundaries                                 │
│  → K value (how many results)                                │
│  → Prompt template                                           │
│  → Embedding model quality                                   │
│                                                              │
│  EXAM TIPS:                                                  │
│  "Semantic search in Bedrock?"    → OpenSearch vector DB     │
│  "Store embeddings?"              → Vector database          │
│  "Search by meaning not keywords?"→ Semantic search          │
│  "Distance metric for vectors?"   → Cosine similarity        │
│  "KNN in RAG context?"            → Finding nearest chunks   │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
