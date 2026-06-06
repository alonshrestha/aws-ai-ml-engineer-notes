# RMSE, R-Squared & MAE — Regression Metrics

## 🔑 Two Types of Predictions = Two Types of Metrics

Everything we learned before (precision, recall, F1, AUC) was for classification — predicting categories.

```
Classification: "Is it fraud?"     → Yes or No
Regression:     "What's the price?" → $347,000
```

You can't use recall or precision on a number. "Was $347,000 a true positive?" doesn't make sense.

So for numerical predictions, we use different metrics.

---

## 📏 The Basic Idea: How Far Off Are You?

You predicted house prices. Some were close, some were off:

```
Predicted    Actual     Error (how far off)
$300K        $310K      $10K off
$250K        $240K      $10K off
$400K        $380K      $20K off
$500K        $520K      $20K off
```

All three metrics below just measure this "how far off" in different ways.

---

## 📐 Metric 1: MAE (Mean Absolute Error)

The simplest one. Just average all the errors.

```
MAE = average of all errors (ignoring + or -)

Errors: $10K, $10K, $20K, $20K

MAE = (10 + 10 + 20 + 20) / 4 = $15K

"On average, predictions are off by $15K"
```

That's it. Dead simple. Just the average distance between prediction and reality.

---

## 📐 Metric 2: RMSE (Root Mean Squared Error)

Same idea as MAE but squares the errors first, then takes the square root.

```
RMSE = √(average of squared errors)

Errors: $10K, $10K, $20K, $20K

Squared: 100, 100, 400, 400
Average: (100 + 100 + 400 + 400) / 4 = 250
Square root: √250 = $15.8K
```

### Why not just use MAE?

RMSE punishes big errors more than small ones.

```
Two models predicting 4 house prices:

Model A errors: $10K, $10K, $10K, $10K  (consistently a bit off)
Model B errors: $1K, $1K, $1K, $37K     (one huge miss)

MAE:
  Model A: (10+10+10+10)/4 = $10K
  Model B: (1+1+1+37)/4 = $10K
  MAE says: "Both equally good" 🤷

RMSE:
  Model A: √((100+100+100+100)/4) = $10K
  Model B: √((1+1+1+1369)/4) = $18.5K
  RMSE says: "Model B is worse because of that big miss" ✅
```

RMSE catches outlier mistakes that MAE hides.

```
MAE  → treats all errors equally
RMSE → punishes big errors more (better at catching outliers)
```

---

## 📐 Metric 3: R-Squared (R²)

Different from MAE and RMSE. Instead of measuring error, it measures how well your predictions FIT the data.

```
R² = how much of the data's pattern does your model explain?

R² = 1.0  → model perfectly explains the data
R² = 0.8  → model explains 80% of the pattern
R² = 0.0  → model explains nothing (just guessing the average)
R² < 0    → model is worse than just guessing the average
```

Think of it like this:

```
Actual data points:     ▲  ▲     ▲
                     ▲     ▲  ▲
                  ▲     ▲

Your prediction line:  ──────────────

R² = how close are the ▲ points to your line?

All points ON the line     → R² = 1.0 (perfect fit)
Points scattered near line → R² = 0.8 (good fit)
Points everywhere randomly → R² = 0.0 (no fit at all)
```

---

## 🥊 When to Use Which

```
┌──────────┬─────────────────────────────────────────┐
│ Metric   │ When to use                             │
├──────────┼─────────────────────────────────────────┤
│ MAE      │ Simple "how far off am I on average?"   │
│          │ Treats all errors equally                │
├──────────┼─────────────────────────────────────────┤
│ RMSE     │ Same but punishes big errors more        │
│          │ Use when outlier mistakes are bad        │
├──────────┼─────────────────────────────────────────┤
│ R²       │ "How well does my model fit the data?"  │
│          │ Closer to 1.0 = better fit              │
└──────────┴─────────────────────────────────────────┘
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│           REGRESSION METRICS — QUICK RECALL                  │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  Classification metrics (categories):                        │
│  → Precision, Recall, F1, AUC                                │
│                                                              │
│  Regression metrics (numbers):                               │
│  → MAE, RMSE, R²                                            │
│                                                              │
│  ─────────────────────────────────────────────────────────── │
│                                                              │
│  MAE = average of absolute errors                            │
│  → Simple, treats all errors equally                         │
│                                                              │
│  RMSE = √(average of squared errors)                         │
│  → Punishes big errors more than MAE                         │
│  → Better at catching outlier mistakes                       │
│                                                              │
│  R² = how well model fits the data                           │
│  → 1.0 = perfect, 0.0 = useless, <0 = worse than guessing   │
│                                                              │
│  ─────────────────────────────────────────────────────────── │
│                                                              │
│  EXAM TIP:                                                   │
│  "Predicting categories?" → Precision, Recall, F1, AUC      │
│  "Predicting numbers?"    → RMSE, MAE, R²                   │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
