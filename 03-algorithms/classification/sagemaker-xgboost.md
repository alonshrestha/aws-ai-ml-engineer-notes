# SageMaker XGBoost — Beginner Friendly Guide

## 🧠 What Is XGBoost?

XGBoost = **Extreme Gradient Boosting**

Imagine you take a test and get some answers wrong. Then your friend takes the SAME test but ONLY focuses on the questions you got wrong. Then another friend does the same for whatever's still wrong. Each person fixes the mistakes of the previous one.

That's XGBoost — a team of decision trees where each new tree fixes the errors of the previous ones.

```
Tree 1: Makes predictions → gets some wrong
            ↓
Tree 2: Focuses on Tree 1's mistakes → still some wrong
            ↓
Tree 3: Focuses on Tree 2's mistakes → even fewer wrong
            ↓
... keeps going ...
            ↓
Final: Combine all trees → really good predictions! 🎯
```

---

## 🌳 Wait, What's a Decision Tree?

A decision tree is like a flowchart of yes/no questions:

```
Is it raining?
├── Yes → Take umbrella ☂️
└── No
    ├── Is it sunny? 
    │   ├── Yes → Wear sunglasses 😎
    │   └── No → Wear jacket 🧥
```

XGBoost builds MANY of these trees, each one learning from the mistakes of the last. That's the "boosting" part.

---

## 🔥 Why Is XGBoost So Popular?

- Wins tons of **Kaggle competitions** (data science contests)
- **Fast** — doesn't cost a fortune to run
- Works for both **classification AND regression**

```
CLASSIFICATION (categories):
  "Is this email spam?" → Yes/No

REGRESSION (numbers):
  "What will the house price be?" → $350,000
  (uses something called "regression trees" for this)
```

---

## 📦 Where Does It Come From?

This is a bit unique — XGBoost is NOT originally built by AWS.

```
Open Source XGBoost (everyone uses this)
        ↓
AWS took it and added it into SageMaker
        ↓
Over time, AWS keeps adding more features on top
```

### Two Ways to Use It in SageMaker:

```
Option 1: Use directly in your SageMaker notebook
  → Simple, runs on your notebook machine
  → Good for small experiments

Option 2: Deploy as built-in SageMaker algorithm
  → Uses XGBoost Docker image from ECR
  → Can spread training across multiple machines
  → Good for large-scale training
```

Models are saved/loaded using Python's **pickle** (a way to save Python objects to files).

---

## 📥 Input Formats

| Format | Supported? |
|--------|-----------|
| CSV | ✅ (original) |
| LibSVM | ✅ (original) |
| RecordIO-Protobuf | ✅ (added later) |
| Parquet | ✅ (added later) |

**Note for distributed GPU training:** only CSV or Parquet work.

---

## 🎛️ Key Hyperparameters (The Main Battle!)

XGBoost has A LOT of settings to tune. This is where most of the work goes.

### Preventing Overfitting (Exam Loves This!)

| Parameter | What It Does | Simple Analogy |
|-----------|-------------|----------------|
| `subsample` | Uses only a portion of data for each tree | Don't study the ENTIRE book — sample chapters |
| `eta` | Step size shrinkage — how much each tree contributes | Taking baby steps instead of giant leaps |
| `max_depth` | How deep each tree can go (default: 6) | Limiting how many follow-up questions you ask |
| `gamma` | Minimum improvement needed to add a new branch | "Only split if it's REALLY worth it" |
| `alpha` | L1 regularization (like Linear Learner) | Removes unimportant features |
| `lambda` | L2 regularization (like Linear Learner) | Quiets down less important features |

```
⚠️ Overfitting warning:

max_depth too high = tree gets too complicated = memorizes data = OVERFITTING

Think of it like this:
  Shallow tree (depth 3): "Is it an animal? Is it big? Does it fly?" → Eagle
  Deep tree (depth 20): Asks SO many specific questions it only 
                        recognizes the EXACT animals it trained on
```

**For both alpha and lambda:** larger values = more conservative (safer) model.

### Other Important Parameters

| Parameter | What It Does | When to Use |
|-----------|-------------|-------------|
| `eval_metric` | What you're optimizing for | `error`/`RMSE` for accuracy, `AUC` if you care about false positives |
| `scale_pos_weight` | Balances positive vs negative cases | When your data is unbalanced |

### Understanding eval_metric:

```
Example: Cancer detection model

Most patients: Healthy (negative) — 95%
Few patients: Have cancer (positive) — 5%

If you just care about accuracy → eval_metric = "error"
If you care about catching ALL cancers (even with some false alarms) → eval_metric = "AUC"
```

### Understanding scale_pos_weight:

```
Your data:
  Spam emails: 100        (positive)
  Normal emails: 10,000   (negative)

Problem: Model just says "not spam" for everything 
         and is 99% accurate! But useless.

Fix: scale_pos_weight = 10,000 / 100 = 100
     Now the model treats each spam example as 
     100x more important
```

---

## 💻 What Machines to Use?

### CPU Training:
```
XGBoost on CPU = MEMORY hungry (not compute hungry)

Best choice: M5 instances (optimized for memory)

Think of it like:
  XGBoost needs a big desk to spread out papers (memory)
  NOT a faster brain to think (compute)
```

### GPU Training (Faster!):

This evolved over time:

```
Version 1.2 (2020):
  ✅ Single GPU training
  → Use P2, P3 instances
  → Set: tree_method = "gpu_hist"

Version 1.2-2 (later):
  ✅ Added G4 and G5 instance types

Version 1.5 (May 2023):
  ✅ Distributed GPU training (multiple machines with GPUs!)
  → Use cluster of G5s for example
  → Set: tree_method = "gpu_hist"
  → Set: use_dask_gpu_training = true
  → Set: distribution = "fully_replicated"
  → ⚠️ Only works with CSV or Parquet input
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────┐
│                 XGBOOST — QUICK RECALL                   │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  What is it?    Boosted decision trees                   │
│                 (each tree fixes previous tree's errors)  │
│                                                          │
│  Does what?     Classification + Regression              │
│                                                          │
│  Origin?        Open source (not AWS-built)              │
│                                                          │
│  Input?         CSV, LibSVM, RecordIO-Protobuf, Parquet │
│                                                          │
│  Overfitting?   Tune: subsample, eta, max_depth,        │
│                 gamma, alpha, lambda                     │
│                                                          │
│  max_depth       Too high = overfitting ⚠️               │
│                                                          │
│  Unbalanced      Use scale_pos_weight                    │
│  data?           (= negative count / positive count)     │
│                                                          │
│  eval_metric     accuracy → error/RMSE                   │
│                  false positives matter → AUC            │
│                                                          │
│  CPU instances?  M5 (memory bound, not compute bound)   │
│                                                          │
│  GPU training?   tree_method = "gpu_hist"                │
│                  Distributed: + use_dask_gpu_training    │
│                  + distribution = "fully_replicated"     │
│                  Distributed GPU: CSV or Parquet only    │
│                                                          │
└──────────────────────────────────────────────────────────┘
```
