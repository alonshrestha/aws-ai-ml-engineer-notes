# Tuning Neural Networks — Beginner Friendly Guide

## ⚠️ This Is Heavily Tested on the Exam!

This section is about the real-world stuff that catches people off guard. Know this well.

---

## 🎯 What Is Learning Rate?

Remember gradient descent — walking downhill to find the lowest error. Learning rate = **how big each step is**.

```
Error
  ▲
  │ ●
  │  \
  │   \
  │    \
  │     ●    ← we want to find this lowest point
  │      \  /
  │       ●
  └──────────────▶ different weight values
```

The model tries different weights, step by step, looking for the lowest error. Learning rate controls the step size.

### Too HIGH learning rate:

```
Error
  ▲
  │ ●
  │          ●          ← jumped right OVER the best answer!
  │
  │     ●  ← best answer (missed it!)
  │
  └──────────────▶

Big steps = might SKIP the best solution entirely
```

### Too LOW learning rate:

```
Error
  ▲
  │ ●
  │ ●
  │  ●
  │  ●
  │   ●
  │   ●
  │    ●  ← taking forever to get here...
  └──────────────▶

Tiny steps = will find the answer but takes VERY long
```

### Just RIGHT:

```
Error
  ▲
  │ ●
  │   ●
  │      ●
  │         ● ← found it in reasonable time!
  └──────────────▶
```

```
┌─────────────────────────────────────────────┐
│  Learning rate too HIGH → overshoots answer  │
│  Learning rate too LOW  → training too slow   │
│  MEMORIZE THIS FOR THE EXAM!                 │
└─────────────────────────────────────────────┘
```

---

## 🎯 What Is Batch Size?

Batch size = **how many training examples the model looks at in one step**.

```
You have 1000 training images.

Batch size = 100:
  Step 1: look at images 1-100, adjust weights
  Step 2: look at images 101-200, adjust weights
  ... 10 steps per epoch

Batch size = 10:
  Step 1: look at images 1-10, adjust weights
  Step 2: look at images 11-20, adjust weights
  ... 100 steps per epoch (more steps, smaller chunks)
```

### The Counterintuitive Part (EXAM LOVES THIS):

You'd think bigger batch = better. WRONG.

### The Local Minima Problem:

```
Error
  ▲
  │
  │    ╲  ╱╲
  │     ╲╱   ╲
  │     ↑     ╲    ╱
  │  "local    ╲  ╱
  │  minima"    ╲╱ ← this is the REAL best answer
  │              ↑
  │           "global minima"
  └──────────────────────▶
```

There can be multiple "dips" in the error curve. The model might get stuck in a small dip (local minima) and think it found the best answer, when the REAL best answer is in a deeper dip further along.

### Small Batch Size:

```
Small batch = lightweight = can wiggle out of local minima

  ╲  ╱╲
   ╲╱   ╲
   ↑     ╲    ╱
   "Nah,  ╲  ╱
   I can   ╲╱ ← finds the real answer!
   escape
   this"
```

### Large Batch Size:

```
Large batch = heavy = gets STUCK in local minima

  ╲  ╱╲
   ╲╱   ╲
   ↑     ╲    ╱
   🪨     ╲  ╱
  "Stuck   ╲╱ ← never reaches here
   here!"
```

### Even Worse: Inconsistent Results

Because data is shuffled randomly each epoch, a too-large batch size might:
- Sometimes get stuck in local minima
- Sometimes escape it
- Different results every time you run it!

```
Run 1: gets stuck → bad result
Run 2: escapes → good result
Run 3: gets stuck → bad result

"Why do I get different answers every time?!"
→ Batch size is probably too large
```

---

## 📝 What Is an Epoch?

One epoch = one complete pass through ALL your training data.

```
You have 1000 images.

Epoch 1: model sees all 1000 images → adjusts weights
Epoch 2: model sees all 1000 images again → adjusts more
Epoch 3: model sees all 1000 images again → adjusts more
...

More epochs = more practice = (usually) better model
But too many epochs = overfitting (memorizes instead of learns)
```

---

## 🧠 How They Work Together

```
Training loop:

For each EPOCH:
  Shuffle the data randomly
  Break data into BATCHES (based on batch size)
  
  For each BATCH:
    Forward pass → make predictions
    Calculate error
    Backward pass (backpropagation) → adjust weights
    Step size of adjustment = LEARNING RATE
```

---

## 🎯 Exam Cheat Sheet — MEMORIZE THIS

```
┌──────────────────────────────────────────────────────────┐
│      TUNING NEURAL NETWORKS — MUST KNOW FOR EXAM        │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  LEARNING RATE:                                          │
│    Too high → overshoots the best solution               │
│    Too low  → training takes too long                    │
│                                                          │
│  BATCH SIZE:                                             │
│    Too large → gets stuck in local minima                │
│              → inconsistent results run to run            │
│    Too small → more steps, slower per epoch               │
│    Small is generally BETTER (counterintuitive!)         │
│                                                          │
│  EPOCH:                                                  │
│    One full pass through all training data                │
│    More epochs = more learning (up to a point)           │
│                                                          │
│  KEY EXAM ANSWERS:                                       │
│    "Model overshoots" → learning rate too high           │
│    "Stuck in local minima" → batch size too large        │
│    "Inconsistent results" → batch size too large         │
│    "Training too slow" → learning rate too low           │
│                                                          │
└──────────────────────────────────────────────────────────┘
```
