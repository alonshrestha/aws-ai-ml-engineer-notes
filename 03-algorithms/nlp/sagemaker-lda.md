# SageMaker LDA (Latent Dirichlet Allocation) — Beginner Friendly Guide

## 🧠 What Is LDA?

Same goal as Neural Topic Model — figure out what documents are about by grouping them into topics. But uses a completely different approach under the hood (no neural network).

```
Neural Topic Model: topic modeling USING a neural network
LDA:                topic modeling WITHOUT a neural network
```

Both are unsupervised. Both produce unlabeled topics. Both need you to interpret what the topics mean.

---

## 🤔 How Is It Different From Neural Topic Model?

| | Neural Topic Model | LDA |
|--|-------------------|-----|
| Goal | Find topics in documents | Find topics in documents |
| Supervised? | No (unsupervised) | No (unsupervised) |
| Under the hood | Neural network | Statistical model (no neural net) |
| GPU? | Yes (recommended) | No — CPU only |
| Cost | More expensive (GPU) | Cheaper (CPU) |
| Multi-machine? | Not specified | ❌ Single instance only |

Same job, different engine. Try both, see which gives better results for your data.

---

## 🔧 It's Not Just for Documents!

Even though topic modeling is the main use case, LDA is actually more general purpose:

```
Documents:
  → Group articles by topic (the classic use case)

Customers:
  → Group customers by purchase patterns
  → "These customers buy similar things"

Music:
  → Harmonic analysis
  → Group songs by musical patterns

Anything where you want to cluster things 
based on what they have in common.
```

---

## 📥 Input Format

```
✅ RecordIO-Protobuf or CSV
✅ File mode — works with both formats
✅ Pipe mode — ONLY works with RecordIO-Protobuf

Must tokenize first (same as NTM):
  → Each document = word counts (not raw text)
  → Pass in integers representing words

Optional: test channel to measure accuracy
```

### How Accuracy Is Measured:

```
LDA uses: per-word log-likelihood

Don't need to understand the math.
Just know that's the metric it uses to score results.
```

---

## 🎛️ Key Hyperparameters

| Parameter | What It Does |
|-----------|-------------|
| `num_topics` | **THE main one** — how many topics to create (same as NTM) |
| `alpha0` | Initial concentration parameter |

### About alpha0:

```
Small alpha0 → sparse topic mixtures
  → Each document belongs to just 1-2 topics
  → "This article is ONLY about sports"

Large alpha0 → uniform mixtures
  → Each document is a mix of many topics
  → "This article is about sports AND health AND business"
```

That's it for hyperparameters. No batch_size, no learning_rate, no optimizer — because there's no neural network. Much simpler to tune than NTM.

---

## 💻 What Machines to Use?

```
✅ CPU only — no GPU needed or supported
❌ Single instance only — cannot distribute

That's it. One CPU machine. Simple and cheap.
```

---

## 🆚 When to Use NTM vs LDA?

```
Want to try the cheaper option first?     → LDA (CPU only)
Have GPU budget and want neural approach?  → NTM
Not sure?                                  → Try both, compare results

Both give you the same kind of output:
  → Documents grouped into topics
  → You interpret what topics mean
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────┐
│               LDA — QUICK RECALL                         │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  What is it?    Topic modeling (same goal as NTM)        │
│                 Groups documents into topics              │
│                                                          │
│  Supervised?    NO — unsupervised                        │
│                                                          │
│  vs NTM:        NTM = neural network based (GPU)         │
│                 LDA = statistical model (CPU only)        │
│                                                          │
│  Not just docs: Also works for customer clustering,      │
│                 music analysis, anything with patterns    │
│                                                          │
│  Input?         RecordIO-Protobuf or CSV                 │
│                 Tokenized word counts                     │
│                 Pipe mode: RecordIO only                  │
│                                                          │
│  Metric:        Per-word log-likelihood                  │
│                                                          │
│  Key params:    num_topics (main one)                    │
│                 alpha0 (sparse vs uniform topics)         │
│                                                          │
│  Instances:     CPU only ❌ no GPU                        │
│                 Single instance only ❌ no multi-machine  │
│                                                          │
└──────────────────────────────────────────────────────────┘
```
