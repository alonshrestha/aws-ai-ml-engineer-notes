# Bedrock Knowledge Bases — Practical Setup

## 🔧 How to Set Up a Knowledge Base in Bedrock

Three things you need: data source, embedding model, vector store.

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  Data Source  │ →   │  Embedding   │ →   │  Vector      │
│  (your docs) │     │  Model       │     │  Store       │
└──────────────┘     └──────────────┘     └──────────────┘
```

---

## 📂 Step 1: Data Source (Where Your Docs Live)

```
Options:
  → S3 bucket (most common — upload PDFs, text, JSON, etc.)
  → Web crawler (crawl web pages you have permission to)
  → Third-party connectors:
    → Confluence
    → Salesforce
    → SharePoint
```

Data can be structured or unstructured — raw text, JSON, whatever you have.

---

## 🧮 Step 2: Embedding Model (Convert Text to Vectors)

```
Available models:
  → Amazon Titan Embeddings ← Amazon's own
  → Cohere Embeddings

You control:
  → Vector dimension (how many numbers per vector)
```

This model converts each chunk of your document into an embedding vector.

---

## 🗄️ Step 3: Vector Store (Where Vectors Are Stored)

```
Default: OpenSearch (Amazon product, auto-created)

Other options:
  → MemoryDB (newer, also has vector support)
  → Aurora (PostgreSQL)
  → MongoDB Atlas
  → Pinecone
  → Redis Enterprise Cloud
```

---

## ✂️ Chunking: How Documents Get Split

Your document gets split into chunks before embedding. You control how.

```
Default: 300 characters per chunk

Options:
  → Fixed size (300 chars, 500 chars, etc.)
  → Control overlap between chunks
  → Custom chunking logic
```

### Chunking quality matters A LOT:

```
Bad chunking (fixed 300 chars):
  Chunk 1: "Our refund policy allows returns within"
  Chunk 2: "30 days with a valid receipt. Free shipping"
  
  → Sentence split mid-thought. Search might miss it.

Better chunking (by paragraph/sentence):
  Chunk 1: "Our refund policy allows returns within 30 days 
            with a valid receipt."
  Chunk 2: "Free shipping on orders over $50."
  
  → Each chunk has a complete thought. Better search results.
```

```
Better alternatives to fixed chunking:
  → By sentences or paragraphs
  → Structured data (JSON, tables)
  → Graph database (knowledge graph)
```

---

## 🔄 Full Flow in Bedrock

```
SETUP (once):
  Your docs in S3
       ↓
  Bedrock chunks them
       ↓
  Embedding model (Titan/Cohere) converts chunks to vectors
       ↓
  Stored in vector store (OpenSearch by default)
       ↓
  Knowledge Base ready ✅


RUNTIME (every query):
  User: "What's the refund policy?"
       ↓
  Semantic search in OpenSearch
  (convert query to vector → find closest chunks)
       ↓
  Top results = context
       ↓
  Augmented prompt = original question + context
       ↓
  Foundation model generates answer
       ↓
  Response to user ✅
```

---

## 🔌 Three Ways to Use Knowledge Bases

```
1. Console playground ("Chat with your document")
   → Quick and dirty, for testing
   → Upload doc, pick model, start chatting

2. Bedrock API (in your application)
   → retrieve: just search the knowledge base
   → retrieve_and_generate: search + generate answer
   → Build into your own app

3. Inside an Agent (Agentic RAG)
   → Knowledge base as one tool among many
   → Agent can search docs AND take actions
   → Most powerful option
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│     BEDROCK KNOWLEDGE BASES — QUICK RECALL                   │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  Knowledge Base = RAG in Bedrock                             │
│                                                              │
│  Data sources: S3, web crawler, Confluence,                  │
│                Salesforce, SharePoint                        │
│                                                              │
│  Embedding models: Titan Embeddings, Cohere                  │
│  You control: vector dimensions                              │
│                                                              │
│  Vector store default: OpenSearch                            │
│  Other options: MemoryDB, Aurora, MongoDB, Pinecone, Redis   │
│                                                              │
│  Chunking: default 300 chars, configurable size + overlap    │
│  Better: chunk by sentences/paragraphs for quality           │
│                                                              │
│  Three ways to use:                                          │
│  → Console playground (testing)                              │
│  → Bedrock API (your app)                                    │
│  → Inside an Agent (agentic RAG)                             │
│                                                              │
│  EXAM TIPS:                                                  │
│  "Default vector store in Bedrock?"  → OpenSearch            │
│  "Data source for Knowledge Base?"   → S3 (most common)     │
│  "Embedding model in Bedrock?"       → Titan or Cohere      │
│  "Poor RAG results?"                 → Check chunk quality   │
│  "Use KB in an app?"                 → Bedrock API           │
│  "KB + actions?"                     → Agent (Agentic RAG)   │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
