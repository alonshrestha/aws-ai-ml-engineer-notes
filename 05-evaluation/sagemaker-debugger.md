# SageMaker Debugger

## 🔍 What Is It?

Debugger watches your model WHILE it's training and tells you if something goes wrong.

```
Without Debugger:
  Train for 5 hours → finish → "Oh, something went wrong at hour 2"
  → Wasted 3 hours of compute. Money gone. 💸

With Debugger:
  Training... hour 1 ✅... hour 2 ⚠️ "Gradient exploding!"
  → Auto stops training or alerts you immediately
  → Saved 3 hours of wasted compute ✅
```

---

## 🧠 What It Does

It saves snapshots of your model's internal state during training — gradients, weights (tensors) — so you can see what's happening over time.

```
Epoch 1:  gradients = [0.5, 0.3, 0.4]    → saved ✅
Epoch 2:  gradients = [0.4, 0.2, 0.3]    → saved ✅
Epoch 3:  gradients = [0.001, 0.0005, 0.0002] → saved ⚠️ vanishing!
Epoch 4:  gradients = [0.00001, ...]      → RULE TRIGGERED 🚨

Debugger: "Vanishing gradient detected at epoch 3!"
```

---

## 📋 Rules — The Core Feature

You define rules = "watch for this bad thing." Debugger monitors and fires alerts when rules are triggered.

```
You set up:
  Rule 1: "Alert me if gradients vanish"
  Rule 2: "Alert me if GPU usage drops below 50%"
  Rule 3: "Alert me if loss stops decreasing"

Each rule → runs as its own debug job
When triggered → logs a CloudWatch event
```

### Three categories of built-in rules:

```
1. System Bottlenecks
   → CPU usage too high?
   → GPU sitting idle?
   → Memory running out?
   → "Your GPU is at 10% — you're wasting money"

2. Framework Operations
   → Profiling TensorFlow/PyTorch internals
   → Initialization taking too long?
   → Training steps have outliers?

3. Model Parameters
   → Vanishing gradients?
   → Exploding gradients?
   → Weights going to zero?
   → Overfitting detected?
```

---

## 🚨 Three Built-in Actions When Rule Triggers

```
Rule triggered → What happens?

  1. Stop Training    → Automatically stops the job
  2. Send Email       → Via SNS (Simple Notification Service)
  3. Send SMS         → Via SNS (text to your phone)
```

```
Example:
  Rule: "If vanishing gradient detected"
  Action: Stop training + send email

  Training... epoch 5... vanishing gradient! 🚨
  → Training stops automatically
  → You get an email: "Training stopped — vanishing gradient at epoch 5"
```

---

## 🖥️ Debugger Insights Dashboard

Visual dashboard in SageMaker Studio that shows everything graphically:

```
┌─────────────────────────────────────────────┐
│  Debugger Insights Dashboard                │
│                                             │
│  GPU Usage:    ████████░░ 80%               │
│  CPU Usage:    ██████░░░░ 60%               │
│  Memory:       ███████░░░ 70%               │
│                                             │
│  Gradient Trend:  📈 healthy                │
│  Loss Trend:      📉 decreasing (good)      │
│  Training Speed:  ⚡ 45 samples/sec         │
│                                             │
│  Rules Status:                              │
│  ✅ No vanishing gradient                   │
│  ✅ No exploding gradient                   │
│  ⚠️ GPU underutilized (rule triggered)      │
│                                             │
└─────────────────────────────────────────────┘
```

Can also auto-generate training reports — just check a box and get a detailed report of what happened during training.

---

## 🔧 Supported Frameworks & Algorithms

```
Frameworks:
  → TensorFlow  ✅
  → PyTorch     ✅
  → MXNet       ✅

Algorithms:
  → XGBoost     ✅
  → SageMaker Generic Estimator ✅ (works with anything)
  → Custom training containers   ✅
```

### Client Library:

```
Library name: smdebug

What it does:
  → Register hooks in your training code
  → Feed training data back to Debugger
  → Works with Create Training Job API

Used when: You have custom training code and want 
           Debugger to monitor it
```

---

## 🧭 Where Debugger Fits

```
Phase 4: Train & Tune

  You start training...
  
  Debugger watches:
    → Are gradients healthy?        (model parameters)
    → Is GPU being used properly?   (system bottlenecks)
    → Is the framework running well? (framework operations)
  
  Something wrong? → Alert or stop automatically
  Everything good? → Training continues normally
```

Think of it like a nurse monitoring a patient during surgery:

```
Surgeon (training):     Doing the work
Nurse (debugger):       Watching vital signs
Heart rate drops:       Nurse alerts surgeon immediately
Everything normal:      Nurse stays quiet, surgery continues
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│          SAGEMAKER DEBUGGER — QUICK RECALL                   │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  What is it?                                                 │
│  → Monitors training in real-time                            │
│  → Saves gradients/tensors at intervals                      │
│  → Fires alerts when rules are triggered                     │
│                                                              │
│  Three rule categories:                                      │
│  → System bottlenecks (CPU, GPU, memory)                     │
│  → Framework operations (TensorFlow, PyTorch profiling)      │
│  → Model parameters (gradients, weights, overfitting)        │
│                                                              │
│  Three actions on trigger:                                   │
│  → Stop training                                             │
│  → Send email (via SNS)                                      │
│  → Send SMS (via SNS)                                        │
│                                                              │
│  Supported: TensorFlow, PyTorch, MXNet, XGBoost              │
│  Client library: smdebug                                     │
│  Dashboard: Debugger Insights in SageMaker Studio            │
│  Events logged to: CloudWatch                                │
│                                                              │
│  EXAM TIPS:                                                  │
│  "Monitor training in real-time?"    → Debugger              │
│  "Auto-stop bad training?"           → Debugger rules        │
│  "Profile GPU usage during training?"→ Debugger profiler     │
│  "Get notified of training issues?"  → Debugger + SNS        │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```
