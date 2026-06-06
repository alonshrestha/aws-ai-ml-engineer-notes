# TensorBoard with SageMaker

## 📊 What Is TensorBoard?

A visualization tool for watching what happens during training. Originally built for TensorFlow, now works with PyTorch too.

```
Training is happening... but what's going on inside?

TensorBoard shows you:
  → Loss going down over time? 📉 (good)
  → Accuracy going up? 📈 (good)
  → Weights changing? How?
  → Model structure (graph visualization)
```

---

## 🔍 What It Can Visualize

```
1. Loss & Accuracy     → Charts showing trends over epochs
2. Model Graph         → Visual diagram of your neural network structure
3. Weights & Biases    → Histograms showing how they change over time
4. Embeddings          → Project high-dimensional vectors into 2D/3D
                         (so your brain can actually see them)
5. Profiling           → Optimize training performance
```

### Embeddings visualization:

```
Embedding vector: [0.2, 0.8, 0.1, 0.5, ... 0.3]  ← 300 dimensions
Your brain: "I can't picture 300 dimensions"

TensorBoard: Projects it down to 2D or 3D
             Now you can see clusters and patterns
             on a simple scatter plot
```

---

## 🔗 Integration with SageMaker

```
Two ways to access:
  → Through SageMaker Studio console
  → Via a URL that SageMaker generates for you

Requires: Small modifications to your training script
          (exam won't ask specifics on this)

Works with: TensorFlow ✅  PyTorch ✅
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│          TENSORBOARD — QUICK RECALL                          │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  What: Visualization tool for training                       │
│  Origin: TensorFlow (now works with PyTorch too)             │
│  Shows: Loss, accuracy, weights, model graph, embeddings     │
│  Access: SageMaker console or web URL                        │
│  Note: Requires training script modifications                │
│                                                              │
│  EXAM TIP:                                                   │
│  "Visualize training metrics?"        → TensorBoard          │
│  "View model graph?"                  → TensorBoard          │
│  "Visualize embeddings in 2D/3D?"     → TensorBoard          │
│                                                              │
│  Don't confuse with:                                         │
│  Debugger → monitors + alerts + auto-stops                   │
│  TensorBoard → visualizes (just shows you charts)            │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
