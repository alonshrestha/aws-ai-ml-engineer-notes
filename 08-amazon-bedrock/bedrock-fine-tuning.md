# Fine-Tuning in Amazon Bedrock (Custom Models)

## 🤔 What Is Fine-Tuning in Bedrock?

Take a foundation model (Titan, Claude, Llama) and train it further with YOUR data. The knowledge gets baked INTO the model permanently.

```
Foundation model: "I know general English"
After fine-tuning: "I know general English AND your company's style/data"
```

---

## 💡 Why Fine-Tune Instead of Other Approaches?

```
Prompt engineering:
  → Stuff examples into every prompt
  → Uses lots of tokens every time (expensive per request)
  → "Here are 10 examples, now answer like this..."

RAG:
  → Search docs at runtime
  → Adds tokens to every request (search results injected)
  → Good for live data, but costs per request

Fine-tuning:
  → Expensive ONCE (training cost)
  → But then every request is cheap (no extra tokens needed)
  → Knowledge is baked in permanently
```

```
Fine-tuning = expensive upfront, cheap long-term
Prompt engineering / RAG = cheap upfront, expensive per request
```

---

## 📋 Use Cases

```
→ Chatbot with specific personality/tone
→ Ads written in your brand's voice
→ Clone of your writing style (trained on your emails)
→ Customer support (trained on past transcripts)
→ Classification tasks (trained on labeled examples)
→ Newer data than the model's training cutoff
→ Proprietary/domain-specific knowledge
```

---

## 🔧 Two Types of Fine-Tuning in Bedrock

### 1. Custom Model (Labeled Data)

You provide prompt + completion pairs (question + answer examples):

```json
{"prompt": "What is your return policy?", 
 "completion": "We offer 30-day returns with receipt, matey! Arrr!"}

{"prompt": "How do I track my order?", 
 "completion": "Head to yer account page and click Track Order, ye scallywag!"}
```

```
→ Labeled: you give it the question AND the answer
→ Model learns the STYLE and CONTENT of your answers
→ Upload to S3 → tell Bedrock to create custom model
→ Can fine-tune text models AND image models
```

For image models:
```
→ Provide prompts + links to images in S3
→ Fine-tune text-to-image or image-to-embedding models
```

### 2. Continued Pre-Training (Unlabeled Data)

Just throw raw text at it. No question/answer pairs needed.

```
Just plain text documents:
  "Our company was founded in 2010. We specialize in 
   cloud consulting and have offices in 5 countries.
   Our refund policy allows returns within 30 days..."
```

```
→ Unlabeled: just raw text, no prompt/completion pairs
→ Model absorbs the information into its weights
→ Like GPT training (text is its own answer key)
→ Good for: business documents, manuals, knowledge bases
```

### When to use which:

```
Have question/answer examples? → Custom Model (labeled)
Just have raw documents?       → Continued Pre-Training (unlabeled)
```

---

## 🔐 Security: Fine-Tuning with Sensitive Data

```
Training with proprietary/sensitive data?
→ Use VPC with PrivateLink

This keeps your training data inside your private network.
Data never crosses the public internet.

⚠️ Exam might ask: "How to fine-tune securely?"
   Answer: VPC + PrivateLink
```

---

## ⚠️ Limitations

```
→ Not all models support fine-tuning (check model card)
→ Expensive (lots of computation)
→ Time-consuming (can take hours)
→ Supported models: Titan, Cohere, Meta (more coming)
```

---

## 🔄 After Fine-Tuning

```
→ Get a new model identifier (custom model ID)
→ Use it exactly like any other model via Bedrock Runtime
→ Same API calls (invoke_model, converse, etc.)
→ Can fine-tune a fine-tuned model again (keep improving)
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│       BEDROCK FINE-TUNING — QUICK RECALL                     │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  What: Extend foundation model with your data                │
│  Result: Custom model (new model ID, use like any other)     │
│                                                              │
│  Two types:                                                  │
│  → Custom Model: labeled (prompt + completion pairs)         │
│  → Continued Pre-Training: unlabeled (raw text)              │
│                                                              │
│  Data format: JSON with prompt/completion → upload to S3     │
│  Also works with: image models (prompts + image links in S3) │
│                                                              │
│  Security: VPC + PrivateLink for sensitive data              │
│                                                              │
│  Supported: Titan, Cohere, Meta (not all models)             │
│  Can re-fine-tune: yes, keep improving over time             │
│                                                              │
│  Cost: expensive upfront, but saves tokens long-term         │
│  Time: can take hours                                        │
│                                                              │
│  EXAM TIPS:                                                  │
│  "Bake company data into model?"    → Fine-tuning            │
│  "Secure fine-tuning?"              → VPC + PrivateLink      │
│  "Labeled data (Q&A pairs)?"        → Custom Model           │
│  "Unlabeled raw text?"              → Continued Pre-Training │
│  "Save tokens long-term?"           → Fine-tuning > RAG      │
│  "Use custom model after training?" → Same API, new model ID │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
