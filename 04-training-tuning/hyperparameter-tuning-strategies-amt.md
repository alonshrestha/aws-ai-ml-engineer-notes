# Hyperparameter Tuning Strategies in AMT

## ⏹️ Early Stopping in AMT

Same concept as early stopping in training — but applied to tuning jobs.

```
Tuning job trying lr=0.001, depth=15:

  Epoch 1: accuracy 60%
  Epoch 2: accuracy 61%
  Epoch 3: accuracy 61%
  Epoch 4: accuracy 60.5%

  AMT: "This combination isn't going anywhere. 
        Stop wasting money. Kill this job. 
        Move on to the next combination."
```

How to enable: just set `early_stopping = auto`

Two benefits:
- Saves compute time (stops bad jobs early)
- Avoids overfitting (doesn't train too long)

Only works with algorithms that report metrics after each epoch (most deep learning algorithms do this).

---

## 🔥 Warm Start

Use results from a PREVIOUS tuning job as the starting point for a new one.

```
Without warm start:

  Tuning job 1: Tried 50 combinations → best was lr=0.05, depth=8
  Tuning job 2: Starts from SCRATCH → tries everything again 😩

With warm start:

  Tuning job 1: Tried 50 combinations → best was lr=0.05, depth=8
  Tuning job 2: "I know lr=0.05 area is good. 
                 Let me start searching AROUND there." ✅
```

### Also useful for recovery:

```
Tuning job running... 40 out of 100 jobs done... CRASH! 💥

Without warm start: Start over from job 1. All progress lost.
With warm start:    Pick up from job 40. Continue. ✅
```

### Two warm start options:

```
Identical Data and Algorithm:
  → Same data, same algorithm, just continuing the search
  → Most common use case

Transfer Learning:
  → Different data sets between tuning runs
  → Useful when you want to try new data while keeping 
    what you learned about good hyperparameters
```

---

## 🔍 Four Tuning Strategies — Know the Differences

### 1. Grid Search (Dumbest)

Try EVERY possible combination. Brute force.

```
learning_rate: [0.01, 0.05, 0.1]
max_depth:     [3, 5, 7]

Grid search tries ALL 9 combinations:
  (0.01, 3), (0.01, 5), (0.01, 7)
  (0.05, 3), (0.05, 5), (0.05, 7)
  (0.1, 3),  (0.1, 5),  (0.1, 7)
```

```
Pros: Guaranteed to find the best in the grid
Cons: Explodes with more parameters
      Only works with categorical parameters
      Very expensive
```

### 2. Random Search (Simple but Okay)

Pick random combinations and hope for the best.

```
Instead of trying all 9:
  Random pick 1: (0.05, 7)  → 88%
  Random pick 2: (0.01, 3)  → 79%
  Random pick 3: (0.1, 5)   → 85%
  Random pick 4: (0.05, 5)  → 87%

  Best so far: (0.05, 7) at 88%
```

```
Pros: Can run ALL jobs in parallel (no dependencies)
      Fast because of parallelism
Cons: Might miss the best combination (it's random)
      Doesn't learn from previous results
```

### 3. Bayesian Optimization (Smart)

Learns from each run. Treats tuning as a regression problem.

```
Job 1: (0.01, 3) → 79%
Job 2: (0.05, 5) → 87%   ← AMT learns: higher lr helped
Job 3: (0.06, 6) → 89%   ← AMT learns: keep going this direction
Job 4: (0.055, 7) → 91%  ← AMT converges on the sweet spot
```

```
Pros: Learns as it goes → finds best values with fewer jobs
      Smarter than random
Cons: Can't fully parallelize (needs to learn from previous runs)
      Can be slower because of sequential nature
```

### 4. Hyperband (Best of All Worlds)

Dynamically allocates resources, does early stopping and parallel processing automatically.

```
Starts many jobs at once with small budgets:
  Job A: quick test → looks promising → give more resources
  Job B: quick test → looks bad → kill it early
  Job C: quick test → looks great → give even more resources
  Job D: quick test → looks terrible → kill it immediately

Survivors get more training time. Losers get cut fast.
```

```
Pros: Fastest overall
      Auto early stopping
      Auto parallel processing
      Best of random + Bayesian
Cons: Only works with algorithms that report metrics 
      iteratively (epoch by epoch)
```

---

## 🥊 Strategy Comparison

```
┌────────────────┬───────────┬───────────┬──────────┬──────────────┐
│                │   Grid    │  Random   │ Bayesian │  Hyperband   │
├────────────────┼───────────┼───────────┼──────────┼──────────────┤
│ How it picks   │ Try ALL   │ Random    │ Learns   │ Learns +     │
│                │           │           │ from     │ auto stops   │
│                │           │           │ results  │ bad jobs     │
├────────────────┼───────────┼───────────┼──────────┼──────────────┤
│ Parallel?      │ Yes       │ Yes       │ Limited  │ Yes          │
├────────────────┼───────────┼───────────┼──────────┼──────────────┤
│ Learns?        │ No        │ No        │ Yes      │ Yes          │
├────────────────┼───────────┼───────────┼──────────┼──────────────┤
│ Speed          │ Slowest   │ Fast      │ Medium   │ Fastest      │
├────────────────┼───────────┼───────────┼──────────┼──────────────┤
│ Cost           │ Highest   │ Medium    │ Medium   │ Lowest       │
├────────────────┼───────────┼───────────┼──────────┼──────────────┤
│ Best for       │ Few       │ Quick     │ Smart    │ Deep         │
│                │ categorical│ rough    │ search   │ learning     │
│                │ params    │ search    │          │ algorithms   │
└────────────────┴───────────┴───────────┴──────────┴──────────────┘
```

---

## 💰 Resource Limits

AWS puts default limits to protect you from huge bills:

```
Limits on:
  → Number of parallel tuning jobs
  → Number of hyperparameters you can tune at once
  → Number of training jobs per tuning job

Want higher limits? → Request quota increase from AWS Support
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│       AMT STRATEGIES & FEATURES — QUICK RECALL               │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  EARLY STOPPING:                                             │
│  → Set to "auto"                                             │
│  → Kills bad tuning jobs early                               │
│  → Saves cost + avoids overfitting                           │
│  → Needs algorithms that report metrics per epoch            │
│                                                              │
│  WARM START:                                                 │
│  → Uses previous tuning job results as starting point        │
│  → Two types: Identical Data/Algo, Transfer Learning         │
│  → Also useful for recovery if job crashes                   │
│                                                              │
│  ─────────────────────────────────────────────────────────── │
│                                                              │
│  STRATEGIES:                                                 │
│  Grid search    → tries everything, categorical only         │
│  Random search  → random picks, fully parallel, no learning  │
│  Bayesian       → learns from results, limited parallel      │
│  Hyperband      → fastest, auto stops bad jobs, best overall │
│                                                              │
│  ─────────────────────────────────────────────────────────── │
│                                                              │
│  EXAM TIPS:                                                  │
│  "Fastest tuning strategy?"        → Hyperband               │
│  "Fully parallel tuning?"          → Random search           │
│  "Learns from previous runs?"      → Bayesian or Hyperband   │
│  "Resume failed tuning job?"       → Warm start              │
│  "Stop bad jobs automatically?"    → Early stopping (auto)   │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
