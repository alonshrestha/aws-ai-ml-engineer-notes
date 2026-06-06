# Bedrock Additional Features

## 📦 Import Your Own Models

Not limited to Bedrock's marketplace models. You can bring your own.

```
Sources:
  → Import from SageMaker (model you trained there)
  → Import from S3 (model file stored in a bucket)

Use case: You built a custom model and want to use it 
through Bedrock's serverless API instead of managing 
your own SageMaker endpoint.
```

---

## 📊 Model Evaluation

How do you know if one model is better than another? Two approaches:

### Automatic Evaluation

```
Provide: test prompts + ideal responses
Bedrock measures:
  → Accuracy (is the answer correct?)
  → Toxicity (is it offensive?)
  → Robustness (does it handle edge cases?)
  → BERTScore (semantic similarity to ideal response)
  → F1 score

Data sources:
  → Your own test prompts + expected responses
  → Built-in prompt datasets (pre-made standard tests)
  → Some test reasoning ability specifically
```

### Human Evaluation

```
Option 1: Bring your own team
  → Show them two model responses side by side
  → They rate which is better
  → Like RLHF (Reinforcement Learning from Human Feedback)

Option 2: AWS managed teams
  → Like Mechanical Turk
  → AWS provides human evaluators
  → They rate your model's responses for you
```

---

## ⚡ Provisioned Throughput

Guarantee capacity for steady/high traffic.

```
On-demand:     shared capacity, might throttle under heavy load
Provisioned:   dedicated capacity, guaranteed performance

Use provisioned when:
  → Steady high traffic expected
  → Need consistent response times
  → Large token usage
```

---

## 🖼️ Watermark Detection (Titan Image Only)

Amazon Titan Image Generator embeds an invisible watermark in every image it creates.

```
Generate image with Titan → watermark embedded automatically
Later: "Was this image AI-generated?"
→ Use Bedrock watermark detection → "Yes, made by Titan" ✅

Use case: Detect AI-generated images, content authenticity
Only works with: Titan Image Generation model
```

---

## 🏢 Bedrock Studio

Web-based workspace for using Bedrock WITHOUT needing individual AWS accounts.

```
Problem: 
  Your team of 20 people wants to use Bedrock
  You don't want to create 20 AWS accounts

Solution: Bedrock Studio
  → Web app environment
  → Uses SSO (Single Sign-On) with your company's identity provider
  → Integrates with IAM under the hood
  → Users collaborate on projects
  → No individual AWS accounts needed
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│       BEDROCK ADDITIONAL FEATURES — QUICK RECALL             │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  Import models:                                              │
│  → From SageMaker or S3                                      │
│  → Use your own models through Bedrock API                   │
│                                                              │
│  Model evaluation:                                           │
│  → Automatic: accuracy, toxicity, BERTScore, F1              │
│  → Human: your team or AWS managed evaluators                │
│  → Provide test prompts + ideal responses                    │
│                                                              │
│  Provisioned throughput:                                     │
│  → Dedicated capacity for high/steady traffic                │
│                                                              │
│  Watermark detection:                                        │
│  → Titan Image embeds invisible watermark                    │
│  → Can detect if image was AI-generated                      │
│                                                              │
│  Bedrock Studio:                                             │
│  → Web workspace, no AWS accounts needed per user            │
│  → SSO integration with your identity provider               │
│  → Team collaboration on projects                            │
│                                                              │
│  EXAM TIPS:                                                  │
│  "Bring own model to Bedrock?"     → Import from SageMaker/S3│
│  "Evaluate model quality?"         → Model evaluation        │
│  "Detect AI-generated images?"     → Watermark detection     │
│  "Use Bedrock without AWS account?"→ Bedrock Studio + SSO    │
│  "Guaranteed capacity?"            → Provisioned throughput   │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
