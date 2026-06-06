# SageMaker Input Modes - Getting Data into Training

## Why Input Modes Matter

When dealing with **massive datasets**, you need to consider:
- How quickly data gets to training instances
- Whether you have enough storage on training instances
- Redundant storage costs
- Training start time (waiting for data transfer)

---

## Input Modes Overview

```
┌─────────────────────────────────────────────────────┐
│ SAGEMAKER INPUT MODES                               │
└─────────────────────────────────────────────────────┘

S3 File Mode (Default)
├── Copies ALL data from S3 to local Docker container
├── Simple, good for small datasets
└── Limitations: Wait time + storage space needed

S3 Fast File Mode (Preferred)
├── Streams data from S3 (no full copy)
├── Supports random access
└── Best for most use cases

S3 Pipe Mode (Legacy)
├── Streams data sequentially from S3
├── No random access
└── Mostly replaced by Fast File Mode

High-Performance Options
├── S3 Express One Zone (faster S3 storage class)
├── FSx for Lustre (massive scale)
└── Amazon EFS (if data already there)
```

---

## S3 File Mode (Default)

**How it works:**
- Copies training data from S3 → local directory in Docker container

**Pros:**
- Simple to use
- Good for getting started

**Cons:**
- Must wait for copy to complete before training starts
- Need enough storage space on training instance
- Redundant storage (data exists in S3 AND locally)

**Use when:** Small datasets, simple experiments

---

## S3 Fast File Mode (Recommended)

**How it works:**
- Reads data directly from S3 on-the-fly as the training job needs it
- Only the chunks being actively used are pulled in — the full dataset never sits on local disk
- Think: streaming a movie (watch as it loads) vs downloading the whole movie first (File Mode)

**Pros:**
- No waiting for data download
- No local storage space needed for full dataset
- Supports **random access** (can access any data point)
- Works best with sequential access but handles random too

**Why preferred over Pipe Mode:**
- Random access capability
- More flexible

**Use when:** Large datasets, production workloads

---

## S3 Pipe Mode (Legacy)

**How it works:**
- Streams data sequentially from S3

**Pros:**
- No waiting for download
- No local storage needed

**Cons:**
- **Sequential access only** (no random access)
- Less flexible than Fast File Mode

**Use when:** Legacy workloads, strictly sequential data access

**Note:** Mostly replaced by Fast File Mode

---

## High-Performance Storage Options

### S3 Express One Zone

**What it is:** High-performance S3 storage class

**Key characteristics:**
- Single availability zone (no redundancy)
- Faster performance than standard S3
- Works with File, Fast File, and Pipe modes

**Use when:**
- Data is backed up elsewhere
- Need maximum speed
- Can tolerate single-AZ risk

### Amazon FSx for Lustre

**What it is:** High-performance parallel file system

**Key characteristics:**
- Hundreds of GB/s throughput
- Millions of IOPS
- Low latency
- Single availability zone
- **Requires VPC**

**Use when:**
- Training large language models (LLMs)
- Massive datasets (TB/PB scale)
- Need extreme performance

### Amazon EFS

**What it is:** Elastic File System (shared NFS)

**Key characteristics:**
- If data already in EFS, can use directly
- **Requires VPC**

**Use when:** Data already exists in EFS

---

## Comparison Table

| Mode | Data Transfer | Storage Needed | Access Pattern | Best For |
|------|--------------|----------------|----------------|----------|
| File Mode | Full copy | Yes (full dataset) | Any | Small datasets |
| Fast File Mode | Streaming | No | Random + Sequential | Most use cases |
| Pipe Mode | Streaming | No | Sequential only | Legacy |

| Storage Option | Performance | Availability | Requirements |
|----------------|-------------|--------------|--------------|
| Standard S3 | Good | Multi-AZ | None |
| S3 Express One Zone | Very High | Single AZ | None |
| FSx for Lustre | Extreme | Single AZ | VPC |
| EFS | Good | Multi-AZ | VPC |

---

## For the Exam - Key Points

### Input Mode Selection
- **Default (File Mode)**: Copies data, simple but slow for large data
- **Fast File Mode**: Streaming, random access, preferred for large datasets
- **Pipe Mode**: Streaming, sequential only, legacy option

### Performance Hierarchy
```
Fastest → Slowest:
FSx for Lustre > S3 Express One Zone > S3 Fast File > S3 File Mode
```

### Trade-offs to Remember
- **File Mode**: Simple but needs storage + wait time
- **Fast File Mode**: No storage needed, supports random access
- **Pipe Mode**: No storage needed, sequential only
- **S3 Express One Zone**: Fast but single AZ (no redundancy)
- **FSx for Lustre**: Fastest but requires VPC + single AZ

### Common Scenarios
- **Scenario**: Small dataset, quick experiment → File Mode
- **Scenario**: Large dataset, need flexibility → Fast File Mode
- **Scenario**: Training LLM with massive data → FSx for Lustre
- **Scenario**: Need faster S3, data backed up elsewhere → S3 Express One Zone
- **Scenario**: Data already in EFS → Use EFS directly

### VPC Requirements
- FSx for Lustre: **Requires VPC**
- Amazon EFS: **Requires VPC**
- S3 modes: No VPC required
