# SageMaker Object2Vec — Beginner Friendly Guide

## 🧠 What Is Object2Vec?

Remember Word2Vec from BlazingText? It finds similar **words**.

Object2Vec is the same idea but for **anything** — words, sentences, documents, users, products, movies... any "object."

```
Word2Vec:    finds similar WORDS only
Object2Vec:  finds similar ANYTHING
```

It creates a map (embedding) where similar objects are close together:

```
Imagine a map of movies:

  Avengers ●
  Iron Man ●        ● Titanic
  Thor ●            ● Notebook
                    ● Romeo & Juliet

Action movies cluster together.
Romance movies cluster together.
Similar things = close on the map.
```

---

## 🤔 What Does "Embedding" Mean?

Every object has lots of features (high dimensional). Object2Vec squishes all that into a small set of numbers (low dimensional) that captures the essence.

```
A movie has many features:
  genre, actors, length, rating, year, budget, language...
  (high dimensional — lots of info)

Object2Vec turns that into something like:
  [0.8, -0.3, 0.5]
  (low dimensional — just 3 numbers, but captures the meaning)

Now you can compare movies by comparing these small number lists.
Close numbers = similar movies.
```

---

## ✅ What Can It Do?

```
1. Find nearest neighbors
   → "What movies are most similar to Avengers?"

2. Clustering
   → Group similar items together automatically

3. Genre prediction
   → "This movie is probably Action based on its embedding"

4. Recommendation systems
   → "Users who liked X also liked Y"
   → Similar users → recommend what they liked
   → Similar items → recommend related products
```

---

## 🔗 It Works With PAIRS

This is the unique part — Object2Vec learns from **pairs of things**:

```
Pair examples:
  (sentence A, sentence B)        → are these similar?
  (genre, movie description)      → does this genre match?
  (user, product)                 → does this user like this product?
  (customer A, customer B)        → are these customers similar?
  (product A, product B)          → are these products related?
```

It looks at pairs and learns: "these two things go together" or "these two things don't."

---

## ⚙️ How Does It Work Inside?

Two parallel paths — one for each item in the pair:

```
Input Pair: (User, Product)

Path 1: User data    → Encoder 1 → vector for user
Path 2: Product data → Encoder 2 → vector for product
                            ↓
                       Comparator
                     (are they similar?)
                            ↓
                   Feedforward Neural Network
                            ↓
                     Final prediction
```

### Each Encoder Can Be:

```
1. Average Pooled Embeddings → simple, fast
2. CNN                       → good for patterns
3. Bidirectional LSTM        → good for sequences

You pick what works best for your data.
Each path can use a DIFFERENT encoder.
```

You don't need to deeply understand this for the exam — just know it uses two encoders feeding into a comparator.

---

## 📥 Input Format

```
- Data must be TOKENIZED into integers first
  (just like Word2Vec — computers need numbers)

- Input comes in PAIRS:
  (token sequence 1, token sequence 2)

❌ Can't throw in raw images or text
✅ Must convert everything to integer tokens first
```

---

## 🎛️ Key Hyperparameters

Standard deep learning settings:

| Parameter | What It Does |
|-----------|-------------|
| `epochs` | How many passes through training data |
| `learning_rate` | How fast it learns |
| `batch_size` | Examples per training step |
| `num_layers` | Depth of the network |
| `activation_function` | How neurons fire |
| `optimizer` | How the model improves (Adam, SGD, etc.) |
| `weight_decay` | L2 regularization (prevent overfitting) |
| `dropout` | Randomly turns off neurons during training (prevent overfitting) |
| `early_stopping` | Stop training when it stops improving |
| `enc1_network` | Encoder type for input 1 (CNN, LSTM, or pooled) |
| `enc2_network` | Encoder type for input 2 (CNN, LSTM, or pooled) |

---

## 💻 What Machines to Use?

### Training:

```
❌ Single machine only — cannot distribute across machines
✅ CPU or GPU
✅ Multi-GPU on one machine is okay

Start with:
  CPU: ml.m5.2xlarge
  GPU: ml.p2.xlarge

Need more power?
  CPU: ml.m5.4xlarge or ml.m5.12xlarge
  GPU: P2, P3, G4, G5 instances
```

### Inference:

```
✅ CPU and GPU supported
Recommended: ml.p3.2xlarge

Tip: Set inference_preferred_mode environment variable
     to optimize for embeddings (instead of classification/regression)
     if you're using it as an embedding layer.
```

---

## 🆚 Word2Vec vs Object2Vec

| | Word2Vec (BlazingText) | Object2Vec |
|--|----------------------|------------|
| Works on | Individual words only | Any object (words, sentences, docs, users, products) |
| Input | Single words | Pairs of things |
| Output | Word embeddings | Object embeddings |
| Use case | Find similar words | Find similar anything |
| Architecture | Simple | Two encoders + comparator + neural network |

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────┐
│             OBJECT2VEC — QUICK RECALL                    │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  What is it?    Embedding layer for ANY objects          │
│                 (generalized Word2Vec)                    │
│                                                          │
│  Does what?     Nearest neighbors, clustering,           │
│                 genre prediction, recommendations         │
│                                                          │
│  Key concept:   Works with PAIRS of objects              │
│                 (user-product, sentence-sentence, etc.)   │
│                                                          │
│  Architecture:  2 encoders → comparator → neural network │
│                 Encoders: CNN, LSTM, or pooled embedding  │
│                                                          │
│  Input?         Tokenized integers in pairs              │
│                                                          │
│  Instances:     Single machine only ❌ no multi-machine  │
│                 CPU or GPU ✅                             │
│                 Multi-GPU on one machine ✅               │
│                 Start: m5.2xlarge (CPU) or p2.xlarge(GPU)│
│                                                          │
│  vs Word2Vec:   Word2Vec = words only                    │
│                 Object2Vec = anything                     │
│                                                          │
│  Inference:     p3.2xlarge recommended                   │
│                 Set inference_preferred_mode for          │
│                 embeddings optimization                   │
│                                                          │
└──────────────────────────────────────────────────────────┘
```
