# SageMaker Seq2Seq (Sequence to Sequence) — Beginner Friendly Guide

## 🧠 What Is Seq2Seq?

Seq2Seq = takes a **sequence in**, gives a **sequence out**.

A "sequence" is just a list of things in order — like words in a sentence.

```
INPUT sequence:  "How are you?"        (English)
OUTPUT sequence: "Comment allez-vous?"  (French)

You gave it a sequence of words → it gave back a sequence of words
```

That's it. Sequence in, sequence out.

---

## 🤔 What Is a "Token"?

Computers don't understand words. They understand numbers. So we convert words to numbers first.

```
Vocabulary file (like a dictionary):
  "how"  → 1
  "are"  → 2
  "you"  → 3
  "hello" → 4
  ...

"How are you?" → [1, 2, 3]   ← these numbers are called TOKENS

The model works with [1, 2, 3] not "How are you?"
```

So tokenizing = converting words (or sounds or anything) into numbers.

---

## ✅ What Can It Do?

```
1. Machine Translation
   "Hello, how are you?" → "Bonjour, comment allez-vous?"
   (English tokens in → French tokens out)

2. Text Summarization
   Long document (1000 words) → Short summary (50 words)
   (Long sequence in → Short sequence out)

3. Speech to Text
   Audio waveform tokens → Text word tokens
   (Sound sequence in → Word sequence out)
```

---

## ⚙️ What's Under the Hood?

It can use either:

```
RNN (Recurrent Neural Network):
  → Processes sequence one step at a time
  → Like reading a sentence word by word

CNN (Convolutional Neural Network) with Attention:
  → Can look at the whole sequence at once
  → Like seeing the whole sentence at a glance

Both work for sequential data. Seq2Seq lets you pick.
```

---

## 📥 Input Format

This one is specific:

```
Format: RecordIO-Protobuf ONLY
Data type: Integers (not floating point!)
           ↑ unusual — most SageMaker algorithms want float numbers

Why integers? Because tokens are whole numbers:
  "hello" = 4  (not 4.0 or 4.5)
  "world" = 7  (not 7.0 or 7.3)
```

### What You Need to Provide:

```
Three things:
1. Training data       → tokenized text files (numbers, not words)
2. Validation data     → to check if translations are correct
3. Vocabulary file     → the dictionary that maps words ↔ numbers

   Vocabulary file example:
   hello → 1
   world → 2
   good  → 3
   ...
```

You can't just throw in a raw text file. You have to tokenize everything first. SageMaker provides sample code to help with this.

---

## 🕐 Training Takes LONG

Translating between languages is hard. Training can take **days**.

```
Why so long?
  → Language is complex
  → Millions of word combinations
  → Model needs to learn grammar, context, meaning
  → Very heavy computation
```

### Good News: Pre-trained Models Exist!

You don't have to train from scratch. People have already trained models for common language pairs.

```
Pre-trained: English ↔ French     ✅ already done
Pre-trained: English ↔ German     ✅ already done
Pre-trained: English ↔ Spanish    ✅ already done

Public datasets also available — no need to build 
your own dictionary of every word in two languages.
```

---

## 🎛️ Key Hyperparameters

Standard neural network settings:

| Parameter | What It Does |
|-----------|-------------|
| `batch_size` | How many examples per training step |
| `optimizer_type` | How the model learns: Adam, SGD, or RMSProp |
| `learning_rate` | How big each learning step is |
| `num_layers` | How many layers in the neural network |

### Special Metrics for Translation:

Regular accuracy doesn't work well for translation because there's no single "correct" answer.

```
English: "I am happy"
Correct French translations:
  → "Je suis heureux"
  → "Je suis content"
  → "Je me sens heureux"
  
All are correct! So simple right/wrong doesn't work.
```

That's why Seq2Seq uses special metrics:

```
BLEU Score:
  → Compares your translation against MULTIPLE correct translations
  → Gives partial credit
  → "You got 80% of the words/phrases right"
  → Standard metric for machine translation

Perplexity:
  → Measures how "confused" the model is
  → Lower = better (less confused)
  → Based on cross-entropy (don't need to know the math)
```

**🎯 Exam tip:** Question about measuring machine translation quality? → Answer is BLEU score or perplexity.

---

## 💻 What Machines to Use?

This is a deep learning algorithm → needs GPU.

```
✅ GPU instances (P3 recommended — beefy!)
✅ Multiple GPUs on ONE machine
❌ Cannot spread across multiple machines

So for big training jobs:
  → Pick ONE powerful machine with LOTS of GPUs
  → Like a P3.16xlarge (8 GPUs on one machine)
  → You can't add more machines, so go big on one
```

```
Compare with others:
  Linear Learner: multi-machine ✅, multi-GPU ❌
  Seq2Seq:        multi-machine ❌, multi-GPU ✅
                  (exact opposite!)
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────┐
│               SEQ2SEQ — QUICK RECALL                     │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  What is it?    Sequence in → Sequence out               │
│                                                          │
│  Common use?    Machine translation (English → French)   │
│                 Text summarization, Speech to text        │
│                                                          │
│  Under hood?    RNN or CNN with attention                │
│                                                          │
│  Input?         RecordIO-Protobuf (integers, not float!) │
│                 + vocabulary file                         │
│                 + tokenized text                          │
│                                                          │
│  Training?      Very slow (days!) — use pre-trained      │
│                 models when possible                      │
│                                                          │
│  Metrics?       BLEU score and Perplexity                │
│                 (not regular accuracy)                    │
│                                                          │
│  Instances?     GPU only — P3 recommended                │
│                 Multi-GPU on one machine ✅               │
│                 Multi-machine ❌                          │
│                                                          │
└──────────────────────────────────────────────────────────┘
```
