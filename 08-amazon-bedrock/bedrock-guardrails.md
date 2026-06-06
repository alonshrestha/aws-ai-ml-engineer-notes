# Amazon Bedrock Guardrails

## 🛡️ What Are Guardrails?

Content filtering for both inputs (prompts) AND outputs (responses). Makes sure nothing bad goes in or comes out of your AI system.

```
User input → [GUARDRAIL: is this okay?] → Model → Response → [GUARDRAIL: is this okay?] → User

Filters BOTH directions:
  → What users say TO the model (input)
  → What the model says BACK (output)
```

---

## 🔧 What Guardrails Can Filter

### 1. Topic Filtering

Block entire topics you don't want discussed:

```
→ "Don't talk about competitors"
→ "Don't discuss politics or religion"
→ "Don't give medical advice"
→ "Don't discuss internal company secrets"
```

### 2. Content Filtering (Automatic)

Filter based on how objectionable content is:

```
Dimensions:
  → Hate speech
  → Bias
  → Violence
  → Sexual content
  → Profanity (built-in filter, just turn it on)

Set thresholds: low, medium, high sensitivity
```

### 3. Word-Level Filtering

Block specific words or phrases.

### 4. PII Removal

Automatically detect and remove personally identifiable information:

```
Can detect and remove/mask:
  → Phone numbers
  → Social Security numbers
  → Driver's license numbers
  → Addresses
  → Email addresses
  → Names

Options:
  → Remove entirely (strip it out)
  → Mask it: "Contact [ADDRESS] for more info"
```

### 5. Contextual Grounding Check (Prevents Hallucinations)

Measures two things:

```
GROUNDING:
  "Is the response based on the actual documents I provided?"
  
  High grounding = answer came from your data ✅
  Low grounding  = model made stuff up ❌ → filter it out

RELEVANCE:
  "Is the response actually answering the question asked?"
  
  High relevance = answer matches the question ✅
  Low relevance  = answer is off-topic ❌ → filter it out
```

```
You set thresholds:
  Grounding < 0.7?  → block the response
  Relevance < 0.6?  → block the response

If blocked → show custom message to user
```

⚠️ Grounding is only as good as your vector store. If your RAG retrieves irrelevant chunks, grounding metrics won't help much.

---

## 📋 Where Guardrails Apply

```
Works with:
  ✅ Text foundation models (Titan, Claude, etc.)
  ✅ Knowledge Bases (RAG)
  ✅ Agents
  ❌ Image models (not yet)

Attach a guardrail → automatically applied to all prompts and responses
```

---

## 💬 Custom Blocked Message

When a guardrail blocks something, you control what the user sees:

```
Default: "I can't help with that."
Custom:  "I'm sorry, I can only help with product-related questions. 
          Please contact support for other inquiries."
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│          BEDROCK GUARDRAILS — QUICK RECALL                   │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  What: Content filtering for inputs AND outputs              │
│                                                              │
│  Filters:                                                    │
│  → Topics (block entire subjects)                            │
│  → Content (hate, bias, violence — with thresholds)          │
│  → Words (specific terms)                                    │
│  → Profanity (built-in, just enable)                         │
│  → PII (remove or mask personal info)                        │
│                                                              │
│  Contextual grounding check:                                 │
│  → Grounding: is response based on provided data?            │
│  → Relevance: does response match the question?              │
│  → Set thresholds, block if too low                          │
│  → Helps prevent hallucinations                              │
│                                                              │
│  Works with: text models, knowledge bases, agents            │
│  NOT with: image models (yet)                                │
│                                                              │
│  EXAM TIPS:                                                  │
│  "Filter offensive content?"       → Guardrails              │
│  "Remove PII from responses?"      → Guardrails (PII filter) │
│  "Prevent hallucinations?"         → Contextual grounding    │
│  "Block specific topics?"          → Guardrails (topic filter)│
│  "Custom message when blocked?"    → Guardrails config       │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
