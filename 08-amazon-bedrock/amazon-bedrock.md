# Amazon Bedrock

## 🤔 What Is Bedrock?

A serverless API for using foundation models. Think of it as a marketplace + unified API for generative AI on AWS.

```
Without Bedrock:
  Find a model → download → set up servers → deploy → manage
  (lots of infrastructure work)

With Bedrock:
  Pick a model → call the API → get results
  (serverless, no infrastructure to manage)
```

---

## 🏪 Foundation Models Available

```
ON Bedrock:
  Amazon Titan    → Amazon's own (text, summarization, embeddings)
  Claude          → Anthropic (conversations, Q&A) ← Amazon invested heavily
  Llama           → Meta (text generation)
  Stable Diffusion→ Stability AI (image generation)
  Jurassic        → AI21 Labs (multilingual text)
  + more being added

NOT on Bedrock:
  GPT/DALL-E      → OpenAI (Microsoft competitor)
  Gemini          → Google (competitor)
```

You can also bring your own models.

---

## 🔌 Four API Endpoints

```
1. Bedrock API
   → Manage, deploy, train/fine-tune models
   → "Set everything up before production"

2. Bedrock Runtime
   → Perform inference (get responses from models)
   → invoke_model → one-shot request/response
   → converse → chat-style interaction
   → Stream versions → word-by-word response (like ChatGPT)
   → Generate embeddings, generate images

3. Bedrock Agent
   → Manage and deploy agents + knowledge bases
   → "Set up agents before production"

4. Bedrock Agent Runtime
   → Use agents and RAG in production
   → invoke_agent → run an agent
   → retrieve → search knowledge base
   → retrieve_and_generate → search + generate response
```

```
Bedrock         = setup (models)
Bedrock Runtime = use (models)
Bedrock Agent         = setup (agents)
Bedrock Agent Runtime = use (agents)
```

---

## 🤖 What Are Agents? (Quick Preview)

Give your LLM tools to do things it can't do alone.

```
LLM alone:
  "What's the weather today?"
  "I don't know, I'm just a language model"

LLM + Agent with weather tool:
  "What's the weather today?"
  → Agent calls weather API → gets data
  "It's 72°F and sunny in Seattle"
```

Agents connect foundation models to YOUR data, YOUR services, YOUR APIs. More on this in the RAG section.

---

## 🔐 Security & Access

### Must use IAM user (NOT root account):

```
Root account → ❌ won't work with Bedrock
IAM user     → ✅ required

Permissions needed:
  → AmazonBedrockFullAccess
  → AmazonBedrockReadOnly
  → Or Administrator access (includes everything)
```

### Must request model access first:

```
Before using any model → request access in console

Amazon Titan:    approved immediately ✅
Third-party models: takes a few minutes
                    Must agree to their license terms (EULA)
```

```
⚠️ Request access BEFORE building your application
   Otherwise you'll get errors when trying to use the model
```

---

## 💰 Pricing

```
→ Third-party models set their own pricing
→ Billed through AWS (one bill, not separate)
→ Check pricing BEFORE requesting access
→ aws.amazon.com/bedrock/pricing

Some models are cheap, some are expensive.
Know what you're getting into.
```

---

## 🎮 Playgrounds

Bedrock provides quick ways to experiment without writing code:

```
Chat playground:       → chat with a model (like ChatGPT)
Text playground:       → generate/summarize text
Image playground:      → generate images

Also works with:
  → Your custom models
  → Imported models
```

---

## 🔗 Bedrock vs SageMaker

```
Bedrock:
  → Serverless (no infrastructure)
  → API-based (just call it)
  → For using foundation models
  → Simpler, faster to start

SageMaker:
  → Full control over infrastructure
  → Build/train your own models
  → More complex, more flexible
  → Can integrate Bedrock underneath

Bedrock = easy button for GenAI
SageMaker = full ML workshop
```

Bedrock integrates with SageMaker Canvas — so you can use Bedrock models within SageMaker too.

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│            AMAZON BEDROCK — QUICK RECALL                     │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  What: Serverless API for foundation models                  │
│  Think: Marketplace + unified API for GenAI                  │
│                                                              │
│  Models: Titan, Claude, Llama, Stable Diffusion, Jurassic    │
│  NOT: GPT, DALL-E, Gemini (competitors)                      │
│                                                              │
│  Four APIs:                                                  │
│  → Bedrock: manage/train models                              │
│  → Bedrock Runtime: inference (use models)                   │
│  → Bedrock Agent: manage agents + knowledge bases            │
│  → Bedrock Agent Runtime: use agents + RAG                   │
│                                                              │
│  Security:                                                   │
│  → Must use IAM user (not root)                              │
│  → Must request model access first                           │
│  → Titan = instant approval, others = few minutes            │
│                                                              │
│  Pricing:                                                    │
│  → Third parties set their own prices                        │
│  → Billed through AWS                                        │
│  → Check pricing before using!                               │
│                                                              │
│  EXAM TIPS:                                                  │
│  "Serverless GenAI on AWS?"       → Bedrock                  │
│  "Use foundation models via API?" → Bedrock Runtime          │
│  "Amazon's own LLM?"             → Titan (on Bedrock)        │
│  "Can't use root account?"       → Bedrock requires IAM user │
│  "Bedrock vs SageMaker?"         → Bedrock = simple/serverless│
│                                   SageMaker = full control   │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
