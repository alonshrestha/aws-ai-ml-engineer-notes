# SageMaker Object Detection — Beginner Friendly Guide

## 🧠 What Is Object Detection?

You give it an image. It tells you **what objects are in it** and **where they are**.

```
Input: A photo of an office

Output:
  ┌─────────────────────────────────┐
  │                                 │
  │  ┌──────┐     ┌─────────┐      │
  │  │ wine │     │ laptop  │      │
  │  │bottle│     │  95%    │      │
  │  │ 92%  │     └─────────┘      │
  │  └──────┘  ┌──────┐            │
  │            │chair │            │
  │  ┌─────┐  │ 88%  │            │
  │  │wine │  └──────┘            │
  │  │glass│                       │
  │  │ 87% │     ┌──────┐         │
  │  └─────┘     │ bowl │         │
  │              │ 76%  │         │
  │              └──────┘         │
  └─────────────────────────────────┘

For each object it gives you:
  1. What it is (wine bottle, chair, laptop...)
  2. Where it is (bounding box / rectangle)
  3. How sure it is (confidence score: 92%, 88%...)
```

Uses a deep neural network under the hood. You can train from scratch or use pre-trained models.

---

## 📦 Two Versions in SageMaker

SageMaker has TWO object detection algorithms:

```
1. Object Detection — MXNet (older, original)
   → Uses specific CNN models
   → More rigid but well-defined

2. Object Detection — TensorFlow (newer, more choices)
   → Pick from many different models
   → More flexible
```

---

## 🔵 MXNet Version

### What's Under the Hood:

```
Uses: CNN (Convolutional Neural Network)
Algorithm: SSD (Single Shot Multibox Detector)

Base network (pick one):
  → VGG-16
  → ResNet-50
```

Don't need to know what SSD/VGG/ResNet mean deeply — just know the names for the exam.

### Training Modes:

```
Transfer Learning:
  → Start with a pre-trained model (trained on ImageNet)
  → Fine-tune it with YOUR images
  → Much faster than training from scratch

Incremental Training:
  → Continue training an already-trained model
  → Add more data without starting over
```

### Avoiding Overfitting:

MXNet version automatically uses these tricks internally:

```
Flip:    → flips images horizontally (mirror)
Rescale: → changes image size slightly
Jitter:  → adds small random changes

Why? So the model doesn't just memorize exact images.
A cat flipped sideways is still a cat!
```

### Input Format (MXNet):

```
Option 1: MXNet RecordIO format

Option 2: Image files (JPEG or PNG) + JSON annotation file
```

The JSON annotation tells the model what's in each image:

```json
{
  "file": "office.jpg",
  "image_size": [800, 600],
  "annotations": [
    {"class": "wine_bottle", "box": [10, 20, 50, 150]},
    {"class": "laptop", "box": [200, 100, 400, 300]},
    {"class": "chair", "box": [300, 200, 500, 500]}
  ]
}
```

You're basically saying: "In this image, here's what's in it and where."

---

## 🟠 TensorFlow Version

More flexible — you pick from many models:

```
Available models (from TensorFlow Model Garden):
  → ResNet (various versions)
  → EfficientNet
  → MobileNet
  → ... and more

Each model has different strengths.
Configuration varies based on which model you pick.
```

Think of TensorFlow version as a wrapper that lets you use popular pre-built models easily inside SageMaker.

Input format depends on which model you select.

---

## 🎛️ Key Hyperparameters

| Parameter | MXNet Name | TensorFlow Name | What It Does |
|-----------|-----------|-----------------|-------------|
| Batch size | `mini_batch_size` | `batch_size` | Examples per training step |
| Learning rate | `learning_rate` | `learning_rate` | How fast it learns |
| Optimizer | choose from many | choose from many | How the model improves |

Same names, same ideas — just standard deep learning tuning.

---

## 💻 What Machines to Use?

### Training (heavy work — needs GPU):

```
✅ GPU instances required
✅ Multi-GPU on one machine — works
✅ Multiple GPU machines — also works!

Recommended starting points:
  → P2.xlarge
  → P3.2xlarge

Need more power?
  → P2.16xlarge or P3.16xlarge
  → G4 and G5 instance types also available
```

### Inference (making predictions):

```
✅ CPU: M5 recommended
✅ GPU: P2, P3, G4 all okay
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────┐
│           OBJECT DETECTION — QUICK RECALL                │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  What is it?    Finds objects in images                  │
│                 Returns: what + where + confidence score  │
│                                                          │
│  Two versions:  MXNet (SSD with VGG-16 or ResNet-50)    │
│                 TensorFlow (many models to choose from)  │
│                                                          │
│  MXNet input:   RecordIO or JPEG/PNG + JSON annotations  │
│  TF input:      Varies by model                          │
│                                                          │
│  Pre-trained?   Yes — ImageNet (MXNet)                   │
│                 TensorFlow Model Garden (TF)              │
│                                                          │
│  Overfitting:   MXNet auto-applies flip, rescale, jitter │
│                                                          │
│  Training:      GPU required                             │
│                 Multi-GPU ✅  Multi-machine ✅            │
│                 P2/P3 recommended, G4/G5 also available  │
│                                                          │
│  Inference:     CPU (M5) or GPU (P2/P3/G4)              │
│                                                          │
└──────────────────────────────────────────────────────────┘
```
