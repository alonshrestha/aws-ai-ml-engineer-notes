# SageMaker BlazingText — Beginner Friendly Guide

## 🧠 What Is BlazingText?

Don't let the fancy name fool you — it does only TWO things:

```
1. Text Classification → predict a label for a sentence
2. Word2Vec           → find words that are similar to each other
```

That's it. It's not some massive NLP powerhouse.

---

## 📝 Thing 1: Text Classification (Supervised)

You give it sentences with labels. It learns. Then it can label new sentences.

```
TRAINING (you provide these):
  "I love this movie"          → label: POSITIVE
  "This was terrible"          → label: NEGATIVE
  "Great product, works well"  → label: POSITIVE

PREDICTION (it figures this out):
  "Amazing experience"         → label: POSITIVE ✅
```

### Use cases:
- Web search (categorize search results)
- Information retrieval (find relevant content)

### ⚠️ Important Limitation:

```
✅ Works on SENTENCES
❌ Does NOT work on entire documents

"I love this movie" → ✅ fine (sentence)
A 10-page article   → ❌ nope (document)
```

---

## 🔤 Thing 2: Word2Vec (Word Embeddings)

This turns words into numbers (vectors) where **similar words end up close together**.

```
Imagine a map where words are placed by meaning:

        king ●
                    ● queen
        man ●
                    ● woman

        car ●
        truck ●
                              ● banana
                              ● apple

Similar words = close together on the map
Different words = far apart
```

### Why is this useful?

By itself, not that useful. But OTHER algorithms use it as a building block:

```
Word2Vec creates the word map
        ↓
Machine translation uses that map to understand word relationships
Sentiment analysis uses that map to understand word meanings
```

### ⚠️ Important Limitation:

```
✅ Works on individual WORDS
❌ Does NOT work on sentences or documents

It just maps words to vectors. That's all.
```

---

## 📥 Input Format

### For Text Classification (supervised mode):

Each line = one sentence, starting with the label:

```
__label__4 linux ready for prime time , says big blue .
__label__2 this movie was not very good .
__label__1 terrible service , would not recommend .
```

Notice:
- Label format: `__label__` followed by the label number
- Everything is **lowercase**
- Punctuation has **spaces around it** (`, .` not `,` `.`)
- Each word separated by spaces (tokenized)

Also accepts **augmented manifest text format**:
```json
{"source": "linux ready for prime time , says big blue .", "label": 4}
```

### For Word2Vec mode:

Just plain text, one sentence per line:
```
the cat sat on the mat
i love machine learning
```

---

## 🔄 Word2Vec Has Three Modes

### 1. CBOW (Continuous Bag of Words)

```
"Bag of words" = word ORDER doesn't matter

"the cat sat on mat" 
  is treated same as
"mat on sat cat the"

It just cares WHICH words appear, not the order.
Like throwing words into a bag and shaking it.
```

### 2. Skip-Gram

```
Word order DOES matter here.
Looks at n-grams (groups of words in sequence).

"the cat sat" → learns "cat" often appears between "the" and "sat"
```

### 3. Batch Skip-Gram

```
Same as Skip-Gram BUT can be distributed across multiple CPU machines.
Good when you need to scale out.
```

---

## 🎛️ Key Hyperparameters

### For Word2Vec:

| Parameter | What It Does |
|-----------|-------------|
| `mode` | cbow, skipgram, or batch_skipgram |
| `learning_rate` | How fast it learns |
| `window_size` | How many nearby words to consider |
| `vector_dim` | Size of the word vectors (bigger = more detail) |
| `negative_samples` | Helps training efficiency |

### For Text Classification:

| Parameter | What It Does |
|-----------|-------------|
| `epochs` | How many passes through training data |
| `learning_rate` | How fast it learns |
| `word_ngrams` | How many words to look at together |
| `vector_dim` | Size of the word vectors |

---

## 💻 What Machines to Use?

This depends on which mode you're using:

### Word2Vec — CBOW and Skip-Gram:

```
✅ Single CPU or single GPU
✅ P3 recommended (takes advantage of GPU)
❌ Single machine only — cannot scale across machines
```

### Word2Vec — Batch Skip-Gram:

```
✅ Multiple CPU instances (can scale horizontally!)
❌ No GPU — CPU only
```

```
Need to scale Word2Vec?
  → Use Batch Skip-Gram mode (multiple CPUs)
  
Don't need to scale?
  → Use CBOW or Skip-Gram (single GPU, faster)
```

### Text Classification:

```
Small data (< 2 GB):
  ✅ C5 instance (CPU, cheaper)

Large data (> 2 GB):
  ✅ P2.xlarge or P3.2xlarge (GPU, more power)
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────┐
│             BLAZINGTEXT — QUICK RECALL                   │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  Does what?     1. Text Classification (label sentences) │
│                 2. Word2Vec (find similar words)          │
│                                                          │
│  Limitations:   Text Classification → sentences only     │
│                 Word2Vec → words only                     │
│                 NOT for full documents!                   │
│                                                          │
│  Input format:  Classification: __label__X sentence      │
│                 Word2Vec: one sentence per line           │
│                 Everything lowercase, tokenized           │
│                                                          │
│  Word2Vec       CBOW: word order doesn't matter          │
│  modes:         Skip-Gram: word order matters             │
│                 Batch Skip-Gram: like skip-gram but       │
│                   can scale across multiple CPUs          │
│                                                          │
│  Instances:     CBOW/Skip-Gram: single GPU (P3)          │
│                 Batch Skip-Gram: multi CPU (no GPU)       │
│                 Classification < 2GB: C5 (CPU)            │
│                 Classification > 2GB: P2/P3 (GPU)         │
│                                                          │
│  Key point:     Word2Vec is a BUILDING BLOCK for other   │
│                 NLP algorithms, not useful alone          │
│                                                          │
└──────────────────────────────────────────────────────────┘
```
