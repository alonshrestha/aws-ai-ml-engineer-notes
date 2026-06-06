# Confusion Matrix

## 🤔 Why Not Just Use Accuracy?

Because accuracy can lie.

```
Rare disease test — only 1 in 1000 people have it.

Dumb model: Just says "NO" for everyone.
Accuracy: 99.9%  ← looks amazing!
Reality: It missed EVERY sick person. Useless. ❌
```

Accuracy alone doesn't tell you WHERE the model is failing. A confusion matrix does.

---

## 📊 What Is a Confusion Matrix?

It's a simple table that shows you exactly what your model got right and wrong.

For a yes/no prediction (binary classification):

```
                        ACTUAL VALUES
                    Yes (Real)    No (Real)
                 ┌──────────────┬──────────────┐
  PREDICTED  Yes │     TRUE     │    FALSE     │
  VALUES         │   POSITIVE   │   POSITIVE   │
                 │    (TP) ✅   │    (FP) ❌   │
                 ├──────────────┼──────────────┤
             No  │    FALSE     │     TRUE     │
                 │   NEGATIVE   │   NEGATIVE   │
                 │    (FN) ❌   │    (TN) ✅   │
                 └──────────────┴──────────────┘
```

Read it like this:

```
TRUE POSITIVE (TP):   Model said YES, actually YES   ✅ correct
FALSE POSITIVE (FP):  Model said YES, actually NO    ❌ wrong (false alarm)
FALSE NEGATIVE (FN):  Model said NO, actually YES    ❌ wrong (missed it!)
TRUE NEGATIVE (TN):   Model said NO, actually NO     ✅ correct
```

The diagonal (TP and TN) = where your model is RIGHT. You want big numbers here.
The off-diagonal (FP and FN) = where your model is WRONG. You want small numbers here.

---

## 🐱 Real Example: "Is There a Cat in This Image?"

```
                        ACTUAL
                   Cat        Not Cat
              ┌──────────┬──────────┐
  PREDICTED   │          │          │
  Cat         │    50    │     5    │
              │   (TP)   │   (FP)   │
              ├──────────┼──────────┤
  Not Cat     │    10    │   100    │
              │   (FN)   │   (TN)   │
              └──────────┴──────────┘

Reading this:
  50 times:  Said cat, was cat         ✅ nice
   5 times:  Said cat, was NOT cat     ❌ false alarm
  10 times:  Said not cat, WAS a cat   ❌ missed it
 100 times:  Said not cat, wasn't cat  ✅ nice
```

The diagonal (50 + 100 = 150) = correct predictions.
Off-diagonal (5 + 10 = 15) = mistakes.

---

## 🏥 Why It Matters — The Disease Example

```
                        ACTUAL
                   Sick       Healthy
              ┌──────────┬──────────┐
  PREDICTED   │          │          │
  Sick        │    1     │    10    │
              │   (TP)   │   (FP)   │
              ├──────────┼──────────┤
  Healthy     │    0     │   989    │
              │   (FN)   │   (TN)   │
              └──────────┴──────────┘

Accuracy: (1 + 989) / 1000 = 99%  ← looks great!

But look closer:
  - 10 healthy people told they're sick (false alarm — scary but fixable)
  - 0 sick people missed (good — no one dies)

Now imagine a WORSE model:

              ┌──────────┬──────────┐
  Sick        │    0     │     0    │
              │   (TP)   │   (FP)   │
              ├──────────┼──────────┤
  Healthy     │    1     │   999    │
              │   (FN)   │   (TN)   │
              └──────────┴──────────┘

Accuracy: (0 + 999) / 1000 = 99.9%  ← even higher!

But it missed the ONE sick person. That person could die.
Higher accuracy. Worse model. 

The confusion matrix shows you this. Accuracy alone doesn't.
```

---

## 🔢 With Totals (Extended Format)

Sometimes you'll see totals added:

```
                        ACTUAL
                   Cat     Not Cat    TOTAL
              ┌────────┬──────────┬─────────┐
  Cat         │   50   │     5    │   55    │ ← predicted cat 55 times
              ├────────┼──────────┼─────────┤
  Not Cat     │   10   │   100    │  110    │ ← predicted not cat 110 times
              ├────────┼──────────┼─────────┤
  TOTAL       │   60   │   105    │  165    │
              └────────┴──────────┴─────────┘
                  ↑         ↑
            60 actual   105 actual
            cats        non-cats
```

Same confusion matrix, just with row and column sums. Nothing new.

---

## 🔤 Multi-Class Confusion Matrix

Not everything is yes/no. What if you're classifying handwritten digits (0-9)?

Same concept, just bigger table:

```
              ACTUAL
         0    1    2    3    4    5    6    7    8    9
    ┌────┬────┬────┬────┬────┬────┬────┬────┬────┬────┐
  0 │ 95 │  0 │  1 │  0 │  0 │  1 │  2 │  0 │  1 │  0 │
  1 │  0 │ 92 │  2 │  1 │  0 │  0 │  0 │  3 │  2 │  0 │
  2 │  1 │  1 │ 90 │  3 │  1 │  0 │  1 │  1 │  2 │  0 │
P 3 │  0 │  0 │  2 │ 88 │  0 │  3 │  0 │  2 │  3 │  2 │
R 4 │  0 │  1 │  0 │  0 │ 93 │  0 │  2 │  0 │  1 │  3 │
E 5 │  1 │  0 │  0 │  4 │  1 │ 87 │  3 │  0 │  2 │  2 │
D 6 │  2 │  0 │  1 │  0 │  1 │  2 │ 93 │  0 │  1 │  0 │
  7 │  0 │  2 │  1 │  0 │  1 │  0 │  0 │ 91 │  1 │  4 │
  8 │  1 │  3 │  2 │  2 │  0 │  1 │  1 │  1 │ 85 │  4 │
  9 │  0 │  1 │  0 │  1 │  3 │  1 │  0 │  3 │  2 │ 89 │
    └────┴────┴────┴────┴────┴────┴────┴────┴────┴────┘

Diagonal = correct predictions (big numbers = good ✅)
Off-diagonal = mistakes (small numbers = good)
```

For example: predicted "1" but was actually "8" → happened 3 times. Makes sense — 1 and 8 can look similar in handwriting.

---

## 🌡️ Heat Map Format

Instead of numbers, sometimes colors represent the values:

```
Dark blue  = high number (many predictions)
Light blue = low number (few predictions)
White      = zero or near zero

You want:
  DARK diagonal    = lots of correct predictions ✅
  LIGHT everywhere else = few mistakes ✅
```

```
         ACTUAL
      A   B   C   D
  A  [█] [░] [░] [ ]     █ = dark (high count)
P B  [░] [█] [ ] [░]     ░ = light (low count)
R C  [ ] [░] [█] [ ]     [ ] = empty (zero)
E D  [░] [ ] [ ] [█]
D
     Dark diagonal = good model ✅
```

---

## ⚠️ EXAM WARNING: Labels Can Be Flipped!

There's no standard for which axis is "predicted" and which is "actual."

```
Format 1:                    Format 2:
  Rows = Predicted             Rows = Actual
  Columns = Actual             Columns = Predicted

ALWAYS read the labels first before answering!
```

The exam might try to trick you by flipping them. Don't assume — READ the labels.

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│              CONFUSION MATRIX — QUICK RECALL                 │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  What is it?                                                 │
│  → Table showing correct vs incorrect predictions            │
│  → Shows WHERE the model fails, not just overall accuracy    │
│                                                              │
│  Four cells (binary):                                        │
│  → TP: said yes, was yes  ✅                                 │
│  → FP: said yes, was no   ❌ (false alarm)                   │
│  → FN: said no, was yes   ❌ (missed it)                     │
│  → TN: said no, was no    ✅                                 │
│                                                              │
│  Diagonal = correct predictions (want HIGH numbers)          │
│  Off-diagonal = mistakes (want LOW numbers)                  │
│                                                              │
│  Why not just accuracy?                                      │
│  → 99.9% accuracy can still miss every sick person           │
│  → Confusion matrix reveals the full picture                 │
│                                                              │
│  Multi-class: Same idea, bigger table                        │
│  Heat map: Colors instead of numbers, dark diagonal = good   │
│                                                              │
│  ⚠️ ALWAYS check labels — predicted/actual can be flipped   │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
