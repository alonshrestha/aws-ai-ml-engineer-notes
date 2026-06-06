# SageMaker Random Cut Forest (RCF) — Beginner Friendly Guide

## 🧠 What Is Random Cut Forest?

It finds **weird stuff** in your data. That's it.

```
Normal data: 10, 12, 11, 13, 10, 12, 11, 500, 10, 13
                                          ↑
                                     "That's weird!"
                                     anomaly score: HIGH
```

This is called **anomaly detection** — finding data points that don't fit the pattern.

It's **unsupervised** — meaning you don't need to label your data as "normal" or "anomaly" beforehand. It figures it out on its own.

---

## 🏆 AWS Is Really Proud of This One

Amazon developed this algorithm themselves and wrote a research paper on it. They keep putting it everywhere:

```
Where Random Cut Forest shows up:
  → SageMaker (as a built-in algorithm)
  → Kinesis Analytics (for streaming data)
  → QuickSight (for visualization)

AWS loves it. Expect it on the exam.
```

---

## 🤔 What Kind of Anomalies Can It Find?

```
1. Sudden spikes or drops
   Normal: 10, 11, 12, 10, 11
   Anomaly: 10, 11, 12, 500, 11
                        ↑ spike!

2. Breaks in patterns (periodicity)
   Normal: up, down, up, down, up, down
   Anomaly: up, down, up, UP, UP, UP
                         ↑ pattern broke!

3. Unclassifiable data points
   → Things that just don't fit anywhere
```

For every data point, it gives an **anomaly score**:
```
Low score  = normal, nothing weird
High score = anomaly, something's off
```

---

## ⚙️ How Does It Work?

Think of it like this:

```
Step 1: Build a "forest" of decision trees from your data
        (each tree is built from a random sample — that's the "random cut" part)

Step 2: Try adding a new data point to the trees

Step 3: See how much the tree has to change to fit this new point

If the tree barely changes → normal data point
If the tree needs LOTS of new branches → anomaly!
```

Simple analogy:

```
Imagine organizing books on a shelf by category.

Normal book: "Oh, this goes right here in the Science section" 
             → easy, no rearranging needed

Weird book: "This is a cookbook about quantum physics written as poetry?!"
            → you'd need to create whole new sections
            → ANOMALY!
```

---

## 📥 Input Format

```
✅ CSV
✅ RecordIO-Protobuf
✅ File mode or Pipe mode (both work)

Optional: Test channel
  → Provide labeled data to measure accuracy/precision/recall/F1
  → But training itself is unsupervised (no labels needed)
```

---

## 🎛️ Key Hyperparameters

| Parameter | What It Does |
|-----------|-------------|
| `num_trees` | How many trees in the forest. More trees = less noise |
| `num_samples_per_tree` | How much data each tree sees |

### Tuning num_samples_per_tree:

```
Rule of thumb:

1 / num_samples_per_tree ≈ ratio of anomalies in your data

Example:
  You think ~1% of your data is anomalous
  1 / num_samples_per_tree ≈ 0.01
  num_samples_per_tree ≈ 100

  You think ~5% of your data is anomalous
  1 / num_samples_per_tree ≈ 0.05
  num_samples_per_tree ≈ 20
```

---

## 📡 Works on Streaming Data Too!

Not just batch data — RCF is available in **Kinesis Analytics** for real-time anomaly detection:

```
Live data stream → Kinesis Analytics (with RCF) → "ANOMALY DETECTED!"

Example:
  Website traffic streaming in...
  100 req/s, 110 req/s, 95 req/s, 50000 req/s ← ALERT!
  
  Could be: DDoS attack, bot traffic, or viral content
```

---

## 💻 What Machines to Use?

```
❌ No GPU — simple algorithm, doesn't need it
✅ CPU only

Training:   M4, C4, or C5
Inference:  C5.xlarge (recommended)
```

This is one of the simpler algorithms — no deep learning, no heavy math. Just trees.

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────┐
│          RANDOM CUT FOREST — QUICK RECALL                │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  What is it?    Anomaly detection                        │
│                 Finds weird/unusual data points           │
│                                                          │
│  Supervised?    NO — unsupervised                        │
│                 (optional test channel for measuring)     │
│                                                          │
│  Made by?       Amazon (their own algorithm)             │
│                 Shows up everywhere: SageMaker,           │
│                 Kinesis Analytics, QuickSight             │
│                                                          │
│  How?           Forest of trees — if new data needs      │
│                 lots of new branches = anomaly            │
│                                                          │
│  Input?         CSV or RecordIO-Protobuf                 │
│                 File or Pipe mode                         │
│                                                          │
│  Key params:    num_trees (more = less noise)            │
│                 num_samples_per_tree                      │
│                 (1/samples ≈ anomaly ratio)               │
│                                                          │
│  Instances:     CPU only ❌ no GPU                        │
│                 Training: M4, C4, C5                     │
│                 Inference: C5.xlarge                      │
│                                                          │
│  Streaming?     Yes! Available in Kinesis Analytics      │
│                                                          │
│  #1 exam tip:   Anomaly detection → Random Cut Forest    │
│                                                          │
└──────────────────────────────────────────────────────────┘
```
