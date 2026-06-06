# SageMaker Neo — Edge Deployment

> Deploy trained models to edge devices (cars, cameras, embedded systems).

---

## What is Neo?

**Problem:** You trained a model in the cloud. Now you want it running locally on a device (self-driving car, smart camera, drone) — not calling the internet every time.

**Neo = a compiler that converts your trained model into optimized code for edge devices.**

Train once in the cloud → Compile with Neo → Run anywhere on the edge.

---

## Why Edge?

**Latency.** A self-driving car can't wait 200ms for an internet response to decide whether to brake. The model needs to run *inside* the car, instantly.

Other use cases:
- Smart cameras (DeepLens) doing real-time object detection
- Industrial sensors making decisions without internet
- Drones processing images mid-flight
- Any device where internet is slow, unreliable, or too slow for the task

---

## What Neo Supports

### Frameworks (input)
- TensorFlow
- MXNet
- PyTorch
- ONNX
- XGBoost

### Target Architectures (output)
- ARM processors
- Intel processors
- NVIDIA processors

---

## Neo Components

| Component | What it does |
|-----------|-------------|
| **Compiler** | Recompiles your model code into bytecode for the target edge processor |
| **Runtime** | Runs on the edge device, executes the Neo-compiled model |

---

## Neo + IoT Greengrass (Key Combo)

**Neo compiles the model. Greengrass delivers it to the device.**

```
SageMaker (cloud)
    │
    ├── Train model
    │
    ├── Compile with Neo (target: ARM/Intel/NVIDIA)
    │
    └── Deploy via IoT Greengrass → Edge Device
                                        │
                                        ├── Runs inference locally
                                        ├── Uses local data
                                        └── No internet needed
```

**Greengrass** = the delivery mechanism that pushes code to edge devices
**Lambda functions** = used extensively within Greengrass for inference applications

---

## Neo Hosting Options

1. **Cloud endpoint** — host Neo-compiled model on C5, M5, M4, P3, or P2 instances
   - Must match the instance type you compiled for
   - Kind of defeats the purpose (why compile for edge if hosting in cloud?)

2. **Edge device via Greengrass** — the real use case
   - Model runs locally on the device
   - No internet round-trip
   - Real-time inference

---

## Exam Tips

- **Neo** = "train once, run anywhere" — compiles models for edge devices
- **Neo + IoT Greengrass** = the pair to remember (Neo compiles, Greengrass deploys)
- **Lambda** = used within Greengrass for inference at the edge
- Supports: TensorFlow, MXNet, PyTorch, ONNX, XGBoost
- Target architectures: ARM, Intel, NVIDIA
- Use case keyword: **latency-sensitive** or **edge inference** → think Neo
- If hosting Neo model in cloud, instance type must match what you compiled for
- Neo has two parts: **compiler** + **runtime**

---

## Memory Aid

**Neo** (from The Matrix) = code is everywhere around you. Neo deploys ML code to the devices surrounding you in daily life.

**Combo to remember:** SageMaker → Neo → Greengrass → Lambda → Edge Device
