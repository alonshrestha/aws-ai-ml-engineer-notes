# EC2 for AI — GPU, Trainium & Inferentia

## CPU vs GPU (Quick Recap)
- **CPU** = few cores, can do complex varied tasks
- **GPU** = thousands of cores, can only do simple math but all at once
- AI/ML is mostly repetitive math at massive scale → GPU wins

**What "repetitive math" means:**
- Same operation on **millions of different data points**
- Example: Training image model on 1 million images — for each image, the model does the same math (multiply pixels by weights, add up, check result). Same formula, different data each time.
- GPU handles this perfectly — give each core a different image, all cores run the same math at the same time

## GPU-Based EC2 Instances
- **P family** (P3, P4, P5) and **G family** (G3, G6) have powerful GPUs
- Good for ML training and inference

## AWS Custom ML Chips

### AWS Trainium (for Training)
- Custom chip built by AWS specifically for **training** deep learning models (100B+ parameters)
- EC2 instance type: **Trn1**
- Trn1 has 16 Trainium accelerators
- **50% cost reduction** compared to regular GPU instances

### AWS Inferentia (for Inference)
- **Inference** = using a trained model to make predictions on new data (training = studying, inference = taking the exam)
- Custom chip built by AWS specifically for **serving/inference**
- EC2 instance types: **Inf1, Inf2**
- **4x throughput** and **70% cost reduction** compared to regular GPU instances

## Exam Key Points
- GPU families: P and G
- Trainium = training, Inferentia = inference (name gives it away)
- Both Trainium and Inferentia have the **lowest environmental footprint** (most efficient)
- Trainium and Inferentia instances can be used **directly via EC2** (you manage everything) or **through SageMaker** (managed for you) — same chips, different level of control
