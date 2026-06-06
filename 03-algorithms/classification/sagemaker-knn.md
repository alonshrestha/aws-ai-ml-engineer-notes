# SageMaker KNN (K-Nearest Neighbors) — Beginner Friendly Guide

## 🧠 What Is KNN?

Probably the simplest ML algorithm ever. When you get a new data point, just look at the **K closest things** to it and copy their answer.

```
You just moved to a new neighborhood.
You want to know: "Is this area expensive or cheap?"

You ask your 3 nearest neighbors (K=3):
  Neighbor 1: "Expensive"
  Neighbor 2: "Expensive" 
  Neighbor 3: "Cheap"

Majority says: "Expensive" → that's your answer!
```

That's KNN. Look at the closest K examples. Go with the majority.

---

## 🔍 How Does It Actually Work?

```
Your data plotted on a map:

  🔴 🔴
  🔴    🔴        🔵 🔵
     🔴          🔵   🔵
                🔵
         ⭐ ← new point (what is it? 🔴 or 🔵?)

K = 3: look at 3 nearest points

  Nearest 1: 🔴
  Nearest 2: 🔴
  Nearest 3: 🔵

  2 red vs 1 blue → Answer: 🔴
```

It literally just counts which label appears most among the nearest neighbors. No training, no complex math.

---

## ✅ It Does Both Classification AND Regression

```
CLASSIFICATION (most common label):
  K=3 neighbors: cat, cat, dog
  Answer: cat (majority wins)

REGRESSION (average of values):
  K=3 neighbors: $200k, $250k, $220k
  Answer: $223k (average)
```

Same idea — just return the majority label OR the average value depending on what you need.

---

## ⚠️ KNN vs K-Means — Don't Mix These Up!

```
KNN (K-Nearest Neighbors):
  → SUPERVISED (needs labeled data)
  → "What label do the closest examples have?"
  → Used for: classification, regression

K-Means:
  → UNSUPERVISED (no labels)
  → "Group similar things together"
  → Used for: clustering
```

```
KNN:     "My 3 nearest neighbors are cats, so I'm a cat"
K-Means: "These things look similar, I'll put them in a group"
```

---

## 📥 Input Format

```
✅ RecordIO-Protobuf or CSV
   (CSV: first column = label, rest = features)
✅ File or Pipe mode

Training channel: your data (required)
Test channel: optional — to measure accuracy or MSE
```

---

## 🚀 What SageMaker Adds on Top

Regular KNN has problems with big data — checking distance to EVERY point is slow. SageMaker fixes this:

```
Step 1: SAMPLE the data
  → Too much data? Take a smart sample
  → Don't need to check every single point

Step 2: DIMENSIONALITY REDUCTION
  → Too many features? Reduce them
  → 1000 features → maybe 50 features
  → Makes distance calculations much faster

  ⚠️ Trade-off: some accuracy lost due to noise

Step 3: BUILD AN INDEX
  → Creates a lookup structure for fast neighbor search
  → Like a phone book — find things quickly instead of 
    scanning everything

Step 4: SERIALIZE the model
  → Save it, then query it with any K value
```

---

## 🎛️ Key Hyperparameters

| Parameter | What It Does |
|-----------|-------------|
| `k` | **THE main one** — how many neighbors to look at |
| `sample_size` | How much data to sample (for large datasets) |

### Choosing K:

```
K too small (K=1):
  → Looks at only 1 neighbor
  → Very sensitive to noise
  → One weird data point ruins everything

K too large (K=1000):
  → Looks at too many neighbors
  → Includes points that are far away
  → Predictions become too generic

Sweet spot: experiment and find where more K stops helping
```

---

## 💻 What Machines to Use?

### Training:

```
✅ CPU: ml.m5.2xlarge (recommended)
✅ GPU: ml.p2.xlarge (recommended)
```

### Inference:

```
✅ CPU → lower latency (faster per request)
         Good for: one prediction at a time

✅ GPU → higher throughput (more predictions at once)
         Good for: large batches of predictions
```

```
Need fast single predictions?  → CPU
Need lots of predictions at once? → GPU
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────┐
│                KNN — QUICK RECALL                        │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  What is it?    Simplest ML algorithm                    │
│                 Look at K nearest points, copy answer     │
│                                                          │
│  Supervised?    YES (needs labeled data)                 │
│                                                          │
│  Does what?     Classification (majority label)          │
│                 Regression (average value)                │
│                                                          │
│  vs K-Means:    KNN = supervised, classifies             │
│                 K-Means = unsupervised, clusters          │
│                                                          │
│  SageMaker      Samples data (handles big datasets)      │
│  extras:        Dimensionality reduction (fewer features) │
│                 Fast index for neighbor lookup             │
│                                                          │
│  Input?         RecordIO-Protobuf or CSV                 │
│                 File or Pipe mode                         │
│                                                          │
│  Key param:     k (how many neighbors)                   │
│                 sample_size                               │
│                                                          │
│  Instances:     CPU or GPU for training                  │
│                 Inference: CPU = low latency              │
│                            GPU = high throughput          │
│                                                          │
└──────────────────────────────────────────────────────────┘
```
