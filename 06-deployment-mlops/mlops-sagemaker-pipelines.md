# MLOps — SageMaker Projects & Pipelines

## 🤔 What Is MLOps?

Managing the entire lifecycle of ML models — building, testing, deploying, monitoring, updating. Like DevOps but for machine learning.

```
DevOps:  code → build → test → deploy → monitor → repeat
MLOps:   data → train → evaluate → deploy → monitor → retrain → repeat
```

---

## 🔧 Two Paths: Kubernetes or SageMaker Native

### Path 1: Already Have Kubernetes? → Integrate SageMaker

If you have existing Kubernetes/Kubeflow ML infrastructure (maybe on-premises), you can integrate SageMaker into it gradually.

```
Existing Kubernetes MLOps pipeline
  ↓
Add SageMaker components for Kubeflow Pipelines:
  → Processing (Spark containers)
  → Hyperparameter tuning
  → Training
  → Inference (hosting)

Benefits:
  → Hybrid: keep some on-premises, some on AWS
  → Gradual migration (don't have to go all-in with SageMaker)
  → Sensitive data stays on-premises if needed
```

### Path 2: Starting Fresh? → SageMaker Projects

All-in with SageMaker. No Kubernetes needed.

```
SageMaker Projects = native MLOps solution in SageMaker Studio
  → CI/CD for machine learning
  → Build, train, evaluate, deploy, monitor — all in one
  → Uses SageMaker Pipelines to define the steps
```

---

## 📋 SageMaker Pipelines

Defines the steps in your ML workflow — like a recipe.

```
Pipeline example:

  Step 1: Process data
       ↓
  Step 2: Train model
       ↓
  Step 3: Evaluate model
       ↓
  Step 4: Register in Model Registry
       ↓
  Step 5: Store model in S3
```

Each step is defined in code. The pipeline runs them in order automatically.

---

## 🏗️ Full MLOps Example (SageMaker Project)

A real production pipeline might look like this:

```
Developer commits code change
       ↓
EventBridge detects the change
       ↓
AWS CodePipeline kicks off (model building)
       ↓
CodeBuild executes SageMaker Pipeline:
  → Process data → Train → Evaluate → Register model
       ↓
EventBridge fires again (model registered)
       ↓
Another CodePipeline kicks off (deployment)
       ↓
CodeBuild + CloudFormation deploys to STAGING endpoint
       ↓
CodeBuild tests the staging endpoint
       ↓
Manual approval ✅
       ↓
CloudFormation deploys to PRODUCTION endpoint
       ↓
Model is live! 🎉
```

### AWS Services involved:

```
EventBridge:     detects events, triggers pipelines
CodePipeline:    orchestrates the CI/CD flow
CodeBuild:       builds and executes steps
CloudFormation:  deploys infrastructure (endpoints)
SageMaker Pipeline: the ML-specific steps (train, evaluate, register)
Model Registry:  tracks model versions and approval
S3:              stores models and data
```

---

## 🧭 Key Terms

```
SageMaker Projects:   MLOps solution in SageMaker Studio (the umbrella)
SageMaker Pipelines:  Define the ML workflow steps (the recipe)
Kubeflow Pipelines:   Kubernetes-based MLOps (alternative to SageMaker)
SageMaker Components: Plug SageMaker into existing Kubeflow pipelines
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│          MLOps & SAGEMAKER PIPELINES — QUICK RECALL           │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  MLOps = CI/CD for machine learning                          │
│                                                              │
│  Two paths:                                                  │
│  → Have Kubernetes? → SageMaker Components for Kubeflow      │
│  → Starting fresh?  → SageMaker Projects + Pipelines         │
│                                                              │
│  SageMaker Projects: native MLOps in Studio                  │
│  SageMaker Pipelines: define ML workflow steps               │
│                                                              │
│  Kubeflow integration:                                       │
│  → Hybrid on-prem + cloud                                    │
│  → Components for: processing, tuning, training, inference   │
│  → Gradual migration to SageMaker                            │
│                                                              │
│  Full pipeline uses:                                         │
│  → EventBridge (triggers)                                    │
│  → CodePipeline + CodeBuild (CI/CD)                          │
│  → CloudFormation (deploy endpoints)                         │
│  → Model Registry (version + approve)                        │
│                                                              │
│  EXAM TIPS:                                                  │
│  "MLOps in SageMaker?"              → SageMaker Projects     │
│  "Define ML workflow steps?"         → SageMaker Pipelines   │
│  "Integrate with Kubernetes?"        → SageMaker Components  │
│                                                              │
│  "Hybrid on-prem + cloud ML?"        → Kubeflow + SageMaker  │
│  "CI/CD for ML models?"              → SageMaker Projects    │
│  "Automate train→deploy pipeline?"   → SageMaker Pipelines   │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
