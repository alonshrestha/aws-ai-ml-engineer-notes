# SageMaker Image Classification — Beginner Friendly Guide

## 🧠 What Is Image Classification?

You give it an image. It tells you **what's in it**. That's it — no location, no bounding boxes.

```
Object Detection:                Image Classification:

┌──────────────────────┐        ┌──────────────────────┐
│                      │        │                      │
│  ┌─────┐  ┌──────┐  │        │                      │
│  │ cat │  │ dog  │  │        │    (same image)      │
│  │     │  │      │  │        │                      │
│  └─────┘  └──────┘  │        │                      │
│                      │        │                      │
└──────────────────────┘        └──────────────────────┘
Output: cat at (10,20)          Output: cat, dog
        dog at (150,20)         (just labels, no location)
        + bounding boxes
```

```
🎯 Exam question alert:

"What's the difference between Object Detection and Image Classification?"

Object Detection  → WHAT + WHERE (labels + bounding boxes)
Image Classification → WHAT only (just labels)
```

Use image classification when you just need to know what's in the picture, not where it is.

---

## 📦 Two Versions in SageMaker

Same setup as object detection:

```
1. Image Classification — MXNet (older)
   → Specific model, well-defined

2. Image Classification — TensorFlow (newer, more choices)
   → Pick from many models in TensorFlow Hub
```

---

## 🔵 MXNet Version

### Two Training Modes:

```
Full Training Mode:
  → Starts from scratch
  → All weights are random
  → Needs lots of data and time

Transfer Learning Mode:
  → Starts with pre-trained weights (from ImageNet)
  → Only the TOP layer starts with random weights
  → Fine-tune with YOUR data
  → Much faster!
```

```
Transfer Learning is like:

Instead of teaching someone photography from zero...
  → Take a photographer who already knows cameras
  → Just teach them YOUR specific style
  → Way faster!
```

### Default Image Size:

```
3 channels (RGB: Red, Green, Blue) × 224 × 224 pixels

Why 224×224? Because that's what ImageNet uses.
Your images get resized to this.
```

---

## 🟠 TensorFlow Version

Pick from many pre-trained models in TensorFlow Hub:

```
Available models:
  → MobileNet (various versions)
  → Inception
  → ResNet
  → EfficientNet
  → ... and more

Each has different trade-offs (speed vs accuracy).
Details vary by model — check docs for the one you pick.
```

You can still fine-tune any of these — the top classification layer is available for further training with your own data.

```
Pre-trained model knows: cats, dogs, cars, trees...
You fine-tune it with: YOUR specific product images
Now it knows: your products too! 
```

---

## 🎛️ Key Hyperparameters

Standard deep learning settings (exact names vary by model):

| Parameter | What It Does |
|-----------|-------------|
| `batch_size` | Examples per training step |
| `learning_rate` | How fast it learns |
| `optimizer` | How the model improves |
| `weight_decay` | L2 regularization (prevent overfitting) |
| `beta_1`, `beta_2`, `eps`, `gamma` | Optimizer-specific fine-tuning |

Nothing surprising here — same knobs as any neural network.

---

## 💻 What Machines to Use?

### Training:

```
✅ GPU required
✅ Multi-GPU on one machine — works
✅ Multiple GPU machines — also works!

Options: P2, P3, G4, G5

Same as Object Detection — image stuff needs GPU power.
```

### Inference:

```
✅ CPU: M5
✅ GPU: P2, P3, G4, G5
```

---

## 🆚 Quick Comparison: Object Detection vs Image Classification

| | Object Detection | Image Classification |
|--|-----------------|---------------------|
| What's in the image? | ✅ Yes | ✅ Yes |
| Where in the image? | ✅ Yes (bounding boxes) | ❌ No |
| Confidence scores? | ✅ Yes | ✅ Yes |
| Two versions? | MXNet + TensorFlow | MXNet + TensorFlow |
| GPU for training? | ✅ Yes | ✅ Yes |
| Transfer learning? | ✅ Yes | ✅ Yes |

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────┐
│         IMAGE CLASSIFICATION — QUICK RECALL              │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  What is it?    Labels what's in an image                │
│                 NO bounding boxes, NO location            │
│                                                          │
│  vs Object      Object Detection = WHAT + WHERE          │
│  Detection:     Image Classification = WHAT only         │
│                                                          │
│  Two versions:  MXNet and TensorFlow                     │
│                                                          │
│  MXNet:         Full training (random weights)           │
│                 Transfer learning (pre-trained weights)   │
│                 Default: 3-channel 224×224 (ImageNet)    │
│                                                          │
│  TensorFlow:    Many models (MobileNet, Inception,       │
│                 ResNet, EfficientNet...)                  │
│                 Top layer available for fine-tuning       │
│                                                          │
│  Training:      GPU required                             │
│                 Multi-GPU ✅  Multi-machine ✅            │
│                 P2, P3, G4, G5                           │
│                                                          │
│  Inference:     CPU (M5) or GPU (P2/P3/G4/G5)           │
│                                                          │
└──────────────────────────────────────────────────────────┘
```
