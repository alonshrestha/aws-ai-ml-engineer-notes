# ML System Design Principles (AWS Best Practices)

## 🎯 The ML Lifecycle

```
Define Business Goal
       ↓
Frame as ML Problem
       ↓
Collect Data
       ↓
Process Data ←──────────┐
       ↓                 │ (iterate back if needed)
Develop Model ───────────┘
       ↓
Deploy Model
       ↓
Monitor ──→ (if drift/issues → back to Model Development or Data)
       ↓
Next Business Goal → repeat
```

Key point: Always start from a BUSINESS GOAL, not from "I have AI, what can I do with it?"

---

## 📋 Design Principles

### 1. Assign Ownership

```
→ Someone must OWN each ML system
→ Models drift, data changes, things break
→ Without an owner, nobody notices until it's too late
```

### 2. Provide Protection (Security)

```
→ Control who can access/modify the model
→ Control what data goes in and comes out
→ IAM roles, VPC, encryption
```

### 3. Enable Resiliency

```
→ Fault tolerance (server goes down → system keeps running)
→ Recoverability (training fails midway → pick up where you left off)
→ Checkpointing for long training jobs
→ Lots of hardware = lots of potential failure points
```

### 4. Enable Reusability

```
→ Training is expensive — don't redo it unnecessarily
→ Reuse trained models across applications
→ Model Registry, Feature Store
→ Foundation models + fine-tuning = reuse
```

### 5. Enable Reproducibility

```
→ Version control for models AND data
→ "What did the model look like 3 months ago?"
→ Need to reproduce results at any point in time
→ Model Registry, data versioning
```

### 6. Optimize Resources

```
→ Choose efficient hardware for the task
→ Not everything needs a giant GPU cluster
→ Small model might work as well as a large one
→ Right-size your instances
```

### 7. Reduce Cost

```
→ Explicitly called out for the exam
→ Use spot instances where possible
→ Serverless inference for spiky traffic
→ Don't over-provision
→ Use smaller models when they're good enough
→ Warm pools to avoid re-provisioning
```

### 8. Enable Automation

```
→ CI/CD for ML (SageMaker Projects/Pipelines)
→ Continuous Training (CT) — auto-retrain with new data
→ But monitor that auto-training doesn't introduce bias/drift
→ Don't manage training by hand
```

### 9. Enable Continuous Improvement

```
→ Monitor model quality over time (Model Monitor)
→ Detect drift before users notice
→ Feed improvements back into the model
→ Iterate: monitor → detect issue → fix → redeploy
```

### 10. Minimize Environmental Impact

```
→ AI is extremely energy hungry
→ Data centers use as much energy as cities
→ Use managed services (efficient by default)
→ Use most energy-efficient hardware available
→ Ask: "Do I even need AI for this? Is there a simpler solution?"
→ Don't spin up expensive servers just because you can
```

---

## 🔄 Lifecycle Stages Mapped to Principles

### Stage 1: Define Business Goal (Detailed)

```
Before building anything, answer these:

→ Do you have the right skills on the team?
  (accountability + empowerment)

→ What level of explainability do you need?
  More complex model = better results BUT harder to explain
  Simpler model = easier to explain BUT less powerful
  Conscious tradeoff. Use Clarify + Experiments to help.

→ How will you monitor compliance to business requirements?
  Not just "is it accurate?" but "is it providing business value?"

→ Data & software concerns:
  Do you have permission to use this data?
  Any licensing issues? Copyright?
  Privacy concerns? PII?
  Think about this BEFORE building, not after.

→ Define KPIs (Key Performance Indicators):
  How will you measure success? (business metrics, not just accuracy)
  What's the ROI? Is the cost justified?
  What's the opportunity cost? (what else could you do instead?)

→ Use managed services when possible:
  Reduces total cost of ownership (TCO)
  Avoids paying for idle capacity
  But evaluate — sometimes managed is MORE expensive

→ Environmental impact:
  Power consumption, water for cooling
  Is AI even necessary for this problem?
  Don't make the planet worse for your project
```

### Stage 2: Frame as ML Problem (Detailed)

```
Best Practice                        AWS Tool
──────────────────────────────────────────────────────────────
Establish ML roles/responsibilities → SageMaker Role Manager
Prepare resource documentation      → ML profile template
Model improvement strategies        → SageMaker Experiments
                                      Automatic HPO
                                      AutoML (Autopilot)
Lineage tracking                    → SageMaker Lineage Tracking
                                      SageMaker Pipelines
                                      SageMaker Studio
                                      Feature Store
                                      Model Registry
Feedback loops                      → Model Monitor (drift)
                                      CloudWatch (operational)
                                      Augmented AI (human eval)
Fairness & explainability           → SageMaker Clarify
Data encryption & PII               → Glue DataBrew (PII detection/masking)
API abstraction                     → SageMaker + API Gateway
                                      (decouple app from model version)
Microservice strategy               → Lambda + Fargate
Purpose-built AI services           → SageMaker JumpStart
                                      AWS Marketplace solutions
Evaluation metrics                  → Define KPIs, monitor over time
```

### Key Exam Points for This Stage:

```
→ Is ML even the right solution? (maybe simpler is better)
→ Cost control is explicitly tested on the exam
→ Trade-off: custom model vs pre-trained model
  (pre-trained = cheaper, faster, reuse when possible)
→ Sustainability: select greener AWS regions
  (West Coast US = more hydroelectric than East Coast)
→ Use API Gateway to decouple apps from model versions
→ Don't reinvent the wheel — use JumpStart, Marketplace, pre-trained models
```

### Stage 3: Data Processing (Detailed)

Three sub-stages: Collection → Pre-processing → Feature Engineering

```
DATA COLLECTION:
  → Label data (Ground Truth, humans, or pre-existing labels)
  → Ingest data (streaming or batch into S3)
  → Aggregate data (combine from multiple sources)

DATA PRE-PROCESSING:
  → Clean (handle missing data, outliers)
  → Partition (organize for efficient access)
  → Scale (Spark/EMR for large datasets)
  → Unbias & balance (detect and fix imbalances)
  → Augment (add more data if needed)

FEATURE ENGINEERING:
  → Select important features
  → Transform (normalize, one-hot encode)
  → Create new features from existing ones
  → Extract (zip code from address, etc.)
```

### Best Practices → AWS Tools:

```
Best Practice                          AWS Tool
──────────────────────────────────────────────────────────────
Profile data quality                → Data Wrangler, Glue, Athena,
                                      Redshift, QuickSight
Version control for data            → Model Registry, Git, 
                                      SageMaker Experiments
Least privilege access              → IAM (start with zero, add only needed)
Secure environment                  → SageMaker, EMR, Athena
                                      (built-in encryption)
Secure data transfer                → VPC + PrivateLink
Protect sensitive data              → Amazon Macie (auto-detect PII)
Data lineage tracking               → SageMaker ML Lineage Tracker
Remove unnecessary data             → Comprehend, Transcribe (identify PII)
                                      Athena (query and remove columns)
Data catalog                        → AWS Glue (schema for unstructured data)
Data pipeline                       → SageMaker Pipelines
Modern data architecture            → Data Lake (S3-based)
Managed data labeling               → Amazon Ground Truth
Interactive data analysis           → SageMaker Data Wrangler
Managed data processing             → SageMaker Processing
Feature reusability                 → SageMaker Feature Store
Minimize idle resources             → Managed services (auto-scale)
Data lifecycle policies             → Delete old data, use Glacier for cold
Sustainable storage                 → Cheapest tier possible, delete if unused
```

### Stage 4: Model Development — Train, Tune, Evaluate (Detailed)

```
Best Practice                              AWS Tool
──────────────────────────────────────────────────────────────
Automate via MLOps/CI/CD               → CloudFormation, CDK, SageMaker 
                                         Pipelines, Step Functions
Secure packaging                       → ECR, CodeArtifact
Secure inter-node communication        → SageMaker inter-node encryption,
                                         EMR encryption in transit
Protect against data poisoning         → Clarify (detect bias drift),
                                         Model Registry + Feature Store (rollback)
CI/CD/CT automation with traceability  → SageMaker Pipelines
Feature consistency (train vs inference)→ Feature Store
Model validation                       → Experiments, Model Monitor
Bias detection                         → Clarify
Optimize instance types                → Max out single instance before parallel
Test alternatives                      → SageMaker Experiments
Model evaluation pipeline              → Pipelines + Model Registry
Feature statistics                     → Model Monitor, Data Wrangler, Clarify
Transfer learning issues               → SageMaker Debugger
Managed training                       → Training Compiler (deprecated),
                                         Managed Spot Instances
Distributed training                   → DDP library, Model Parallel library
Hyperparameter tuning                  → SageMaker AMT (Bayesian/Hyperband)
Debugging & logging                    → SageMaker Debugger, CloudWatch
AutoML                                 → SageMaker Autopilot
```

### Key Cost & Sustainability Points (Exam Loves These):

```
→ Stop resources when not in use (billing alarms!)
→ SageMaker Studio: enable auto-shutdown
→ SageMaker lifecycle configuration: auto spin up/down
→ Start with small datasets for experiments
→ Use warm pools (reuse hardware between runs)
→ Use checkpointing (don't restart from zero on failure)
→ AWS Budgets + Cost Explorer to track spending
→ Data and compute in same region (reduce latency)
→ Early stopping (stop training when good enough)
→ Don't use deep learning if simpler algorithm works
→ Archive/delete old training artifacts
→ Limit concurrent tuning jobs
→ Only tune most important hyperparameters
→ Use Bayesian/Hyperband tuning (not random/grid search)
→ Use managed spot instances (cheaper)
→ Select energy-efficient algorithms when possible
```

### Trade-offs to Know:

```
Accuracy vs Complexity    → more accurate = more expensive
Bias vs Fairness          → biased data = unfair model
Precision vs Recall       → classic ML tradeoff
Simple vs Complex model   → simpler is cheaper and often good enough
```

### Stage 5: Deployment (Detailed)

```
Architecture:
  Model Registry → Inference Container (code + model artifacts)
  Feature Store  → ensures features match training
  Endpoint       → serves predictions to your application
  Application    → uses predictions for business value
```

### Best Practices → AWS Tools:

```
Best Practice                              AWS Tool
──────────────────────────────────────────────────────────────
Deployment metrics & alerting          → CloudWatch, EventBridge, SNS
Detect data poisoning/drift            → Model Monitor
Automate deployments                   → SageMaker Pipelines
Deployment strategies                  → Blue/Green, A/B testing,
                                         Canary, Linear deployments
Edge deployment                        → SageMaker Neo, IoT Greengrass
                                         (low latency, no internet needed)
Choose deployment type                 → Real-time, Serverless, Async, Batch
Multi-model/multi-container            → Inference Pipelines
Rightsize instance fleet               → Inference Recommender + Auto-scaling
```

### Cost & Sustainability for Deployment:

```
Cost-effective hardware:
  → Inf1/Inf2 (Inferentia) for deep learning inference
  → Graviton3 for CPU inference
  → Trn1 (Trainium) for training
  → SageMaker Neo for edge optimization
  → Elastic Inference (boost existing instances)

Optimize models for inference:
  → Neo, Tree Lite, Hugging Face Infinity

Align SLAs with sustainability:
  → Don't use real-time if batch is fine
  → Serverless for spiky traffic (scales to zero)
  → Auto-scaling to avoid idle resources

Multiple models behind one endpoint:
  → SageMaker Inference Pipelines
```

### Key Instance Types for Exam:

```
Task                    Instance/Chip
─────────────────────────────────────
CPU inference        → Graviton3
Deep learning inference → Inferentia (Inf1, Inf2)
Training             → Trainium (Trn1)
Edge deployment      → Neo + Greengrass
```

### Stage 6: Monitoring (Detailed)

```
Best Practice                              AWS Tool
──────────────────────────────────────────────────────────────
Model observability & tracking         → Model Monitor, CloudWatch,
                                         Clarify, Model Cards,
                                         Lineage Tracking
Sync architecture, check for skew     → CloudFormation + Model Monitor
Secure inference endpoints             → IAM, logging
Detect anomalous human activity        → GuardDuty, Macie
Auto-scaling (avoid midnight alerts)   → Auto-scaling, Elastic Inference
Recoverable endpoints (rollback)       → ECR, CloudFormation, CodeCommit,
                                         Pipelines, Projects
Model explainability over time         → Clarify
Data drift detection                   → Clarify, Model Monitor, OpenSearch
Automated retraining framework         → SageMaker Pipelines, Step Functions,
                                         Jenkins
Review updated data for retraining     → Data Wrangler
Human evaluation                       → Amazon Augmented AI (A2I)
Monitor usage and cost                 → Tagging, AWS Budgets
Monitor ROI                            → QuickSight (business value tracking)
Rightsize instance fleet               → CloudWatch + SageMaker Auto-scaling
Large-scale training storage           → Amazon FSx for Lustre (not S3)
                                         (more bandwidth for massive training)
Only retrain when necessary            → Model Monitor (detect when drift
                                         exceeds acceptable threshold)
```

### Key Exam Points for Monitoring:

```
→ Material efficiency = provisioned resources / business outcomes
  (Are you spending more than you're earning? Measure this!)

→ FSx for Lustre > S3 for large-scale training (more bandwidth)

→ Only retrain when model drift exceeds your defined threshold
  (don't retrain just for fun — it's expensive)

→ GuardDuty + Macie for detecting malicious/anomalous activity

→ Humans (A2I) may be the best evaluators for GenAI quality

→ Auto-scaling + Elastic Inference = avoid over-provisioning

→ Tagging resources = understand where money is going
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│       ML DESIGN PRINCIPLES — QUICK RECALL                    │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  10 Principles:                                              │
│  1. Assign ownership (someone watches it)                    │
│  2. Provide protection (security controls)                   │
│  3. Enable resiliency (fault tolerance, recovery)            │
│  4. Enable reusability (don't retrain unnecessarily)         │
│  5. Enable reproducibility (version control)                 │
│  6. Optimize resources (right-size, efficient hardware)      │
│  7. Reduce cost (spot, serverless, smaller models)           │
│  8. Enable automation (CI/CD, continuous training)           │
│  9. Continuous improvement (monitor, detect drift, fix)      │
│  10. Minimize environmental impact (energy efficiency)       │
│                                                              │
│  Lifecycle:                                                  │
│  Business Goal → Frame Problem → Collect Data →              │
│  Process Data → Develop Model → Deploy → Monitor → Repeat    │
│                                                              │
│  KEY EXAM POINTS:                                            │
│  → Always start from business goal (not from "I have AI")    │
│  → Cost reduction is explicitly tested                       │
│  → Monitoring feeds back into model development              │
│  → Continuous training needs monitoring (can introduce bias) │
│  → Not everything needs AI — ask if simpler solution exists  │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
