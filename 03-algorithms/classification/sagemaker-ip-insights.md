# SageMaker IP Insights — Beginner Friendly Guide

## 🧠 What Is IP Insights?

It finds **suspicious activity** in your web logs by learning which IP addresses are normal for each user.

```
Normal behavior:
  User "john" always logs in from IP 192.168.1.10 (his home)
  User "john" always logs in from IP 10.0.0.5 (his office)

Suspicious behavior:
  User "john" suddenly logs in from IP 45.33.99.200 (unknown location!)
  
  IP Insights: "That's fishy! 🐟 Flag it!"
```

It's basically a security tool that watches patterns and says "this doesn't look right."

---

## ✅ What Can It Detect?

```
1. Login attempts from unusual IP addresses
   → "John never logs in from Russia. Why now?"

2. Resource creation from unusual IPs
   → "Someone is spinning up servers from a new IP"

3. Any suspicious entity + IP combination
   → Anything that breaks the normal pattern
```

---

## 🔍 How Does It Work?

It's **unsupervised** — you don't need to label what's suspicious. It learns normal patterns and flags anything that doesn't fit.

```
Step 1: Feed it your web logs (user + IP pairs)
        john, 192.168.1.10
        john, 10.0.0.5
        sarah, 172.16.0.1
        sarah, 172.16.0.2
        ... millions of these

Step 2: Neural network learns patterns
        "john usually uses these IPs"
        "sarah usually uses those IPs"

Step 3: New login comes in
        john, 45.33.99.200
        
        IP Insights: "john has NEVER used this IP → anomaly score: HIGH"
```

### Neat Trick: Auto-Generated Fake Data

Most of your data is normal (not suspicious). So IP Insights creates fake suspicious examples by randomly pairing users with random IPs:

```
Real data:    john + 192.168.1.10  (normal)
Fake data:    john + 88.77.66.55   (randomly paired → probably suspicious)

This helps it learn what "suspicious" looks like
without you having to label anything.
```

---

## 📥 Input Format

Super simple:

```
✅ CSV only
   Just two columns: entity, IP address

Example:
  john,192.168.1.10
  john,10.0.0.5
  sarah,172.16.0.1
  sarah,172.16.0.2

Entity can be: username, account ID, session ID — any identifier.
No preprocessing needed — it takes usernames and IPs directly.
```

```
Training channel: required
Validation channel: optional (measures AUC score)
```

---

## 🎛️ Key Hyperparameters

| Parameter | What It Does | Tip |
|-----------|-------------|-----|
| `num_entity_vectors` (hash size) | How many unique entities it can handle | Set to 2× the number of unique users/accounts |
| `vector_dim` | Size of embedding vectors | Too large → overfitting |
| `epochs` | Training passes | Standard neural network tuning |
| `learning_rate` | How fast it learns | Standard neural network tuning |
| `batch_size` | Examples per step | Standard neural network tuning |

### About Hash Size:

```
You have 10,000 unique users
→ Set num_entity_vectors to at least 20,000 (2×)

Too small = entities collide = bad results
```

---

## 💻 What Machines to Use?

```
✅ GPU recommended (it's a neural network)
   → P3.2xlarge or higher
   → Multi-GPU on one machine works

✅ CPU also works
   → Size depends on your hyperparameter choices
```

---

## 🎯 Exam Cheat Sheet

```
┌──────────────────────────────────────────────────────────┐
│             IP INSIGHTS — QUICK RECALL                   │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  What is it?    Finds suspicious IP address activity     │
│                 Security tool for web logs                │
│                                                          │
│  Supervised?    NO — unsupervised                        │
│                                                          │
│  Input?         CSV only (entity + IP address)           │
│                 No preprocessing needed                   │
│                                                          │
│  Under hood?    Neural network with embeddings           │
│                 Auto-generates fake suspicious data       │
│                                                          │
│  Key param:     num_entity_vectors (hash size)           │
│                 Set to 2× unique entities                │
│                 vector_dim (too large = overfitting)      │
│                                                          │
│  Instances:     GPU recommended (P3.2xlarge+)            │
│                 Multi-GPU on one machine ✅               │
│                                                          │
│  #1 exam tip:   Suspicious IP / security → IP Insights   │
│                                                          │
└──────────────────────────────────────────────────────────┘
```
