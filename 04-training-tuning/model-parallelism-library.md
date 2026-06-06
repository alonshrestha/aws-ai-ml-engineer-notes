# SageMaker Model Parallelism Library (SMP)

## 🤔 Why Model Parallelism?

Your model is SO big that it won't fit on one GPU's memory.

```
GPU memory: 80GB
Your model: 200GB of weights

One GPU can't hold it. Split the model across multiple GPUs.
```

Or maybe the model fits, but you want extra GPU memory to increase batch size for faster training.

---

## 🔧 Three Key Techniques

### 1. Optimization State Sharding

"Optimization state" = just a fancy word for the model's weights.

Splits the weights across multiple GPUs.

```
Model has 10 billion weights:

  GPU 1: holds weights 1-2.5 billion
  GPU 2: holds weights 2.5-5 billion
  GPU 3: holds weights 5-7.5 billion
  GPU 4: holds weights 7.5-10 billion

  No single GPU needs to hold everything.
```

```
Key facts:
  → Requires stateful optimizer (Adam or FP16)
  → Only useful for models with 1 billion+ parameters
  → We're talking GPT-scale models here
```

### 2. Activation Checkpointing

During training, each layer produces "activations" (intermediate results). Storing all of them eats memory.

```
Without activation checkpointing:
  Layer 1 output: saved in memory
  Layer 2 output: saved in memory
  Layer 3 output: saved in memory
  ...
  Layer 50 output: saved in memory
  
  GPU memory: FULL 😩

With activation checkpointing:
  Layer 1 output: saved ✅
  Layer 2 output: CLEARED (save memory)
  Layer 3 output: saved ✅
  Layer 4 output: CLEARED (save memory)
  
  When needed later: recompute the cleared ones
  
  GPU memory: has room 🎉
```

```
Tradeoff:
  → Saves memory ✅
  → Costs extra computation ❌ (need to recompute cleared activations)
  → Trading memory for compute time
```

### 3. Activation Offloading

Move activations from GPU to CPU when not needed, bring them back when needed.

```
GPU memory full?
  → Move some activations to CPU memory (which is much bigger)
  → When GPU needs them back → swap them in

GPU (fast, small memory) ←→ CPU (slower, big memory)

Like moving boxes from your desk to a storage room.
Need them? Go grab them. But your desk stays clean.
```

---

## 📊 Sharded Data Parallelism

Combines data parallelism AND model parallelism together.

```
Regular data parallelism:
  → Each GPU has FULL copy of model
  → Each GPU gets different data
  → Problem: model is duplicated everywhere (wastes memory)

Sharded data parallelism:
  → Model weights SPLIT across GPUs (not duplicated)
  → Gradients SPLIT across GPUs
  → Data SPLIT across GPUs
  → Everything is shared, nothing is duplicated
```

```
Regular:  4 GPUs × full model copy = 4× memory used
Sharded:  4 GPUs sharing one model = 1× memory used
```

---

## 🔗 Framework Support

```
SageMaker Interleaved Pipelines: TensorFlow ✅ PyTorch ✅
SageMaker Model Parallelism (SMP): PyTorch ✅ (TensorFlow deprecated)

PyTorch is the way to go for distributed training.
```

### How to enable:

```
import torch.sagemaker as tsm
tsm.init()

In estimator:
  distribution = {
    "smdistributed": {
      "modelparallel": {
        "enabled": True
      }
    }
  }
```

Already included in Deep Learning Containers for PyTorch — nothing extra to install.

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│       MODEL PARALLELISM LIBRARY — QUICK RECALL               │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  When: Model too big for one GPU                             │
│                                                              │
│  Three techniques:                                           │
│                                                              │
│  1. Optimization State Sharding                              │
│     → Split weights across GPUs                              │
│     → Needs Adam/FP16 optimizer                              │
│     → For 1 billion+ parameter models                        │
│                                                              │
│  2. Activation Checkpointing                                 │
│     → Clear some activations to save memory                  │
│     → Recompute them when needed                             │
│     → Tradeoff: saves memory, costs compute                  │
│                                                              │
│  3. Activation Offloading                                    │
│     → Swap activations between GPU and CPU                   │
│     → CPU has more memory, use it as overflow                │
│                                                              │
│  Sharded Data Parallelism:                                   │
│  → Combines data + model parallelism                         │
│  → Splits weights, gradients, AND data across GPUs           │
│  → No duplication = saves memory                             │
│                                                              │
│  Framework: PyTorch (TensorFlow deprecated)                  │
│  Built into: Deep Learning Containers                        │
│                                                              │
│  EXAM TIPS:                                                  │
│  "Model won't fit on one GPU?"    → Model parallelism (SMP)  │
│  "Save GPU memory during train?"  → Activation checkpointing │
│  "Offload to CPU?"                → Activation offloading    │
│  "Split weights across GPUs?"     → Optimization sharding    │
│  "Combine data + model parallel?" → Sharded data parallelism │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
