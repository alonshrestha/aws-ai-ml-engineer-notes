# SageMaker Factorization Machines — Beginner Friendly Guide

## 🧠 What Is Factorization Machines?

It's built for **sparse data** — when you have a HUGE table but most of it is empty.

The classic use case: **recommender systems** (like Netflix or Amazon suggestions).

---

## 🤔 What Is "Sparse Data"?

Imagine a table of users and movies:

```
              Movie1  Movie2  Movie3  Movie4  ... Movie10000
User A:         5       ?       ?       3     ...    ?
User B:         ?       ?       4       ?     ...    ?
User C:         ?       2       ?       ?     ...    5
User D:         ?       ?       ?       ?     ...    ?

? = never watched/rated (MOST of the table is ?)
```

Each user has only rated a tiny fraction of all movies. 99% of the table is empty. That's sparse data.

```
Sparse = mostly empty
Dense  = mostly filled

A user rates maybe 50 movies out of 10,000
That's 0.5% filled, 99.5% empty → VERY sparse
```

---

## 🎯 What Does It Do?

It fills in the blanks. It predicts what those `?` values would be.

```
User A rated Movie1: 5 stars, Movie4: 3 stars
User A never watched Movie3

Factorization Machines predicts:
  "User A would probably rate Movie3: 4 stars"

Now you can recommend Movie3 to User A!
```

It can do:
```
Classification: "Will this user LIKE this movie?" → Yes/No
Regression:     "What RATING will this user give?" → 4.2 stars
```

It's **supervised** — you train it on the ratings/interactions you DO have.

---

## 📦 Real World Examples

```
1. Product recommendations (Amazon)
   Users × Products → predict what they'd buy

2. Movie recommendations (Netflix)
   Users × Movies → predict what they'd rate

3. Click prediction (ads)
   Users × Web pages → predict what they'd click

All of these = sparse data
(users only interact with a tiny fraction of all items)
```

---

## ⚙️ How Does It Work?

Think of it as a giant matrix (table) that you're trying to fill in:

```
The matrix:
              Item1  Item2  Item3  Item4
  User A:       5      ?      ?      3
  User B:       ?      ?      4      ?
  User C:       ?      2      ?      ?

Factorization = break this matrix into FACTORS (smaller pieces)

Factor 1 (user preferences):     Factor 2 (item properties):
  User A: [0.8, 0.3]              Item1: [0.9, 0.1]
  User B: [0.2, 0.7]              Item2: [0.3, 0.8]
  User C: [0.5, 0.5]              Item3: [0.4, 0.6]
                                   Item4: [0.7, 0.4]

Multiply factors together → fills in the blanks!
  User A × Item3 = 0.8×0.4 + 0.3×0.6 = 0.50 → predicted rating
```

It finds hidden patterns like "User A likes action movies" and "Item3 is an action movie" without anyone telling it those categories.

---

## ⚠️ Important Limitations

```
Works with PAIRS only (2 dimensions):
  ✅ Users × Items
  ✅ Users × Pages
  ✅ Sessions × Products

  It's factorizing a 2D matrix, so you need two dimensions.
```

---

## 📥 Input Format

```
✅ RecordIO-Protobuf (Float32) ONLY
❌ CSV does NOT work

Why no CSV?
  Because the data is sparse — a CSV would be:
  5,,,3,,,,,,,,,,,,,,,,
  ,,4,,,,,,,,,,,,,,,,,
  
  Mostly commas. Wasteful and impractical.
  RecordIO-Protobuf handles sparse data efficiently.
```

---

## 🎛️ Key Hyperparameters

| Parameter | What It Does |
|-----------|-------------|
| `bias_init_method` | How to initialize bias terms (uniform, normal, or constant) |
| `factors_init_method` | How to initialize factors (uniform, normal, or constant) |
| `linear_init_method` | How to initialize linear terms (uniform, normal, or constant) |

Not much to tune here. The main decision is choosing this algorithm in the first place.

---

## 💻 What Machines to Use?

```
✅ CPU recommended for training
   (sparse data = CPU is fine)

✅ GPU works too, but only useful for DENSE data
   → If your data is dense, why are you using 
     factorization machines? 🤔

Rule: Sparse data → Factorization Machines → CPU
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────┐
│        FACTORIZATION MACHINES — QUICK RECALL             │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  What is it?    Classification/regression on SPARSE data │
│                                                          │
│  Supervised?    YES                                      │
│                                                          │
│  Classic use:   Recommender systems                      │
│                 (users × items, click prediction)         │
│                                                          │
│  Sparse data:   Most of the table is empty               │
│                 (users only rate a few items)             │
│                                                          │
│  How?           Factorizes a 2D matrix into smaller      │
│                 factors, multiplies to fill blanks        │
│                                                          │
│  Limitation:    Pairwise only (2 dimensions)             │
│                                                          │
│  Input?         RecordIO-Protobuf ONLY (Float32)         │
│                 ❌ No CSV (sparse data = too many commas) │
│                                                          │
│  Instances:     CPU recommended                          │
│                 GPU only if dense data (unlikely)         │
│                                                          │
│  #1 exam tip:   Recommender system → Factorization       │
│                 Machines                                  │
│                                                          │
└──────────────────────────────────────────────────────────┘
```
