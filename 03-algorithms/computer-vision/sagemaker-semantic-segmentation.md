# SageMaker Semantic Segmentation — Beginner Friendly Guide

## 🧠 What Is Semantic Segmentation?

It classifies **every single pixel** in an image. Not just "what's in the image" — but "what is THIS specific pixel part of?"

```
Three levels of computer vision (exam loves this!):

Image Classification:
  "There's a cat and a dog in this image"
  → Just labels. No location.

Object Detection:
  "Cat is HERE, dog is HERE" 
  → Labels + bounding boxes (rectangles)

Semantic Segmentation:
  "This pixel = cat, this pixel = cat, this pixel = dog..."
  → Labels for EVERY SINGLE PIXEL
  → Most detailed of the three
```

### What Does the Output Look Like?

You get back a "segmentation mask" — a colored map of the image:

```
Original Image:          Segmentation Mask:

┌──────────────────┐    ┌──────────────────┐
│   🧸    🌹       │    │ AAAA  BBBB       │
│   🧸  🌹🌹      │    │ AAAA BBBBB       │
│   🧸    🪣      │    │ AAAA  CCCC       │
│         🪣      │    │       CCCC       │
└──────────────────┘    └──────────────────┘

A = stuffed animal (every pixel)
B = rose (every pixel)
C = bucket (every pixel)

Each pixel gets a label — not just a box around it.
```

---

## 🚗 Why Would You Need Pixel-Level Detail?

```
Self-driving cars:
  → Need to know EXACTLY where the road ends and sidewalk begins
  → A bounding box isn't precise enough
  → Every pixel matters: road? person? tree? sign?

Medical imaging:
  → Find exact boundaries of a tumor
  → Not just "there's a tumor somewhere in this scan"
  → Need to know exactly which pixels are tumor

Robot sensing:
  → Robot needs to know exactly where objects are
  → Pixel-level precision for grabbing/avoiding things
```

---

## 📥 Input Format

### Training:

```
✅ JPEG or PNG images + annotations
✅ Label maps (describes what each annotation means)
✅ Augmented manifest image format (for pipe mode)
✅ Needs both training AND validation data

Pipe mode supported → stream from S3 for better performance
```

### Inference:

```
✅ Just give it a JPEG image
   → Returns the segmentation mask (pixel-level labels)
```

---

## ⚙️ What's Under the Hood?

Built on **MXNet** (specifically Gluon and GluonCV).

### Three Algorithm Choices:

```
1. FCN  (Fully Convolutional Network)
2. PSP  (Pyramid Scene Parsing)
3. DeepLab v3

Don't need to know how they work internally.
Just know the names exist as options.
```

### Base Network (Backbone):

```
ResNet-50  → smaller, faster
ResNet-101 → bigger, potentially more accurate

Both pre-trained on ImageNet (common everyday objects).
```

### Training Options:

```
From scratch:        → Train on entirely new objects
Transfer learning:   → Start with ImageNet, fine-tune with your data
Incremental:         → Continue training an existing model
```

---

## 🎛️ Key Hyperparameters

| Parameter | What It Does |
|-----------|-------------|
| `epochs` | How many passes through training data |
| `learning_rate` | How fast it learns |
| `batch_size` | Examples per training step |
| `optimizer` | How the model improves |
| `algorithm` | FCN, PSP, or DeepLab v3 |
| `backbone` | ResNet-50 or ResNet-101 |

The algorithm and backbone choices are important tuning decisions specific to semantic segmentation.

---

## 💻 What Machines to Use?

### Training:

```
✅ GPU only — no CPU training
❌ Single machine only — cannot distribute

Options: P2, P3, G4, G5
```

```
⚠️ Key difference from Object Detection and Image Classification:

Object Detection:       multi-machine ✅
Image Classification:   multi-machine ✅
Semantic Segmentation:  multi-machine ❌ (single only!)
```

### Inference:

```
✅ CPU: C5 or M5
✅ GPU: P3 or G4
```

---

## 🆚 The Three Computer Vision Algorithms Compared

| | Image Classification | Object Detection | Semantic Segmentation |
|--|---------------------|------------------|----------------------|
| What's in image? | ✅ Labels | ✅ Labels | ✅ Labels |
| Where in image? | ❌ | ✅ Bounding boxes | ✅ Every pixel |
| Detail level | Low | Medium | Highest |
| Use case | "Is this a cat?" | "Where is the cat?" | "Which exact pixels are cat?" |
| Multi-machine training? | ✅ | ✅ | ❌ |
| GPU for training? | ✅ | ✅ | ✅ |

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────┐
│        SEMANTIC SEGMENTATION — QUICK RECALL              │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  What is it?    Pixel-level classification               │
│                 Every pixel gets a label                  │
│                                                          │
│  Output?        Segmentation mask                        │
│                                                          │
│  Use cases?     Self-driving cars, medical imaging,      │
│                 robot sensing                             │
│                                                          │
│  vs others:     Classification = WHAT                    │
│                 Detection = WHAT + WHERE (boxes)          │
│                 Segmentation = WHAT + WHERE (every pixel) │
│                                                          │
│  Built on:      MXNet (Gluon/GluonCV)                   │
│                                                          │
│  Algorithms:    FCN, PSP, DeepLab v3                     │
│  Backbone:      ResNet-50 or ResNet-101 (ImageNet)       │
│                                                          │
│  Input:         JPEG/PNG + annotations                   │
│                 Supports pipe mode                        │
│                                                          │
│  Training:      GPU only                                 │
│                 Single machine only ❌ no multi-machine   │
│                 P2, P3, G4, G5                           │
│                                                          │
│  Inference:     CPU (C5/M5) or GPU (P3/G4)              │
│                                                          │
└──────────────────────────────────────────────────────────┘
```
