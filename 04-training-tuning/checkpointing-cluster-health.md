# Checkpointing & Cluster Health Checks

## 💾 Checkpointing

Saves snapshots of your training at regular intervals. If something crashes, you pick up where you left off instead of starting over.

```
Without checkpointing:

  Training... hour 1 ✅ hour 2 ✅ hour 3 ✅ hour 4 💥 CRASH
  
  Start over from hour 1. Four hours wasted.


With checkpointing:

  Training... hour 1 💾 hour 2 💾 hour 3 💾 hour 4 💥 CRASH
                saved    saved    saved
  
  Restart from hour 3 checkpoint. Only lost 1 hour.
```

### How it works:

```
Training instance                         S3
┌──────────────┐                    ┌──────────────┐
│ Local path:  │  ← auto sync →    │ Checkpoint    │
│ /opt/ml/     │                    │ S3 URI        │
│ checkpoints/ │                    │               │
└──────────────┘                    └──────────────┘

Checkpoints saved locally → automatically synced to S3
If crash → reload from S3 → continue training
```

### How to enable:

```
In code (SageMaker Estimator):
  → checkpoint_s3_uri = "s3://my-bucket/checkpoints/"
  → checkpoint_local_path = "/opt/ml/checkpoints" (default)

In console:
  → Create Training Job → Checkpoint Configuration
  → Enter S3 location + local path
```

### Also useful for debugging:

```
Training went wrong somewhere in 10 hours of training.

With checkpoints you can:
  → Look at model state at hour 2, 4, 6, 8, 10
  → "Model was fine at hour 6, broken at hour 8"
  → Narrow down the problem
```

---

## 🏥 Cluster Health Checks & Automatic Restarts

When training on many instances, hardware can fail. SageMaker automatically handles this.

```
Training across 10 GPU instances:

  Instance 1: ✅ healthy
  Instance 2: ✅ healthy
  Instance 3: ❌ GPU failed!
  Instance 4: ✅ healthy
  ...

  SageMaker automatically:
    1. Detects Instance 3 is bad
    2. Replaces it with a new instance
    3. Restarts healthy instances
    4. Restarts the training job
```

### What it checks:

```
→ GPU health on all instances
→ NCCL (Nvidia Collective Communication Library) working properly
→ SageMaker internal service errors
```

### Key points:

```
→ Happens automatically (no setup needed)
→ Works with ML and MLP instance types
→ Replaces faulty instances automatically
→ Restarts training after replacement
→ More instances = higher chance of failure = more important
```

### Why this matters at scale:

```
Training on 2 instances:   low chance of failure
Training on 100 instances: something WILL break eventually

SageMaker handles it so you don't have to babysit.
```

---

## 🔗 Checkpointing + Health Checks Together

```
Instance fails at hour 6 of training:

  1. Health check detects bad instance
  2. Replaces it with new instance
  3. Loads checkpoint from hour 5 (saved in S3)
  4. Continues training from hour 5

  Without checkpointing: restart from hour 0 😩
  With checkpointing: restart from hour 5 ✅
```

They work together — health checks handle the hardware, checkpointing saves your progress.

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│     CHECKPOINTING & CLUSTER HEALTH — QUICK RECALL            │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  CHECKPOINTING:                                              │
│  → Saves training snapshots to S3                            │
│  → Resume from last checkpoint if crash                      │
│  → Also useful for debugging (analyze model at each point)   │
│  → Config: checkpoint_s3_uri + checkpoint_local_path         │
│  → Default local path: /opt/ml/checkpoints                   │
│                                                              │
│  CLUSTER HEALTH CHECKS:                                      │
│  → Automatic (no setup needed)                               │
│  → Checks GPU health + NCCL                                  │
│  → Replaces faulty instances automatically                   │
│  → Restarts training job after replacement                   │
│  → Works with ML/MLP instance types                          │
│                                                              │
│  EXAM TIPS:                                                  │
│  "Resume training after failure?"     → Checkpointing        │
│  "Auto-replace bad GPU instances?"    → Cluster health checks│
│  "Save training progress to S3?"      → Checkpointing        │
│  "Debug training at specific points?" → Checkpointing        │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
