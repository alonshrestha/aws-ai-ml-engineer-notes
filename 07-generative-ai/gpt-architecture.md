# GPT — How It Actually Works

## 🧠 What Is GPT?

GPT = Generative Pre-trained Transformer

```
Generative   = generates new text
Pre-trained  = already trained on massive data
Transformer  = the architecture (what you just learned)
```

GPT is just a transformer, but a specific version — decoder blocks only, stacked on top of each other.

---

## 🏗️ Architecture: Decoder Only

The original transformer had encoder + decoder. GPT simplified it.

```
Original transformer:     GPT:
  Encoder                   (no encoder)
     ↓
  Decoder                   Decoder block 1
                            Decoder block 2
                            Decoder block 3
                            ...
                            Decoder block N
                            (just stacked decoders)
```

Why no encoder? The decoder's self-attention already understands the input — it does both jobs (understanding + generating) in one block. No need for a separate encoder.

```
Other models for reference:
  GPT  = decoder only  (generates text)
  BERT = encoder only  (understands text)
  T5   = encoder + decoder (both)
```

---

## 🔄 How GPT Generates Text

There's no real "input" and "output." It's one continuous sequence. GPT just predicts the next token over and over.

```
Feed in:  "Please translate me [GO]"
Get back: "Please translate me [GO] I"

Feed in:  "Please translate me [GO] I"
Get back: "Please translate me [GO] I go"

Feed in:  "Please translate me [GO] I go"
Get back: "Please translate me [GO] I go home"

Each pass → one new token added → feed it back → repeat
```

```
GPT doesn't know what's "input" vs "output."
It just sees one long sequence of tokens 
and predicts what comes next. That's it.
```

---

## 📚 How GPT Is Trained (No Labels Needed!)

This is the clever part. You don't need labeled data. Just throw text at it.

```
Training data: "The cat sat on the mat"

GPT learns by predicting next word:
  Input: "The"           → should predict: "cat"
  Input: "The cat"       → should predict: "sat"
  Input: "The cat sat"   → should predict: "on"
  Input: "The cat sat on"→ should predict: "the"

The text IS the answer key.
No human needs to label anything.
```

```
Feed it Wikipedia → it learns facts
Feed it Reddit    → it learns conversation
Feed it books     → it learns storytelling
Feed it code      → it learns programming

It's not learning a specific task.
It's learning the LANGUAGE itself.
```

That's why it's called "pre-trained" — trained on massive text before being used for any specific task.

---

## 🧱 Inside Each Decoder Block

Each decoder block has two parts, stacked:

```
┌─────────────────────────┐
│  Masked Self-Attention   │  ← find word connections
├─────────────────────────┤
│  Feedforward Neural Net  │  ← process and predict
└────────────┬────────────┘
             ↓
┌─────────────────────────┐
│  Masked Self-Attention   │  ← find deeper connections
├─────────────────────────┤
│  Feedforward Neural Net  │  ← process more
└────────────┬────────────┘
             ↓
        ... repeat N times ...
```

```
GPT-2:  12 decoder blocks stacked
GPT-3:  96 decoder blocks stacked
GPT-4:  rumored 1 trillion+ parameters

More blocks = deeper understanding = better predictions
But also = more expensive to train and run
```

---

## 📥 Input Processing (Three Steps)

### Step 1: Tokenization

Words → numbers. Neural networks only work with numbers.

```
"Please translate me" → [5765, 15772, 502]

Some words = one token:
  "translate" → [15772]

Some words = multiple tokens:
  "indivisible" → [521, 452, 1253]  (ind + ivis + ible)

Also:
  Common patterns become one token: "123" → single token
  Punctuation = its own token: "." → [13]
  Apostrophe t = its own token: "'t" → [470]
```

### Step 2: Token Embedding

Tokens → vectors. Similar words are close together in this space.

```
Each token becomes a vector of thousands of numbers:
  "potato"   = [0.2, 0.8, 0.1, 0.5, ...]  (1000+ numbers)
  "rhubarb"  = [0.3, 0.7, 0.2, 0.4, ...]  ← close to potato (both food)
  "starship" = [0.9, 0.1, 0.8, 0.2, ...]  ← far from potato (not food)

Distance between vectors = similarity in meaning
  potato ↔ rhubarb  = close (similar meaning)
  potato ↔ starship = far (different meaning)
```

### Step 3: Positional Encoding

Since everything is parallel, the model doesn't know word order. Positional encoding adds that info.

```
Without position: "dog bites man" = "man bites dog" (same tokens!)
With position:    each token gets its position baked in

Uses sine/cosine waves:
  → Within a window (~100 tokens), position is unique
  → Scales to any length of text
  → Alternates between sine and cosine values
```

```
Full input processing:

  "Please translate me"
       ↓
  Tokenize: [5765, 15772, 502]
       ↓
  Embed: [[0.2, 0.8, ...], [0.5, 0.3, ...], [0.1, 0.9, ...]]
       ↓
  Add position: [[0.2+pos1, 0.8+pos1, ...], ...]
       ↓
  Ready for self-attention
```

---

## 📤 Output Stage: How GPT Picks the Next Word

After all the decoder blocks process the input, a vector comes out the top. But it's not a word yet — it's just numbers. Here's how it becomes a word:

```
Final decoder block outputs: [0.7, 0.2, 0.9, 0.4, ...]
                                    ↓
Multiply with token embeddings matrix
                                    ↓
Get probabilities for EVERY possible word:

  "incredible" = 35%
  "amazing"    = 25%
  "great"      = 20%
  "hard"       = 10%
  "boring"     = 5%
  ...50,000 other words with tiny %...
                                    ↓
Pick one based on temperature setting
```

### Temperature: The Randomness Dial

```
Temperature = 0 (no randomness):
  ALWAYS picks highest probability
  "incredible" (35%) ← always this one
  Same input = same output every time

Temperature = 0.7 (some randomness):
  Usually picks high probability, sometimes surprises
  Might pick "amazing" (25%) instead
  Same input = slightly different outputs

Temperature = 1.0+ (high randomness):
  Could pick anything, even low probability words
  Might pick "hard" (10%) 
  Same input = very different outputs each time
```

```
Low temperature  = predictable, safe, repetitive
High temperature = creative, varied, sometimes weird
```

### When to use what:

```
Want factual answers?     → low temperature (consistent)
Want creative writing?    → high temperature (varied)
Want brainstorming ideas? → high temperature (different each time)
```

---

## 🔄 The Complete GPT Flow (End to End)

```
"What is this course about?"
         ↓
  1. Tokenize: words → token IDs
         ↓
  2. Embed: token IDs → vectors (meaning)
         ↓
  3. Positional encoding: add word order
         ↓
  4. Decoder blocks (stacked):
     [masked self-attention → feedforward NN] × N
         ↓
  5. Output vector comes out
         ↓
  6. Multiply with embedding matrix → probabilities
         ↓
  7. Pick next token (based on temperature)
         ↓
  8. Output: "incredible" 
         ↓
  9. Add to sequence, repeat from step 1
     "What is this course about? incredible ___"
         ↓
  ...until done...
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│              GPT ARCHITECTURE — QUICK RECALL                 │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  GPT = Generative Pre-trained Transformer                    │
│  → Decoder only (no encoder)                                 │
│  → Stacked decoder blocks                                    │
│  → Each block: masked self-attention + feedforward NN        │
│                                                              │
│  How it works:                                               │
│  → Predicts next token, over and over                        │
│  → No concept of input vs output (one sequence)              │
│  → Trained on unlabeled text (self-supervised)               │
│                                                              │
│  Input processing:                                           │
│  → Tokenization: words → numbers                             │
│  → Embedding: numbers → vectors (similar = close)            │
│  → Positional encoding: adds word order (sine/cosine)        │
│                                                              │
│  Scale:                                                      │
│  → GPT-2: open source, available on Hugging Face             │
│  → GPT-3: 175 billion parameters                             │
│  → GPT-4: rumored 1 trillion+ parameters                    │
│                                                              │
│  Comparison:                                                 │
│  → GPT = decoder only (generates)                            │
│  → BERT = encoder only (understands)                         │
│  → T5 = encoder + decoder (both)                             │
│                                                              │
│  EXAM TIPS:                                                  │
│  "Decoder only transformer?"     → GPT                       │
│  "Encoder only transformer?"     → BERT                      │
│  "No labeled data needed?"       → Self-supervised (GPT)     │
│  "Open source GPT on AWS?"       → GPT-2 via Hugging Face    │
│  "Positional encoding method?"   → Sine/cosine waves         │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
