# LLM Key Terms — Tokens, Embeddings, Temperature & More

## 📝 Tokens

Words (or parts of words) converted to numbers. You already know this.

```
"Hello world" → [15496, 995]

Computers need numbers, not words. Tokens = that conversion.
Think of tokens ≈ words (but sometimes one word = multiple tokens).
```

---

## 📐 Embeddings

Tokens converted to vectors that capture MEANING. You already know this too.

```
Token [15496] → [0.2, 0.8, 0.1, 0.5, ...]

Similar words = close vectors:
  "Hello" and "Hi"  → close together
  "Hello" and "Car" → far apart

Works across languages:
  "Hello" (English) and "Hola" (Spanish) 
  → similar vectors (same meaning)
```

---

## 🎯 How GPT Picks the Next Word: Top P, Top K, Temperature

After GPT processes your input, it gets probabilities for every possible next word. Three settings control which word gets picked.

### Top P (Probability Threshold)

"Only consider words above this probability."

```
Next word probabilities:
  "incredible" = 35%
  "amazing"    = 25%
  "great"      = 20%
  "hard"       = 10%
  "boring"     = 5%
  "purple"     = 3%
  "fish"       = 2%

Top P = 0.80 → keep adding words until probabilities sum to 80%:
  "incredible" (35%) + "amazing" (25%) + "great" (20%) = 80%
  → Only these 3 are candidates. Rest eliminated.

Higher Top P = more candidates = more random
Lower Top P  = fewer candidates = more predictable
```

### Top K (Fixed Number of Candidates)

"Only consider the top K words."

```
Same probabilities:
  "incredible" = 35%
  "amazing"    = 25%
  "great"      = 20%
  "hard"       = 10%
  ...

Top K = 3 → only top 3 words are candidates:
  "incredible", "amazing", "great"
  → Rest eliminated regardless of probability.

Higher K = more candidates = more random
Lower K  = fewer candidates = more predictable
```

### Temperature (Randomness Dial)

After Top P or Top K narrows down the candidates, temperature decides how to pick from them.

```
Candidates: "incredible" (35%), "amazing" (25%), "great" (20%)

Temperature = 0:
  Always pick highest → "incredible" every time
  Predictable, consistent

Temperature = 0.7:
  Usually "incredible", sometimes "amazing"
  Balanced

Temperature = 1.0:
  Could be any of the three, almost equally
  Creative, varied, sometimes weird
```

### How they work together:

```
Step 1: GPT outputs probabilities for all 50,000+ words
Step 2: Top P or Top K narrows it down to a few candidates
Step 3: Temperature decides how randomly to pick from those candidates
Step 4: One word is selected → output
```

---

## 📏 Context Window

How many tokens the model can see at once. Think of it as the model's "field of vision."

```
Context window = 4,000 tokens:
  Model can look at ~3,000 words at once
  Your input + its output must fit within this

Context window = 128,000 tokens:
  Model can look at ~96,000 words at once
  Can handle much longer conversations/documents
```

```
Small context window:
  Long conversation → forgets the beginning
  "What did we talk about 2 hours ago?" → "I don't know"

Large context window:
  Long conversation → remembers everything
  "What did we talk about 2 hours ago?" → "You asked about RNNs"
```

---

## 📊 Maximum Tokens

Upper limit on total tokens — either input or output.

```
Max tokens = 4,096:
  Your input (prompt): 3,000 tokens
  Remaining for output: 1,096 tokens
  
  If your input is too long → won't fit
  If you want a long response → might get cut off
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│            LLM KEY TERMS — QUICK RECALL                      │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  Tokens:     words → numbers                                 │
│  Embeddings: tokens → meaning vectors (similar = close)      │
│                                                              │
│  Top P:      probability threshold for candidates            │
│              higher = more random                            │
│                                                              │
│  Top K:      fixed number of candidates                      │
│              higher = more random                            │
│                                                              │
│  Temperature: randomness in picking from candidates          │
│              0 = always pick highest probability             │
│              1.0 = very random                               │
│                                                              │
│  Context window: how many tokens model can see at once       │
│              bigger = handles longer text                     │
│                                                              │
│  Max tokens: upper limit on input + output length            │
│                                                              │
│  EXAM TIPS:                                                  │
│  "More creative output?"     → higher temperature/Top P/Top K│
│  "Consistent output?"        → lower temperature (0)         │
│  "Handle longer documents?"  → larger context window         │
│  "Output getting cut off?"   → increase max tokens           │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
