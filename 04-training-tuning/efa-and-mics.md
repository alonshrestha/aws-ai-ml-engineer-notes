# Elastic Fabric Adapter (EFA) & MiCS

## ⚡ Elastic Fabric Adapter (EFA)

When you distribute training across many instances, they need to talk to each other constantly — sending gradients, weights, activations back and forth. The network becomes the bottleneck.

EFA = a special network device that makes this communication much faster.

```
Without EFA:
  GPU 1 ──── regular network ──── GPU 2
  "Sending gradients... waiting... waiting..."
  Network is slow. GPUs sit idle waiting for data.

With EFA:
  GPU 1 ──── EFA (high speed) ──── GPU 2
  "Gradients sent instantly."
  GPUs stay busy computing. No waiting.
```

### What it is:

```
→ A network device attached to your SageMaker instances
→ Gives cloud performance equal to on-premises HPC clusters
→ Uses NCCL (Nvidia Collective Communication Library)
→ Requires Nvidia GPUs
```

### How to enable:

```
Include in your training container:
  → NCCL
  → EFA
  → AWS OFI NCCL plugin

Set environment variable:
  FI_PROVIDER = "efa"
```

---

## 🚀 MiCS (Minimize Communication Scale)

When billions of parameters aren't enough and you need TRILLIONS.

MiCS is just a name for combining everything we've learned:

```
MiCS = all the distributed training tricks together

  → Sharded data parallelism (split weights + data)
  → Model parallelism (split model across GPUs)
  → EFA (fast networking)
  → Huge instance types (P4d with 8 GPUs each)
  → FSx storage (fast I/O)

All combined to train 1 trillion+ parameter models.
```

### The infrastructure at this scale:

```
┌─────────────────────────────────────────────────┐
│  EC2 Ultra Cluster                              │
│                                                 │
│  Storage: FSx (petabytes/sec throughput)        │
│           ↕                                     │
│  4000+ Nvidia A100 GPUs                         │
│  Built on P4d instances                         │
│  (8 GPUs per instance, 80GB GPU memory each)    │
│           ↕                                     │
│  400 Gbps EFA networking between instances      │
│                                                 │
│  Cost: hundreds of millions of dollars 💸       │
└─────────────────────────────────────────────────┘
```

---

## 🧭 The Full Picture — All Distributed Training Tools

```
Problem                          Solution
─────────────────────────────────────────────────
Data too big for one GPU       → Data parallelism (DDP)
Model too big for one GPU      → Model parallelism (SMP)
Network too slow               → EFA
Need trillion+ parameters      → MiCS (everything combined)
Want faster GPU code           → Training Compiler (phased out)
Repeated training runs         → Warm Pools
Training crashes               → Checkpointing
Hardware fails                 → Cluster health checks
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│          EFA & MiCS — QUICK RECALL                           │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  EFA (Elastic Fabric Adapter):                               │
│  → Network device for faster instance-to-instance comms      │
│  → On-prem HPC performance in the cloud                      │
│  → Uses NCCL (Nvidia library)                                │
│  → Requires Nvidia GPUs                                      │
│  → Set FI_PROVIDER = "efa"                                   │
│                                                              │
│  MiCS (Minimize Communication Scale):                        │
│  → Combines ALL distributed training techniques              │
│  → For training 1 trillion+ parameter models                 │
│  → Uses: sharded parallelism + EFA + P4d + FSx               │
│  → EC2 Ultra Cluster (4000+ GPUs)                            │
│                                                              │
│  Key instance: P4d                                           │
│  → 8 Nvidia A100 GPUs                                        │
│  → 80GB GPU memory each                                      │
│  → 400 Gbps networking                                       │
│                                                              │
│  EXAM TIPS:                                                  │
│  "Accelerate network between instances?" → EFA               │
│  "Train 1 trillion+ parameters?"         → MiCS              │
│  "On-prem HPC performance in cloud?"     → EFA               │
│  "Requires Nvidia GPUs?"                 → EFA + NCCL        │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
