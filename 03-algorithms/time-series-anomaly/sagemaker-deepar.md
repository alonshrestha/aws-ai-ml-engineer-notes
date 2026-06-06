# SageMaker DeepAR — Beginner Friendly Guide

## 🧠 What Is DeepAR?

DeepAR = **forecasting future values in time series data**

Time series = data points collected over time (like daily stock prices, monthly sales, hourly temperature).

```
You have this data (past):
  Jan: 100 sales
  Feb: 120 sales
  Mar: 90 sales
  Apr: 150 sales
  May: ???          ← DeepAR predicts this
  Jun: ???          ← and this
```

It's basically an RNN (Recurrent Neural Network) built for predicting the future based on the past.

---

## 🤔 What Makes DeepAR Special?

### It Can Learn From MULTIPLE Time Series Together

A regular RNN looks at one sequence. DeepAR can look at **many related sequences at once** and learn from the relationships between them.

```
Regular RNN:
  Just looks at Product A's sales → predicts Product A

DeepAR:
  Looks at Product A sales ──┐
  Looks at Product B sales ──┼── learns patterns → better prediction for ALL
  Looks at Product C sales ──┘

Example: If Product A sales go up, Product B always goes down.
DeepAR catches that relationship!
```

### It Finds Seasonality and Patterns

```
Ice cream sales:
  Summer: HIGH ☀️
  Winter: LOW ❄️
  Summer: HIGH ☀️
  Winter: LOW ❄️
  Next Summer: ??? → DeepAR knows it'll be HIGH

DeepAR automatically picks up these repeating patterns.
Way smarter than just drawing a straight line through the data.
```

---

## 📥 Input Format

```
Format: JSON Lines (can be gzipped) or Parquet

Each record needs:
  1. start     → starting timestamp (required)
  2. target    → list of time series values (required)
  3. cat       → categorical features (optional)
  4. dynamic_feature → dynamic features (optional)
```

Example input:

```json
{
  "start": "2024-01-01",
  "target": [100, 120, 90, 150, 130],
  "cat": [0],
  "dynamic_feature": [[0, 0, 1, 0, 0]]
}
```

### What Are Dynamic Features?

Extra info that changes over time alongside your data:

```
Product sales time series:
  Jan: 100 sales, no promotion
  Feb: 120 sales, no promotion
  Mar: 200 sales, PROMOTION RUNNING  ← this explains the spike!
  Apr: 110 sales, no promotion

dynamic_feature (promotion): [0, 0, 1, 0]

DeepAR learns: "Oh, when promotion = 1, sales go up"
```

### What Are Categorical Features?

Labels that don't change — like which product or which store:

```
cat: [0] → Product A
cat: [1] → Product B
cat: [2] → Product C

Helps DeepAR know which time series belongs to what.
```

---

## ⚠️ Important Rules (Exam Loves These!)

### 1. Always Use the ENTIRE Time Series

Don't cut your data. Give it everything, even old data.

```
❌ Wrong: Only give last 3 months
✅ Right: Give ALL historical data

Why? DeepAR can look back up to 1 YEAR to find seasonal patterns.
If you only give 3 months, it can't see that last summer was high too.
```

### 2. Training vs Testing — It's a Bit Weird

```
Your full data: [Jan, Feb, Mar, Apr, May, Jun, Jul, Aug]

For TRAINING: Remove the last few points
  → [Jan, Feb, Mar, Apr, May, Jun]

For TESTING: Use the ENTIRE dataset
  → [Jan, Feb, Mar, Apr, May, Jun, Jul, Aug]
  
  It evaluates on Jul & Aug (the ones you held back)
  but still needs Jan-Jun for context
```

### 3. Don't Predict Too Far Ahead

```
✅ Predict next 100 data points → fine
✅ Predict next 400 data points → okay, pushing it
❌ Predict next 1000 data points → gets unreliable ("wiggly")

Rule of thumb: Stay under ~400 prediction points
```

### 4. More Time Series = Better

```
Training on 1 time series:   okay
Training on 10 time series:  better
Training on 100 time series: best! 🎯

DeepAR's real power comes from learning across 
multiple related time series together.
```

---

## 🎛️ Key Hyperparameters

| Parameter | What It Does | Simple Analogy |
|-----------|-------------|----------------|
| `epochs` | How many times to go through all training data | Reading the textbook 1 time vs 10 times |
| `batch_size` | How many examples per training step | Studying 1 problem vs 10 problems at a time |
| `learning_rate` | How big each learning step is | Baby steps vs giant leaps |
| `num_cells` | How many neurons in the network | Brain size — more neurons = more capacity |
| `context_length` | How many past time points the model sees before predicting | Looking back 7 days vs 30 days before guessing tomorrow |

### About context_length:

```
context_length = 30 means:
  "Look at the last 30 data points to predict the next one"

But even if context_length is short, DeepAR still 
looks back up to 1 YEAR for seasonal patterns.

That's why you always give it the full dataset!
```

---

## 💻 What Machines to Use?

### Training:

```
START with CPU (AWS recommends this!):
  ✅ ml.c4.2xlarge (good starting point)
  ✅ ml.c4.4xlarge (if you need more)

Move to GPU ONLY if:
  → Very large model
  → Mini batch size > 512
  
  Otherwise GPU is overkill and wastes money.
```

```
✅ Single machine — works
✅ Multiple machines — also works (scalable!)
```

### Inference (making predictions):
```
✅ CPU only — GPU not supported for inference
```

### Tuning (finding best hyperparameters):
```
⚠️ Tuning can be very heavy
   → Use bigger instances for tuning
   → Then scale down for production

Example:
  Tuning:     ml.c4.4xlarge (beefy)
  Production: ml.c4.2xlarge (smaller, cheaper)
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────┐
│                DEEPAR — QUICK RECALL                     │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  What is it?    Time series forecasting (RNN-based)      │
│                                                          │
│  Special?       Trains on MULTIPLE related time series   │
│                 Finds seasonality and patterns            │
│                                                          │
│  Input?         JSON Lines (gzip ok) or Parquet          │
│                 Needs: start timestamp + target values    │
│                 Optional: dynamic features, categories    │
│                                                          │
│  MUST do:       Always use ENTIRE time series            │
│                 Don't predict beyond ~400 points          │
│                 More time series = better results         │
│                                                          │
│  Training/      Train: remove last few points             │
│  Testing:       Test: use full dataset                    │
│                                                          │
│  Instances:     Start with CPU (ml.c4.2xlarge)           │
│                 GPU only if large model or batch > 512   │
│                 Single or multi-machine ✅                │
│                 Inference: CPU only                       │
│                                                          │
│  Tuning:        Use bigger instances for tuning          │
│                 Scale down for production                 │
│                                                          │
│  context_length Can be short — model still looks back    │
│                 up to 1 year for seasonality              │
│                                                          │
└──────────────────────────────────────────────────────────┘
```
