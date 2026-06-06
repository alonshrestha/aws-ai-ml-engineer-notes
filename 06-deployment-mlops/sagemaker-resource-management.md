# SageMaker Resource Management — Instance Types & Spot Training

---

## Choosing Instance Types

### Simple Rule

| Algorithm Type | Training | Inference |
|---------------|----------|-----------|
| **Deep Learning** (neural networks) | GPU → P3 or G4 | Lighter → C5 (compute) |
| **Not Deep Learning** (XGBoost, linear, etc.) | CPU → M5 (general purpose) | M5 or C5 |

### Key Points

- **GPU (P3, G4)** = deep learning. Always benefits from GPU acceleration.
- **Inference is less demanding** than training → you can use cheaper instances (C5, M5)
- **Don't use GPU if your algorithm can't use it** — waste of money
- **One GPU machine with multiple GPUs** can be more cost-effective than many CPU machines doing the same work
- GPU instances are expensive per-hour, but they finish faster → sometimes cheaper overall

### Instance Family Cheat Sheet

| Family | Type | Use For |
|--------|------|---------|
| **P3, P4** | GPU | Deep learning training (heavy) |
| **G4, G5** | GPU | Training + inference (cost-effective GPU) |
| **C5** | Compute optimized | Inference, CPU-heavy algorithms |
| **M5** | General purpose | Non-deep-learning training, general workloads |

---

## Managed Spot Training

### What It Is

Use EC2 Spot Instances for training instead of On-Demand. **Save up to 90% cost.**

### How It Works

```
On-Demand:  You pay full price, instance is always yours
Spot:       You pay ~10% of the price, BUT AWS can take it back anytime
```

### The Catch

Spot instances can be **interrupted at any time** (AWS needs the capacity back).

**Solution: Checkpoints to S3**

```
Training starts → saves progress to S3 periodically (checkpoints)
    │
    ├── Instance interrupted? No problem.
    │   Wait for new spot instance → resume from last checkpoint
    │
    └── Instance NOT interrupted? Great, training finishes cheaper.
```

### Tradeoffs

| | On-Demand | Spot |
|---|---|---|
| Cost | Full price | Up to 90% cheaper |
| Availability | Immediate | May have to wait |
| Interruption risk | None | Can be interrupted anytime |
| Complexity | Simple | Need to set up S3 checkpoints |
| Training time | Predictable | Unpredictable (waits + restarts) |

### When to Use Spot

✅ Large, expensive training jobs (e.g., machine translation)
✅ You care more about cost than time
✅ Your framework supports checkpointing

### When NOT to Use Spot

❌ You need results urgently (tight deadline)
❌ Short training jobs (overhead of checkpointing not worth it)
❌ Can't afford interruptions

---

---

## Automatic Scaling (Inference)

### What It Is

SageMaker auto-scales your inference endpoints — adds or removes instances based on demand. Works with CloudWatch.

### How It Works

```
Low traffic  → scale down (fewer instances, save money)
High traffic → scale up (more instances, handle load)
```

### What You Configure

- **Target metric** — what to watch (e.g., invocations per instance)
- **Minimum capacity** — never go below this many instances
- **Maximum capacity** — never go above this many instances
- **Cool down period** — wait time before scaling again (prevents thrashing)

### Key Points

- Scales **per production variant** — not just the whole endpoint
- As you shift traffic between variants, scaling adjusts each one independently
- Works alongside CloudWatch monitoring

### Best Practice: Load Test First

**Always load test your scaling policy before production.**

If your policy is wrong:
- Too aggressive → not enough capacity → requests fail
- Too conservative → way more instances than needed → waste money

Test it. Verify it works. Then deploy.

---

## Availability Zones

### What SageMaker Does

SageMaker **automatically distributes instances across multiple AZs** for resiliency.

### Requirements

- You need **more than 1 instance** (can't distribute 1 instance across AZs)
- Your VPC needs **at least 2 subnets in different AZs**

### Why It Matters

```
AZ-1 goes down:
├── 1 instance only?  → Your endpoint is DEAD
└── 2+ instances across AZs? → Still running, no downtime
```

### Best Practice

Always deploy **multiple instances** for production endpoints — even if one could handle the load. The second instance isn't for capacity, it's for **resilience**.

---

## Exam Tips

- Deep learning → **GPU (P3, G4)**
- Not deep learning → **CPU (M5)**
- Inference → **cheaper than training (C5, M5)**
- "Save money on training" → **Managed Spot Training**
- "Spot + checkpoints to S3" → always paired together
- Spot = up to **90% savings** but **longer training time** and **more complexity**
- One multi-GPU machine can be cheaper than many CPU machines for the same work
- **Auto-scaling** works per production variant, uses CloudWatch
- **Load test** scaling policies before production
- **Multiple AZs** = need 2+ instances + VPC with 2+ subnets in different AZs
- SageMaker distributes across AZs **automatically** (if you give it enough instances)
