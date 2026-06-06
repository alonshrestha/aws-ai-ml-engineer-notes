# Distributed Training in SageMaker

## 🤔 Why Distribute Training?

When your model is so big that one machine can't handle it.

```
Small model:  Fits on 1 GPU → train normally
Large model:  Needs 8 GPUs → use a bigger instance (P4d xlarge)
Huge model:   Needs 100+ GPUs → distribute across multiple instances
```

### Important rule: Max out ONE instance first!

```
⚠️ Before going multi-instance:

  Single instance with 8 GPUs → ALWAYS faster and cheaper
  than 8 instances with 1 GPU each

  Why? No network overhead between machines.
  GPUs in one box talk to each other much faster 
  than GPUs across different machines.

  Go multi-instance ONLY when one instance isn't enough.
```

---

## 📦 Three Types of Parallelism

```
1. Job Parallelism
   → Training multiple DIFFERENT models at the same time
   → Model A on Instance 1, Model B on Instance 2
   → Simple, intuitive

2. Data Parallelism
   → ONE model, but split the TRAINING DATA across instances
   → Each instance trains on a chunk of data
   → Results combined at the end

3. Model Parallelism
   → The model itself is TOO BIG for one instance
   → Split the MODEL across instances
   → Layer 1-5 on Instance A, Layer 6-10 on Instance B
```

```
Job parallelism:   different models, different machines
Data parallelism:  same model, different data chunks
Model parallelism: same model split across machines (it's THAT big)
```

---

## 📊 Data Parallelism (Most Common)

Each instance gets a copy of the model but different chunks of training data. They all compute gradients, then combine results.

```
Training data: [A, B, C, D, E, F, G, H]

Instance 1 (full model copy): trains on [A, B]  → gradients
Instance 2 (full model copy): trains on [C, D]  → gradients
Instance 3 (full model copy): trains on [E, F]  → gradients
Instance 4 (full model copy): trains on [G, H]  → gradients
                                                      ↓
                                              Combine all gradients
                                                      ↓
                                              Update model weights
                                                      ↓
                                              Next round
```

Think of it like MapReduce but for gradient descent:

```
MapReduce:           Distribute data processing
Data Parallelism:    Distribute gradient computation

Same idea, different application.
```

### SageMaker DDP Library (Distributed Data Parallelism):

Two components:

```
AllReduce Collective:
  → Distributes gradient computation across GPUs
  → Sends gradients to/from GPUs across instances
  → The core of data parallelism

AllGather Collective:
  → Manages communication between nodes
  → Offloads communication to CPU (frees up GPU)
  → Improves performance
```

### How to enable:

```
In training script:
  → Set backend to "smddp" in torch.distributed.init_process_group

In PyTorch estimator:
  → distribution = {"smdistributed": {"dataparallel": {"enabled": True}}}
```

---

## 🧩 Model Parallelism

When the model itself won't fit on one machine. Billions/trillions of parameters.

```
Model has 100 layers. One GPU can hold 25 layers.

Instance 1: Layers 1-25
Instance 2: Layers 26-50
Instance 3: Layers 51-75
Instance 4: Layers 76-100

Data flows through: Instance 1 → 2 → 3 → 4
```

This is for truly massive models like GPT-scale. Most people won't need this.

---

## 🔧 Other Distributed Training Libraries

Not locked into SageMaker's library. You can use others:

```
┌─────────────────────┬────────────────────────────────────┐
│ Library             │ Notes                              │
├─────────────────────┼────────────────────────────────────┤
│ SageMaker DDP       │ AWS native, AllReduce + AllGather  │
│ PyTorch DDP         │ PyTorch's own distributed library  │
│ Torch Run           │ Requires P3, P4, or Trn1 instances│
│ DeepSpeed           │ Microsoft, open source, PyTorch    │
│ Horovod             │ Know the name (might appear on exam│
│ MPI Run             │ Message Passing Interface          │
└─────────────────────┴────────────────────────────────────┘

Most are PyTorch-specific. PyTorch is the way to go for 
distributed training these days.
```

---

## ⚠️ Incompatibility Note

```
SageMaker DDP library ❌ NOT compatible with Training Compiler
(But Training Compiler is being phased out anyway)
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│          DISTRIBUTED TRAINING — QUICK RECALL                 │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  RULE: Max out single instance BEFORE going multi-instance   │
│                                                              │
│  Three types:                                                │
│  → Job parallelism: different models on different machines   │
│  → Data parallelism: same model, split data across machines  │
│  → Model parallelism: model too big, split model itself      │
│                                                              │
│  SageMaker DDP Library:                                      │
│  → AllReduce: distributes gradient computation               │
│  → AllGather: offloads communication to CPU                  │
│  → Backend: "smddp"                                          │
│                                                              │
│  Other options: PyTorch DDP, DeepSpeed, Horovod, MPI         │
│                                                              │
│  EXAM TIPS:                                                  │
│  "Data too big for one instance?"     → Data parallelism     │
│  "Model too big for one instance?"    → Model parallelism    │
│  "Multiple different models?"         → Job parallelism      │
│  "Offload comms to CPU?"             → AllGather collective  │
│  "Max out single instance first?"     → YES, always          │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
