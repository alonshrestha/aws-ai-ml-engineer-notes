# Foundation Models on AWS

## 🤔 What Is a Foundation Model?

A giant pre-trained transformer that you fine-tune for your specific task. You already know this — it's the base model that knows language, and you build on top of it.

```
Foundation model = knows how to speak
Fine-tuning      = teaches it what to say for YOUR use case
```

---

## 🏢 Who Makes What (And What's on AWS)

### NOT on AWS (competitors):

```
GPT (OpenAI)     → funded by Microsoft (Azure) → competitor ❌
BERT (Google)     → Google Cloud → competitor ❌
DALL-E (OpenAI)   → funded by Microsoft → competitor ❌
Gemini (Google)   → Google Cloud → competitor ❌
```

### Available on AWS:

```
Claude (Anthropic)        → conversations, Q&A, workflows
                            Amazon invested in Anthropic ✅

Llama (Meta)              → text generation
                            Meta has no cloud platform → no conflict ✅

Stable Diffusion          → image generation
(Stability AI)              Open/independent ✅

Amazon Titan              → Amazon's own foundation model
                            Text generation, summarization, 
                            Q&A, embeddings ✅

Jurassic (AI21 Labs)      → multilingual text generation
                            Spanish, French, German, etc. ✅

Hugging Face models       → GPT-2, GPT-J, Falcon, Bloom, etc.
                            Open source, integrated into AWS ✅
```

---

## 🏗️ Amazon Titan — AWS's Own Model

```
What it does:
  → Text generation
  → Summarization
  → Question answering
  → Embeddings (similarity between words/phrases)

Embeddings use case:
  → Search: "Find documents similar to my query"
  → Personalization: "Recommend things similar to what you like"
  → Just computing how close two things are in meaning
```

```
Embedding example:
  Search query: "how to train a dog"
  
  Titan converts to vector → finds closest vectors in your data:
    "puppy training tips"     → very close ✅
    "dog obedience classes"   → close ✅
    "cat food recipes"        → far ❌
```

---

## 🚀 SageMaker JumpStart

A quick way to start using foundation models in SageMaker. Pre-built notebooks ready to go.

```
Without JumpStart:
  Find model → download → configure → write code → load → test
  (hours of setup)

With JumpStart:
  Click a model → notebook opens → already loaded → start playing
  (minutes)
```

### What's available in JumpStart:

```
Foundation Models:
  → Claude, Llama, Stable Diffusion, Amazon Titan
  → Jurassic, Alexa model

Hugging Face models:
  → GPT-2, GPT-J, Falcon, Bloom, Flan
  → Thousands of open source models

Each comes with a pre-populated notebook:
  → How to load the model
  → How to feed data
  → How to query it
  → How to fine-tune it
  → How to shut it down
```

---

## 🧭 Where This Fits

```
You want to build an AI app:

  Option 1: Build from scratch
    → Train your own transformer
    → Costs millions, takes months
    → Only big companies do this

  Option 2: Fine-tune a foundation model ← most common
    → Pick a model from JumpStart/Bedrock
    → Fine-tune on your data
    → Costs much less, takes hours/days

  Option 3: Use as-is (no fine-tuning)
    → Just send prompts to the model
    → Zero training needed
    → Good for general tasks
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│       FOUNDATION MODELS ON AWS — QUICK RECALL                │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  Foundation model = pre-trained transformer you build on     │
│                                                              │
│  On AWS:                                                     │
│  → Claude (Anthropic) — conversations, Q&A                   │
│  → Llama (Meta) — text generation                            │
│  → Stable Diffusion — image generation                       │
│  → Amazon Titan — text, summarization, embeddings            │
│  → Jurassic (AI21) — multilingual text                       │
│  → Hugging Face models — GPT-2, Falcon, Bloom, etc.          │
│                                                              │
│  NOT on AWS:                                                 │
│  → GPT/DALL-E (OpenAI/Microsoft)                             │
│  → BERT/Gemini (Google)                                      │
│                                                              │
│  SageMaker JumpStart:                                        │
│  → Quick-start notebooks with models pre-loaded              │
│  → Click → notebook opens → start using                      │
│                                                              │
│  Amazon Titan embeddings:                                    │
│  → Search (find similar documents)                           │
│  → Personalization (recommend similar items)                 │
│                                                              │
│  EXAM TIPS:                                                  │
│  "Use foundation models on AWS?"  → JumpStart or Bedrock     │
│  "Amazon's own LLM?"             → Amazon Titan              │
│  "Open source models on AWS?"    → Hugging Face via JumpStart│
│  "Image generation on AWS?"      → Stable Diffusion          │
│  "Multilingual text on AWS?"     → Jurassic (AI21 Labs)      │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
