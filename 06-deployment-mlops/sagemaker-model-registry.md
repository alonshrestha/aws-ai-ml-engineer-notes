# SageMaker Model Registry

## 📦 What Is It?

A catalog (fancy database) for all your models. It keeps track of which models exist, their versions, and whether they're approved for production.

```
Think of it like an app store for your organization's models:

┌─────────────────────────────────────────────────┐
│  Model Registry                                 │
│                                                 │
│  Fraud Model v1    → approved ✅  → in production│
│  Fraud Model v2    → pending ⏳   → waiting      │
│  Churn Model v1    → approved ✅  → in production│
│  Pricing Model v1  → rejected ❌  → needs work   │
│  Pricing Model v2  → approved ✅  → in production│
│                                                 │
└─────────────────────────────────────────────────┘
```

---

## 🔑 What It Does

```
1. Version tracking     → "Which version of this model is live?"
2. Approval management  → "Is this model approved for production?"
3. Metadata storage     → "What's this model trained on? What are its limits?"
4. Model sharing        → "Other teams can find and use my model"
5. CI/CD integration    → "Auto-deploy when model is approved"
```

---

## 🔄 Real Pipeline Example

How model registry fits into a real ML workflow:

```
Step 1: Train model
        ↓
Step 2: Register model in Model Registry
        ↓
Step 3: EventBridge detects "new model registered"
        ↓
Step 4: Lambda sends approval request to a human
        ↓
Step 5: Human reviews → approves or rejects
        ↓
Step 6: Lambda updates Model Registry → "approved" ✅
        ↓
Step 7: Triggers deployment pipeline
        ↓
Step 8: Model deployed to dev → test → production
        ↓
Step 9: Team notified: "New model is live"
```

```
Train → Register → Approve → Deploy → Notify

Model Registry sits in the middle, controlling 
the flow from training to production.
```

---

## 🏷️ Model Cards Integration

Model cards = a simple web page describing your model.

```
Model Card for "Fraud Detection v2":

  What it does:      Detects fraudulent transactions
  Trained on:        2 years of transaction data
  Accuracy:          94%
  Limitations:       Doesn't work well for transactions < $1
  Bias check:        Passed Clarify review
  Approved by:       Alice (ML Lead)
  Last updated:      March 2026
```

Model Registry links to the model card so anyone can look up this info.

---

## 🧭 Where It Fits

```
Phase 4: Train model
Phase 5: Evaluate model
Phase 6: Deploy model ← Model Registry lives here
         ├── Register the model
         ├── Get approval
         ├── Deploy to production
         └── Share with other teams
Phase 7: Monitor model
```

It's the gatekeeper between "model is ready" and "model is in production."

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│          SAGEMAKER MODEL REGISTRY — QUICK RECALL             │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  What is it?                                                 │
│  → Catalog/database for all your models                      │
│  → Tracks versions, approval status, metadata                │
│                                                              │
│  Key features:                                               │
│  → Version management (v1, v2, v3...)                        │
│  → Approval workflow (pending → approved → deployed)         │
│  → Metadata + Model Cards (description, limitations)         │
│  → Share models across teams/applications                    │
│  → CI/CD integration (auto-deploy on approval)               │
│                                                              │
│  Integrates with:                                            │
│  → EventBridge (detect new model registered)                 │
│  → Lambda (trigger approval workflow)                        │
│  → SageMaker Inference Pipelines (deploy)                    │
│  → Model Cards (governance info)                             │
│                                                              │
│  EXAM TIPS:                                                  │
│  "Manage model versions?"           → Model Registry         │
│  "Approve models before deploy?"    → Model Registry         │
│  "Share models across org?"         → Model Registry         │
│  "Model governance and metadata?"   → Model Registry + Cards │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
