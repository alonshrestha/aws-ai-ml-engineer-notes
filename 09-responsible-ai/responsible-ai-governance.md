# Responsible AI & ML Governance

## 🎯 Core Dimensions of Responsible AI

Amazon defines these as the key things to consider when building AI systems:

```
1. Fairness         → Is it biased? Does it treat everyone equally?
2. Explainability   → Can you explain WHY it made a decision?
3. Privacy/Security → Is sensitive data protected? Can it leak PII?
4. Safety           → Can it cause harm? Does it behave predictably?
5. Controllability  → Can you tune/change its behavior when needed?
6. Veracity/Robustness → Does it tell the truth? How often does it hallucinate?
7. Governance       → How do you track what it's doing? Are you compliant with laws?
8. Transparency     → Are you publishing its limitations and capabilities?
```

---

## 🔧 AWS Tools for Responsible AI

### Amazon Bedrock — Model Evaluation

```
→ Evaluate accuracy of LLM responses
→ Test prompts vs ideal responses
→ Works even with non-deterministic models
→ Built into Bedrock
```

### SageMaker Clarify — Bias Detection & Explainability

```
Bias detection:
  → Detects bias in training data
  → Detects bias in model output
  → Shows imbalances across groups (age, income, etc.)
  → Tools to FIX bias (rebalance data, add new data)

Explainability:
  → Shows which features drive predictions
  → "Remove this feature — what changes?"
  → Helps with feature selection and engineering
  → "Loan denied because: income (60%), credit score (30%)"
```

### SageMaker Model Monitor — Continuous Monitoring

```
→ Alerts when model accuracy degrades
→ Detects drift over time
→ Keeps watching after deployment
→ CloudWatch alarms when things go wrong
```

### Amazon Augmented AI (A2I) — Humans in the Loop

```
→ Insert humans to double-check AI results
→ RLHF (Reinforcement Learning from Human Feedback)
→ Humans rate which response is better → model improves
→ Options: bring your own team OR use AWS managed teams
→ You control who does the work
```

### SageMaker ML Governance Tools

```
SageMaker Role Manager:
  → Define roles and permissions for SageMaker users
  → "Data scientists can train but not deploy"
  → "ML engineers can deploy but not delete"

Model Cards:
  → Document: intended use, risks, limitations
  → Publish for transparency
  → "This model is for X, not for Y, has these risks"

Model Dashboard:
  → Quick access to all your models
  → Overview of status, performance, issues
```

---

## 🧭 How These Map Together

```
Concern                    AWS Tool
─────────────────────────────────────────
Fairness/Bias           → Clarify
Explainability          → Clarify
Privacy                 → Guardrails (PII removal)
Safety                  → Guardrails (content filtering)
Controllability         → Guardrails + Model Monitor
Veracity (truth)        → Bedrock Model Evaluation + Grounding checks
Governance              → Role Manager + Model Cards + Model Dashboard
Transparency            → Model Cards
Human oversight         → Augmented AI (A2I)
Continuous monitoring   → Model Monitor
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│       RESPONSIBLE AI & GOVERNANCE — QUICK RECALL             │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  8 dimensions: fairness, explainability, privacy, safety,    │
│  controllability, veracity, governance, transparency         │
│                                                              │
│  Tools:                                                      │
│  → Clarify: bias detection + explainability                  │
│  → Model Monitor: continuous accuracy monitoring             │
│  → Bedrock Guardrails: content filtering + PII removal       │
│  → Bedrock Model Evaluation: test LLM accuracy               │
│  → Augmented AI (A2I): humans in the loop                    │
│  → Model Cards: document limitations and risks               │
│  → Role Manager: permissions for SageMaker users             │
│  → Model Dashboard: overview of all models                   │
│                                                              │
│  EXAM TIPS:                                                  │
│  "Detect bias in data?"           → Clarify                  │
│  "Explain model decisions?"       → Clarify                  │
│  "Human review of AI output?"     → Augmented AI (A2I)       │
│  "Document model limitations?"    → Model Cards              │
│  "Control user permissions?"      → Role Manager             │
│  "Monitor deployed model?"        → Model Monitor            │
│  "Remove PII from responses?"     → Bedrock Guardrails       │
│  "Evaluate LLM accuracy?"         → Bedrock Model Evaluation │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
