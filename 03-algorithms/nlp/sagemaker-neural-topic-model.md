# SageMaker Neural Topic Model (NTM) — Beginner Friendly Guide

## 🧠 What Is Topic Modeling?

You throw a bunch of documents at it and it figures out **what topics they're about**. No labels needed — it's unsupervised.

```
Input: 1000 random articles

Output:
  Topic 1: bike, car, train, speed, mileage     → (probably transportation)
  Topic 2: goal, score, team, match, player      → (probably sports)
  Topic 3: stock, price, market, trade, profit   → (probably finance)
```

**Important:** The model does NOT name the topics. It just groups related words together. YOU have to look at the words and figure out "oh, that's transportation."

---

## 🤔 How Is This Different From TF-IDF?

```
TF-IDF:
  → Searches for specific WORDS
  → "Find documents that mention 'car'"

Topic Modeling:
  → Groups documents by CONCEPTS
  → "Find documents about transportation"
    (even if they never use the word 'transportation')
    (catches: bike, train, speed, mileage too)
```

Topic modeling works at a higher level — it understands that "bike" and "car" are related even though they're different words.

---

## ✅ What Can It Do?

```
1. Document classification
   → "This article belongs to Topic 3 (finance)"

2. Document summarization
   → "This document is mainly about Topic 1 and Topic 5"

3. Content organization
   → Automatically sort thousands of documents into groups
```

---

## 🔍 How Does It Work?

```
Step 1: You give it a bunch of documents (tokenized into numbers)

Step 2: It looks at which words appear together across documents

Step 3: It groups frequently co-occurring words into "topics"

Step 4: Each document gets assigned to one or more topics

Example:
  Document A mentions: car, speed, highway, fuel
  Document B mentions: bike, mileage, road, tire
  Document C mentions: stock, profit, trade, market

  Model says:
    Topic 1 (words: car, bike, speed, mileage, road) → Docs A & B
    Topic 2 (words: stock, profit, trade, market)     → Doc C
```

The underlying algorithm is called **Neural Variational Inference** (just know the name, don't need to understand it).

---

## 📥 Input Format

```
✅ RecordIO-Protobuf or CSV
✅ File or Pipe mode (pipe is faster)

What you need to provide:
  1. Documents → tokenized into integers (not raw text!)
     Each document = a count of every word in the vocabulary
  
  2. Vocabulary file → maps words to numbers
     Passed through an "auxiliary channel"
     
  3. Optional: test/validation channel
     → To measure performance on known topics

❌ Cannot pass in raw text files
✅ Must tokenize words into numbers first
```

```
Example:
  Vocabulary:  {"car": 1, "bike": 2, "stock": 3, "speed": 4}
  
  Document A: "car speed car speed speed"
  Tokenized:  [1, 4, 1, 4, 4]
  Word counts: {car: 2, speed: 3}
```

---

## 🎛️ Key Hyperparameters

| Parameter | What It Does |
|-----------|-------------|
| `num_topics` | **THE main one** — how many topics to create |
| `batch_size` | Examples per training step |
| `learning_rate` | How fast it learns |

### About num_topics:

```
num_topics = 5:
  → Very high-level grouping
  → Broad topics like "science", "sports", "politics"

num_topics = 50:
  → More specific grouping
  → Narrower topics like "basketball", "football", "tennis"

num_topics = 500:
  → Very granular
  → Might be too specific to be useful

You have to experiment to find the right number.
```

The topics are a **latent representation** — meaning hidden patterns the model discovers based on the top-ranking words in your documents.

---

## 💻 What Machines to Use?

```
Training:
  ✅ GPU recommended (it's a neural network)
  ✅ CPU also works

Inference:
  ✅ CPU is probably enough (cheaper)
```

---

## 📝 One of TWO Topic Modeling Algorithms

SageMaker has two topic modeling options:

```
1. Neural Topic Model (NTM) ← this one
   → Uses neural network (neural variational inference)

2. LDA (Latent Dirichlet Allocation) ← covered separately
   → Different approach, same goal

Try both and see which works better for your data.
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────┐
│         NEURAL TOPIC MODEL — QUICK RECALL                │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  What is it?    Topic modeling — figure out what          │
│                 documents are about                       │
│                                                          │
│  Supervised?    NO — unsupervised                        │
│                 Topics are NOT human-readable names       │
│                 YOU interpret what the topics mean        │
│                                                          │
│  vs TF-IDF:     TF-IDF = search by words                │
│                 NTM = group by concepts                   │
│                                                          │
│  Input?         RecordIO-Protobuf or CSV                 │
│                 Tokenized integers + vocabulary file      │
│                 File or Pipe mode                         │
│                                                          │
│  Key param:     num_topics (how many topics to create)   │
│                 + batch_size, learning_rate               │
│                                                          │
│  Instances:     GPU recommended for training             │
│                 CPU fine for inference                    │
│                                                          │
│  Related:       LDA is the other topic modeling option   │
│                 Try both, see which works better          │
│                                                          │
└──────────────────────────────────────────────────────────┘
```
