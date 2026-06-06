# Self-Attention & How It Actually Works

## 🎯 What Self-Attention Does

Takes a word and figures out its TRUE meaning based on the words around it.

```
"I read a good novel"        → novel = a book
"Attention is a novel idea"  → novel = something new/original

Same word. Different meaning. Self-attention figures out which one.

How?
  Sentence 1: "novel" pays attention to "read" → book
  Sentence 2: "novel" pays attention to "idea" → original
```

After self-attention, each word's token gets UPDATED to represent its real meaning in context — not just the word itself.

---

## 🔑 The Three Matrices: Query, Key, Value (Q, K, V)

Every token gets three vectors, computed from three learned matrices:

```
Think of it like a search engine:

Query (Q) = "What am I looking for?"
Key (K)   = "What do I contain?"
Value (V) = "What's my actual content?"
```

Real example:

```
Processing the word "novel" in "I read a good novel"

Query of "novel": "What words help define my meaning?"
Key of "read":    "I'm about reading"
Key of "idea":    "I'm about concepts"

novel's Query × read's Key = HIGH score (reading + novel = book!)
novel's Query × idea's Key = LOW score (not in this sentence)

The Value of the high-scoring words gets used to update "novel"
```

---

## 📐 How Scores Are Computed

```
Step 1: Each token gets Q, K, V vectors
        (embedding × learned matrices)

Step 2: Multiply Query of current word with Key of every other word
        (this is the dot product — measures similarity)

Step 3: Apply softmax to normalize scores (all add up to 1)

Step 4: Multiply scores by Value vectors

Step 5: Add them all up → new embedding (Z) for that word
```

```
"I read a good novel"

Processing "novel":
                    Score (Q×K)    After softmax
  "I"               0.02           2%
  "read"            0.65           40%    ← high attention!
  "a"               0.01           1%
  "good"            0.22           17%
  "novel"           0.10           40%

  Multiply each score × that word's Value vector
  Add them all up
  → Z vector for "novel" = meaning closer to "book"
```

### Why "scaled dot product attention"?

```
Scaled    = divide by a number to keep values stable
Dot product = the math operation used to compare Q and K
Attention = the whole mechanism

It's just the technical name for this process.
```

---

## 🎭 Masked Self-Attention

Prevents words from seeing FUTURE words. Only looks backward.

```
Sentence: "a good novel"

Processing "a":
  Can see: "a" ✅
  Can't see: "good", "novel" ❌ (future words masked)

Processing "good":
  Can see: "a", "good" ✅
  Can't see: "novel" ❌ (future word masked)

Processing "novel":
  Can see: "a", "good", "novel" ✅
  Nothing masked (it's the last word)
```

```
Why? Because when generating text, you don't know 
what comes next. You can only use what came before.

This happens in PARALLEL but conceptually mimics 
reading left to right.
```

### Who uses what:

```
GPT:  Uses masked self-attention (decoder only — generates text)
BERT: Uses regular self-attention (encoder only — understands text)
      Can look at words in BOTH directions
```

---

## 🧠 Multi-Headed Self-Attention

Instead of one attention calculation, do MANY in parallel.

```
Single head:
  One Q, K, V calculation → one perspective on word relationships

Multi-head (say 8 heads):
  Head 1: focuses on grammar relationships
  Head 2: focuses on meaning relationships
  Head 3: focuses on position relationships
  Head 4: focuses on subject-object relationships
  ... and so on

  Each head learns different types of connections.
  Combine all heads → richer understanding.
```

```
Like asking 8 different experts to analyze the same sentence:
  Expert 1: "novel relates to read (it's a book)"
  Expert 2: "novel relates to good (it's a good one)"
  Expert 3: "novel is the object of the sentence"
  
  Combine all opinions → much better understanding
```

### How it works technically:

```
Q, K, V vectors are SPLIT into rows
Each row = one "head"
Each head processes in parallel
Results combined at the end

Just a way to further parallelize the attention computation.
```

---

## 🔄 The Full Flow

```
Input: "I read a good novel"
         ↓
  [Convert words to embeddings (numbers)]
         ↓
  [Add positional encoding (word order)]
         ↓
  ┌─────────────────────────────────┐
  │  SELF-ATTENTION                 │
  │                                 │
  │  Each word:                     │
  │  1. Gets Q, K, V vectors       │
  │  2. Compares with all others    │
  │  3. Gets attention scores       │
  │  4. Updates its embedding       │
  │                                 │
  │  "novel" → now means "book"     │
  │  (updated based on context)     │
  └─────────────┬───────────────────┘
                ↓
  [Updated embeddings (Z vectors)]
                ↓
  [Feedforward neural network]
  (the massive layer with billions of parameters)
                ↓
  Output
```

---

## 🏗️ GPT vs BERT Architecture

```
GPT:
  → Decoder only (stacks of decoders)
  → Masked self-attention (can't see future)
  → Good for: generating text
  → "Predict the next word"

BERT:
  → Encoder only (stacks of encoders)
  → Regular self-attention (sees all directions)
  → Good for: understanding text
  → "What does this sentence mean?"
  → Uses masked language modeling (randomly masks words)
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│          SELF-ATTENTION — QUICK RECALL                        │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  Purpose: Find relationships between words                   │
│           Update word meaning based on context               │
│                                                              │
│  Q, K, V:                                                    │
│  → Query: "What am I looking for?"                           │
│  → Key: "What do I contain?"                                 │
│  → Value: "What's my actual content?"                        │
│  → All learned through training (backpropagation)            │
│                                                              │
│  Process: Q × K → scores → softmax → × V → sum → Z          │
│  Name: Scaled dot product attention                          │
│                                                              │
│  Masked self-attention:                                      │
│  → Can't see future words                                    │
│  → Used in GPT (decoder)                                     │
│                                                              │
│  Multi-headed:                                               │
│  → Multiple attention calculations in parallel               │
│  → Each head learns different relationships                  │
│  → More heads = richer understanding                         │
│                                                              │
│  GPT = decoder only (generates text)                         │
│  BERT = encoder only (understands text)                      │
│                                                              │
│  EXAM TIPS:                                                  │
│  "How does transformer understand context?" → Self-attention │
│  "Q, K, V matrices?"        → Query, Key, Value             │
│  "Can't see future tokens?" → Masked self-attention (GPT)    │
│  "Parallel attention?"      → Multi-headed self-attention    │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
