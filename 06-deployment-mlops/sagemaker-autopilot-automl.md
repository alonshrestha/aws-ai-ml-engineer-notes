# SageMaker Autopilot (AutoML)

## 🤖 What Is Autopilot?

Autopilot = "I'll do the whole ML thing for you."

You give it data and tell it what to predict. It figures out the best algorithm, tunes the hyperparameters, and gives you a ready-to-deploy model. That's it.

```
You:       "Here's my data in S3. Predict the 'price' column."
Autopilot: "Got it. Let me try different algorithms, 
            tune them, and find the best one."

            ...working...

Autopilot: "Done. Here's your best model. 
            Want to deploy it?"
```

---

## 📋 The Workflow

```
Step 1: Load data from S3 (CSV or Parquet)
           ↓
Step 2: Select target column ("what do I want to predict?")
           ↓
Step 3: Autopilot runs automatically
        → Tries different algorithms
        → Tunes hyperparameters
        → Finds the best model
           ↓
Step 4: Get a model notebook (see what it did)
           ↓
Step 5: See model leaderboard (ranked list of best models)
           ↓
Step 6: Pick a model → Deploy → Monitor
```

### Not a black box:

```
Autopilot gives you:
  → Model notebook: see exactly what it did, tweak if you want
  → Leaderboard: ranked list of models, pick any one
  → Clarify integration: check for bias

You CAN just push a button and deploy.
But you SHOULD review what it built.
```

---

## 🎯 What Problems Can It Solve?

```
1. Binary Classification    → "Fraud or not fraud?" (yes/no)
2. Multi-Class Classification → "Which category?" (cat/dog/fish)
3. Regression               → "What number?" (house price)
```

### Input data:

```
CSV     ✅
Parquet ✅
That's it. Must be tabular data (rows and columns).
```

---

## ⚙️ Three Training Modes

### 1. HPO Mode (Hyperparameter Optimization)

```
Algorithms available:
  → Linear Learner
  → XGBoost
  → Deep Learning (MLP)

What it does:
  → Picks the best algorithm for your data
  → Runs up to 100 trials to find best hyperparameters

How it tunes depends on data size:
  → Data < 100MB  → Bayesian optimization
  → Data > 100MB  → Multi-fidelity optimization
                     (auto-stops bad trials early)
```

### 2. Ensembling Mode

```
Algorithms available:
  → Everything in HPO mode PLUS
  → Random Forest and other tree methods
  → PyTorch neural networks
  → Uses AutoGluon library

What it does:
  → Runs 10 trials with different models
  → Combines them into a stacked ensemble
  → Multiple models vote on the answer (like bagging!)

Wider selection of algorithms than HPO mode.
```

### 3. Auto Mode (Most Common)

```
Autopilot decides for you:

  Data > 100MB  → uses HPO mode
  Data < 100MB  → uses Ensembling mode

Simple. But there are edge cases where it can't decide:
```

### ⚠️ When Auto Mode Defaults to HPO:

```
Can't determine data size? → defaults to HPO

This happens when:
  → S3 bucket is inside a VPC (can't peek at size)
  → S3 data type is set to "manifest file"
  → S3 URI contains more than 1,000 items
```

---

## 🔍 Autopilot + Clarify (Bias Detection)

This is the important part. Autopilot makes it easy to build models, but easy = dangerous if you don't check for bias.

```
Danger of AutoML:

  You push a button → model is created → you deploy it
  
  But what if your training data was biased?
  → Loan approval model that discriminates by race
  → Hiring model that discriminates by gender
  → You'd never know without checking!
```

Autopilot integrates with Clarify to catch this:

```
Clarify shows you:
  → Which features contributed most to predictions
  → Uses SHAP (Shapley values) to calculate feature importance
  → Flags potential bias

Example:
  Loan approval model
  Clarify says: "Race has 40% feature importance"
  
  You: "That's wrong. Race shouldn't matter for loans."
  Action: Go back, fix training data, retrain.
```

### How Clarify works under the hood:

```
SHAP (Shapley Additive Explanations):
  → From cooperative game theory
  → Assigns importance value to each feature
  → For each prediction, shows which features mattered most

You already learned this in your Clarify notes!
```

---

## 🧭 Where Autopilot Fits in the Pipeline

```
Without Autopilot (manual):
  Phase 2: YOU clean data
  Phase 3: YOU pick algorithm
  Phase 4: YOU tune hyperparameters
  Phase 5: YOU evaluate

With Autopilot:
  Phase 2-5: Autopilot does it all
  You: Review notebook, check leaderboard, 
       verify with Clarify, deploy
```

It's a shortcut through Phases 2-5. But you should still review what it did — especially for bias.

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│          SAGEMAKER AUTOPILOT — QUICK RECALL                  │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  What is it?                                                 │
│  → AutoML in SageMaker                                       │
│  → Automatically picks algorithm + tunes hyperparameters     │
│  → Input: CSV or Parquet from S3                             │
│                                                              │
│  Problem types: Binary, Multi-class, Regression              │
│                                                              │
│  ─────────────────────────────────────────────────────────── │
│                                                              │
│  TRAINING MODES:                                             │
│                                                              │
│  HPO mode:                                                   │
│  → Linear Learner, XGBoost, Deep Learning                    │
│  → Up to 100 trials                                          │
│  → <100MB data: Bayesian optimization                        │
│  → >100MB data: Multi-fidelity (auto early stopping)         │
│                                                              │
│  Ensembling mode:                                            │
│  → More algorithms (+ Random Forest, PyTorch)                │
│  → Uses AutoGluon library                                    │
│  → 10 trials, combines into stacked ensemble                 │
│                                                              │
│  Auto mode:                                                  │
│  → >100MB → HPO                                              │
│  → <100MB → Ensembling                                       │
│  → Can't determine size? → defaults to HPO                   │
│    (VPC, manifest file, >1000 S3 items)                      │
│                                                              │
│  ─────────────────────────────────────────────────────────── │
│                                                              │
│  NOT A BLACK BOX:                                            │
│  → Gives you a model notebook (see what it did)              │
│  → Model leaderboard (ranked list of models)                 │
│  → Clarify integration (bias detection with SHAP)            │
│                                                              │
│  ─────────────────────────────────────────────────────────── │
│                                                              │
│  EXAM TIPS:                                                  │
│  "No-code ML in SageMaker?"     → Autopilot (or Canvas)     │
│  "Auto mode defaults to HPO?"   → VPC, manifest, >1000 items│
│  "Check bias in Autopilot?"     → Clarify + SHAP             │
│  "Data < 100MB auto mode?"      → Ensembling                 │
│  "Data > 100MB auto mode?"      → HPO                        │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
