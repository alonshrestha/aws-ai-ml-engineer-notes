# SageMaker Inference Options — How to Deploy Models

---

## Ways to Deploy a Model

From simplest to most control:

| Method | When to Use |
|--------|-------------|
| **SageMaker JumpStart** | Common use case, don't want to think about it. Pre-trained models deployed to pre-configured endpoints. Just run it. |
| **Model Builder (Python SDK)** | Want more control over deployment settings from code |
| **AWS CloudFormation** | Need repeatable, consistent deployments as part of CI/CD. Resource: `AWS::SageMaker::Model` |

---

## Inference Types

### 1. Real-Time Inference

```
Application → sends request → gets response immediately
```

- Interactive workloads with **low latency** requirements
- Example: "Is this credit card application fraudulent?" → need answer NOW
- One request at a time, fast response
- Standard choice for most production use cases

---

### 2. Serverless Inference

Same as real-time, but **you don't manage the infrastructure**.

**Good when:**
- Traffic has **idle periods** (no requests for stretches of time)
- Uneven/spiky traffic patterns
- Want to save money during quiet times (scales to zero)

**Bad when:**
- Steady, constant traffic (no savings over regular instances)
- Strict latency requirements (cold starts are a problem)

**Cold start** = after idle period, first request takes longer because the instance needs to wake up.

---

### 3. Asynchronous Inference

```
Application → sends large payload → "Got it, I'll get back to you"
                                          ... processing ...
                                    → "Here's your result"
```

**Good when:**
- **Large payload** (up to 1 GB of input data)
- **Long processing time** needed
- You don't want your app blocking/waiting for the answer
- Still want near real-time, just not instant

**How it works:**
1. You send the data
2. SageMaker says "received" (fast)
3. It processes asynchronously
4. Gives you the result when ready

**Use case:** Large image/video processing, big document analysis — anything where input is huge and processing takes time.

---

### 4. Batch Transform

(Not in this lecture but for completeness)

```
Give it a whole dataset → processes everything → outputs predictions for all
```

- No persistent endpoint
- Process large datasets offline
- No real-time requirement at all

---

## Quick Decision Guide

| Scenario | Choose |
|----------|--------|
| Need answer NOW, small input | **Real-time** |
| Spiky traffic, lots of idle time | **Serverless** |
| Large payload, can wait a bit | **Asynchronous** |
| Process entire dataset offline | **Batch Transform** |
| Deploy to edge device, low latency | **Neo** |

---

## Auto-Scaling (Review)

- Dynamically adjusts instances based on traffic
- Middle ground: not fully serverless, but not fully manual
- Handles traffic surges without waking you up at night

---

## Serverless Inference — Deeper Details

Automatic scaling taken to the extreme — scales to ZERO when no traffic.

```
You specify:
  → Container image
  → Memory requirements
  → Concurrency requirements

AWS handles:
  → Provisioning instances
  → Scaling up/down automatically
  → Scaling to ZERO when idle (no cost!)
```

### When to use:

```
✅ Infrequent or unpredictable traffic
✅ Want to optimize costs (pay only for what you use)
✅ Don't want to manage infrastructure

❌ Steady constant traffic (no savings)
❌ Strict latency (cold starts are a problem)
```

### CloudWatch Metrics:

```
→ Model Setup Time: how long to deploy/scale up
→ Invocations: number of requests
→ Invocation Errors: failed requests
→ Memory Utilization: is your memory config correct?
```

### How you can mess it up:

```
Wrong memory config → container crashes or wastes resources
Wrong concurrency   → requests get throttled
```

---

## SageMaker Inference Recommender

Helps you pick the RIGHT instance type for your endpoint. Not a recommendation system for users — it recommends INFRASTRUCTURE to you.

```
Problem: "I have a model. What instance type should I deploy it on?"
Solution: Inference Recommender tests different options and tells you.
```

### How it works:

```
1. Register model in Model Registry
2. Recommender benchmarks different instance types
3. Runs load tests automatically
4. Shows you: cost/hour, cost/inference, latency
5. You pick what matters most (cheap vs fast)
```

### Two modes:

```
Instance Recommendations (quick):
  → Runs load tests on recommended instance types
  → Takes ~45 minutes
  → Quick answer for "what's probably best?"

Endpoint Recommendations (custom):
  → YOU specify: instance types to test, traffic patterns, 
    latency/throughput requirements
  → Takes ~2 hours
  → Tailored to YOUR specific SLAs
```

### Output example:

```
┌──────────────────┬────────────┬────────────────┬─────────┐
│ Instance Type    │ Cost/hour  │ Cost/inference │ Latency │
├──────────────────┼────────────┼────────────────┼─────────┤
│ ml.c5.xlarge     │ $0.20      │ $0.0001        │ 45ms    │
│ ml.m5.large      │ $0.12      │ $0.0002        │ 80ms    │
│ ml.g4dn.xlarge   │ $0.73      │ $0.00005       │ 12ms    │
└──────────────────┴────────────┴────────────────┴─────────┘

Want cheapest? → ml.m5.large
Want fastest?  → ml.g4dn.xlarge
Want balanced? → ml.c5.xlarge
```

---

## Three Approaches to Capacity (Summary)

```
Manual + Inference Recommender:
  → You pick instance types (Recommender helps you choose)
  → You set auto-scaling rules
  → Most control

Auto-Scaling:
  → You pick initial instances
  → AWS adds/removes based on traffic
  → Middle ground

Serverless:
  → You specify nothing about instances
  → AWS handles everything, scales to zero
  → Least control, least cost when idle
```

---

## Neo for Inference Optimization

- Neo can optimize models for specific hardware (e.g., AWS Inferentia chips)
- Makes inference faster on target devices
- Not just for edge — can optimize for cloud inference too

---

## Exam Tips

- **JumpStart** = easiest, pre-built, just run it
- **CloudFormation** = CI/CD, repeatable deployments → `AWS::SageMaker::Model`
- **Real-time** = low latency, interactive, one request at a time
- **Serverless** = idle periods + cost savings, but cold starts
- **Asynchronous** = large payloads (up to 1GB), long processing, non-blocking
- "Cold start" in a question → they're talking about **Serverless**
- "Large payload" or "long processing time" → **Asynchronous**
- "Optimize for Inferentia" → **Neo**
- Auto-scaling works with real-time endpoints (not serverless — that scales itself)

---

## Inference Pipelines

Chain multiple containers together in sequence for inference. Each container handles one step.

```
Request → [Container 1: Pre-processing] 
        → [Container 2: Prediction] 
        → [Container 3: Post-processing] 
        → Response

Example:
  Container 1: Clean and transform raw input data
  Container 2: Run the model, get prediction
  Container 3: Format the output, add business logic
```

### Key facts:

```
→ 2 to 15 containers chained together (linear sequence)
→ Any combination of:
    • Pre-built SageMaker algorithms
    • Your own custom algorithms in Docker
    • Spark ML containers (serialized in MLeap format)
    • scikit-learn containers
→ Works with BOTH real-time inference AND batch transform
→ Spark ML can run with Glue or EMR
```

### When to use:

```
→ Need pre-processing before prediction
→ Need post-processing after prediction
→ Want to separate concerns into different containers
→ Combining multiple models/steps in sequence
```

### Exam tip:

```
"Chain multiple containers for inference?" → Inference Pipeline
"2-15 containers in sequence?"            → Inference Pipeline
"Spark ML format in SageMaker?"           → MLeap
"Pre-process + predict + post-process?"   → Inference Pipeline
```
