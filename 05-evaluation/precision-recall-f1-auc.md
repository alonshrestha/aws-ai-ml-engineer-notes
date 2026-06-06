# Precision, Recall, F1, ROC & AUC — Model Evaluation Metrics

## 🔑 Super Important for the Exam — Memorize These!

All these metrics come from the confusion matrix. They tell you different things about HOW your model is failing, not just IF it's failing.

Quick refresher on the confusion matrix:

```
                     ACTUAL
                  Yes         No
            ┌───────────┬───────────┐
Pred Yes    │    TP      │    FP     │
            │ (correct)  │ (false    │
            │            │  alarm)   │
            ├───────────┼───────────┤
Pred No     │    FN      │    TN     │
            │ (missed    │ (correct) │
            │  it!)      │           │
            └───────────┴───────────┘

⚠️ Layout can vary on the exam — ALWAYS read the labels first!
```

---

## 📏 Metric 1: Recall

```
                    TP
Recall = ─────────────────
              TP + FN
```

Also called: Sensitivity, True Positive Rate, Completeness

### What it answers:

"Out of all the things that WERE positive, how many did I catch?"

```
Example: Fraud detection

100 transactions were actually fraud

Your model caught 80 of them
Your model missed 20 of them

Recall = 80 / (80 + 20) = 80%

"I caught 80% of all fraud"
```

### When to care about Recall:

When FALSE NEGATIVES are dangerous — when MISSING something is the worst outcome.

```
Fraud detection:
  FN = Fraud happened but model said "all good" 
  → Money stolen. Terrible. ❌
  
  FP = Normal transaction flagged as fraud
  → Annoying but fixable. Just review it. 🤷

You'd rather have false alarms than miss real fraud.
→ Optimize for RECALL
```

### Example with numbers:

```
                     ACTUAL
                  Yes         No
            ┌───────────┬───────────┐
Pred Yes    │    5       │    20     │
            ├───────────┼───────────┤
Pred No     │    10      │   100     │
            └───────────┴───────────┘

Recall = TP / (TP + FN)
       = 5 / (5 + 10)
       = 5 / 15
       = 33.3%

"Out of 15 actual positives, I only caught 5. Not great."
```

---

## 🎯 Metric 2: Precision

```
                      TP
Precision = ─────────────────
                TP + FP
```

Also called: Correct Positive Rate, Percent of Relevant Results

### What it answers:

"Out of everything I SAID was positive, how many actually were?"

```
Example: Drug testing

Your model flagged 50 people as positive

40 of them actually were positive
10 of them were clean (false alarm)

Precision = 40 / (40 + 10) = 80%

"80% of the people I flagged actually were positive"
```

### When to care about Precision:

When FALSE POSITIVES are dangerous — when a false alarm causes real harm.

```
Drug testing:
  FP = Clean person told they tested positive
  → Career ruined, reputation destroyed. Terrible. ❌
  
  FN = Drug user passes the test
  → Bad, but doesn't destroy an innocent person's life.

You'd rather miss some positives than falsely accuse someone.
→ Optimize for PRECISION
```

### Example with numbers:

```
                     ACTUAL
                  Yes         No
            ┌───────────┬───────────┐
Pred Yes    │    5       │    20     │
            ├───────────┼───────────┤
Pred No     │    10      │   100     │
            └───────────┴───────────┘

Precision = TP / (TP + FP)
          = 5 / (5 + 20)
          = 5 / 25
          = 20%

"Out of 25 things I said were positive, only 5 actually were. Yikes."
```

---

## 🤝 Recall vs Precision — The Tradeoff

You usually can't have both high. Improving one hurts the other.

```
Fraud detection example:

Strategy 1: Flag EVERYTHING as fraud
  → Recall = 100% (caught all fraud!)
  → Precision = 1% (99% were false alarms)

Strategy 2: Only flag when 100% sure
  → Precision = 99% (almost never wrong)
  → Recall = 10% (missed 90% of fraud)

You have to decide what matters more for YOUR problem.
```

### Quick decision guide:

```
"Missing a positive is DANGEROUS"     → care about RECALL
  (fraud, cancer, security threats)

"False alarm is DANGEROUS"            → care about PRECISION  
  (drug tests, spam filter, criminal conviction)
```

---

## ⚖️ Metric 3: F1 Score

```
              2 × TP
F1 = ─────────────────────────
      2 × TP + FP + FN
```

OR equivalently:

```
         2 × Precision × Recall
F1 = ─────────────────────────────
          Precision + Recall
```

### What it is:

The harmonic mean of precision and recall. A single number that balances both.

```
Precision = 20%
Recall = 33%

F1 = 2 × (0.20 × 0.33) / (0.20 + 0.33)
   = 2 × 0.066 / 0.53
   = 0.132 / 0.53
   = 24.9%
```

### When to use:

When you care about BOTH precision and recall and want one number to compare models.

```
Model A: Precision 90%, Recall 20%  → F1 = 32.7%
Model B: Precision 70%, Recall 65%  → F1 = 67.4%

Model B is more balanced → higher F1
```

But in the real world, you usually care more about one than the other. F1 is a shortcut when you can't decide.

---

## 📐 Metric 4: Specificity

```
                        TN
Specificity = ─────────────────
                  TN + FP
```

Also called: True Negative Rate

### What it answers:

"Out of all the things that WERE negative, how many did I correctly identify as negative?"

```
1000 healthy people tested
950 correctly told "healthy"
50 incorrectly told "sick" (false alarm)

Specificity = 950 / (950 + 50) = 95%
```

---

## 📊 Metric 5: RMSE (Root Mean Squared Error)

```
RMSE = √(average of all squared errors)
```

### What it is:

A straight-up accuracy measure. How far off are your predictions from reality?

```
Predicted house prices vs actual:

Predicted: $300K  Actual: $310K  Error: $10K  Squared: 100
Predicted: $250K  Actual: $245K  Error: $5K   Squared: 25
Predicted: $400K  Actual: $380K  Error: $20K  Squared: 400

Average of squared errors: (100 + 25 + 400) / 3 = 175
RMSE = √175 = $13.2K

"On average, predictions are off by about $13.2K"
```

### When to use:

- Regression problems (predicting numbers, not categories)
- When you just care about overall accuracy
- Doesn't get into precision/recall nuances

---

## 📈 Metric 6: ROC Curve & AUC

### ROC Curve (Receiver Operating Characteristic):

Plots True Positive Rate (recall) vs False Positive Rate at different thresholds.

```
True Positive
Rate (Recall)
    ▲
1.0 │        ●●●●●●●●●●●●●●●●●
    │     ●●●
    │   ●●        ← Good model (bent toward upper-left)
    │  ●
    │ ●
0.5 │●─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─  ← Diagonal = random guessing
    │          ╱
    │        ╱
    │      ╱
    │    ╱
0.0 └──────────────────────────▶
   0.0                        1.0
         False Positive Rate

Curve bent toward upper-left = GOOD ✅
Curve on the diagonal = USELESS (random guessing) ❌
```

### AUC (Area Under the Curve):

Just the area under the ROC curve. One number to summarize it.

```
AUC = 1.0   → Perfect model (curve is a right angle at top-left)
AUC = 0.5   → Random guessing (curve is the diagonal line)
AUC < 0.5   → Worse than random (something is very wrong)

┌────────────────────────────────┐
│  AUC = 1.0    Perfect       🏆 │
│  AUC = 0.9+   Excellent    ✅ │
│  AUC = 0.8    Good         👍 │
│  AUC = 0.7    Fair         🤷 │
│  AUC = 0.5    Useless      ❌ │
│  AUC < 0.5    Broken       💀 │
└────────────────────────────────┘
```

### When to use:

Comparing different classifiers. Higher AUC = better model.

---

## 📉 Metric 7: PR Curve (Precision-Recall Curve)

Plots Precision vs Recall at different thresholds. Similar to ROC but better for specific cases.

```
Precision
    ▲
1.0 │●●●
    │   ●●●
    │      ●●●        ← Good model
    │         ●●●
    │            ●●●
0.5 │               ●●●
    │
    │
0.0 └──────────────────────────▶
   0.0                        1.0
              Recall

Higher area under curve = better ✅
```

### When to use PR curve instead of ROC:

```
ROC curve:  Good for most classification problems
PR curve:   Better for information retrieval problems
            (searching millions of documents, only few are relevant)
            
Why? When you have millions of negatives and few positives,
ROC can look misleadingly good. PR curve shows the real picture.
```

---

## 🎯 Exam Cheat Sheet — MEMORIZE THIS

```
┌──────────────────────────────────────────────────────────────┐
│         MODEL EVALUATION METRICS — MUST KNOW                 │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  RECALL = TP / (TP + FN)                                     │
│  → "How many positives did I catch?"                         │
│  → Care about: FALSE NEGATIVES                               │
│  → Use for: Fraud detection, cancer screening                │
│  → Also called: Sensitivity, True Positive Rate              │
│                                                              │
│  PRECISION = TP / (TP + FP)                                  │
│  → "How many of my positive calls were correct?"             │
│  → Care about: FALSE POSITIVES                               │
│  → Use for: Drug testing, spam filters                       │
│  → Also called: Correct Positive Rate                        │
│                                                              │
│  F1 = 2×TP / (2×TP + FP + FN)                               │
│  → Balances precision and recall                             │
│  → Harmonic mean of both                                     │
│  → Use when you care about both equally                      │
│                                                              │
│  SPECIFICITY = TN / (TN + FP)                                │
│  → True Negative Rate                                        │
│                                                              │
│  RMSE = √(avg of squared errors)                             │
│  → Pure accuracy measure for regression                      │
│  → No precision/recall nuance                                │
│                                                              │
│  ROC CURVE: TPR vs FPR at different thresholds               │
│  → Bent toward upper-left = good                             │
│  → On diagonal = random guessing                             │
│                                                              │
│  AUC: Area under ROC curve                                   │
│  → 1.0 = perfect, 0.5 = useless, <0.5 = broken              │
│  → Higher = better. Use to compare models.                   │
│                                                              │
│  PR CURVE: Precision vs Recall curve                         │
│  → Like ROC but better for information retrieval             │
│  → Higher area = better                                      │
│                                                              │
│  ─────────────────────────────────────────────────────────── │
│                                                              │
│  QUICK DECISION:                                             │
│  "Missing something is bad"  → RECALL (fraud, cancer)        │
│  "False alarm is bad"        → PRECISION (drug test, spam)   │
│  "Both matter equally"       → F1 SCORE                      │
│  "Just want overall accuracy"→ RMSE                          │
│  "Compare classifiers"       → AUC                           │
│  "Info retrieval at scale"   → PR CURVE                      │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
