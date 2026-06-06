# SageMaker Workflow - Data Preparation, Training, and Deployment

## The Complete ML Pipeline in SageMaker

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   DATA      │ -> │  TRAINING   │ -> │ DEPLOYMENT  │
│ PREPARATION │    │             │    │             │
└─────────────┘    └─────────────┘    └─────────────┘
```

---

## 1. DATA PREPARATION

### Data Sources

**Primary Source: Amazon S3**
```
Most common: S3 buckets
- Raw data stored here
- Processed data stored here
- Easy integration with SageMaker
```

**Alternative: FSx for Lustre**
```
For high-performance needs:
- Very large datasets (TB/PB scale)
- High-throughput requirements
- Parallel processing workloads
```

**Other Data Sources:**
```
✅ Amazon Athena (query data in S3 using SQL)
✅ Amazon EMR (Elastic MapReduce - big data processing)
✅ Amazon Redshift (data warehouse)
✅ Amazon Keyspaces (Cassandra-compatible database)
✅ Apache Spark (distributed processing)
```

### Data Formats

**Format depends on the algorithm you're using!**

```
Common formats:
├── RecordIO-Protobuf (efficient binary format)
│   └── Used by: Many built-in algorithms
│
├── CSV (comma-separated values)
│   └── Used by: XGBoost, Linear Learner
│
├── Parquet (columnar format)
│   └── Used by: Efficient for large datasets
│
├── JSON/JSON Lines
│   └── Used by: Text-based algorithms
│
└── Image formats (JPEG, PNG)
    └── Used by: Computer vision algorithms
```

**Why different formats?**
- RecordIO-Protobuf: Faster to read, more efficient
- CSV: Easy to work with, human-readable
- Parquet: Columnar storage, great for analytics
- Each algorithm has recommendations for best performance

### Data Processing Tools

**Python Libraries (Available in SageMaker):**
```python
import pandas as pd      # Data manipulation
import numpy as np       # Numerical operations
import sklearn           # Machine learning utilities

# All pre-installed in SageMaker notebooks!
```

**Apache Spark Integration:**
```python
from pyspark.sql import SparkSession

# Use Spark for distributed data processing
spark = SparkSession.builder.appName("DataPrep").getOrCreate()
df = spark.read.csv("s3://bucket/data.csv")
```

### Processing Architecture

```
┌─────────────────────────────────────────────────────┐
│ DATA PROCESSING FLOW                                │
└─────────────────────────────────────────────────────┘

Step 1: Input Data
┌──────────────┐
│ S3 Bucket    │
│ Raw Data     │
└──────┬───────┘
       │
       ▼
Step 2: Processing Container
┌──────────────────────────────┐
│ Processing Container (ECR)   │
│ - Built-in OR custom code    │
│ - Runs on EC2 instances      │
│ - Transforms data            │
└──────┬───────────────────────┘
       │
       ▼
Step 3: Output Data
┌──────────────┐
│ S3 Bucket    │
│ Processed    │
│ Training Data│
└──────────────┘
```

**Key Points:**
- Input: Raw data from S3
- Processing: Container with your code (or built-in)
- Output: Processed data to S3
- Container-based: Everything runs in Docker containers

---

## 2. MODEL TRAINING

### Training Job Components

```
TRAINING JOB INPUTS:
├── Training Data (S3 URL)
│   └── s3://bucket/processed-data/
│
├── Compute Resources
│   ├── Instance type (ml.p3.8xlarge for GPU)
│   ├── Instance count (1 or more)
│   └── Volume size (storage for training)
│
├── Training Code (ECR path)
│   └── Container with your algorithm
│
├── Hyperparameters
│   ├── Learning rate
│   ├── Batch size
│   └── Epochs
│
└── Output Location (S3)
    └── s3://bucket/model-artifacts/
```

### Training Architecture

```
┌─────────────────────────────────────────────────────┐
│ TRAINING FLOW                                       │
└─────────────────────────────────────────────────────┘

Input: Processed Data
┌──────────────────┐
│ S3 Bucket        │
│ Training Data    │
└────────┬─────────┘
         │
         ▼
Training Container
┌─────────────────────────────────┐
│ Training Container (ECR)        │
│ - Your algorithm code           │
│ - Runs on training instances    │
│ - ml.p3.8xlarge (GPU)           │
│ - Can use multiple instances    │
└────────┬────────────────────────┘
         │
         ▼
Output: Trained Model
┌──────────────────┐
│ S3 Bucket        │
│ Model Artifacts  │
│ (model.tar.gz)   │
└──────────────────┘
```

### Training Algorithm Options

**1. Built-in Algorithms (AWS Provides)**
```
Supervised Learning:
├── XGBoost (classification, regression)
├── Linear Learner (linear models)
├── Factorization Machines (recommendations)
└── K-Nearest Neighbors (KNN)

Unsupervised Learning:
├── K-Means (clustering)
├── PCA (dimensionality reduction)
└── Random Cut Forest (anomaly detection)

Computer Vision:
├── Image Classification
├── Object Detection
└── Semantic Segmentation

NLP:
├── BlazingText (text classification, word2vec)
├── Seq2Seq (translation, summarization)
└── Neural Topic Model

Time Series:
└── DeepAR (forecasting)
```

**2. Framework-Based Training**
```python
# TensorFlow
from sagemaker.tensorflow import TensorFlow
estimator = TensorFlow(entry_point='train.py', ...)

# PyTorch
from sagemaker.pytorch import PyTorch
estimator = PyTorch(entry_point='train.py', ...)

# Scikit-learn
from sagemaker.sklearn import SKLearn
estimator = SKLearn(entry_point='train.py', ...)

# MXNet
from sagemaker.mxnet import MXNet
estimator = MXNet(entry_point='train.py', ...)
```

**3. Spark MLlib**
```python
# Use Spark for distributed ML
from sagemaker.spark import SparkML
estimator = SparkML(...)
```

**4. Hugging Face (LLMs and Transformers)**
```python
# Pre-trained models and fine-tuning
from sagemaker.huggingface import HuggingFace
estimator = HuggingFace(
    entry_point='train.py',
    transformers_version='4.26',
    pytorch_version='1.13',
    ...
)
```

**5. Reinforcement Learning**
```python
from sagemaker.rl import RLEstimator
estimator = RLEstimator(...)
```

**6. Custom Docker Image**
```python
# Bring your own container
estimator = Estimator(
    image_uri='123456789.dkr.ecr.us-east-1.amazonaws.com/my-algorithm:latest',
    ...
)
```

**7. AWS Marketplace Algorithms**
```
Purchase pre-built algorithms:
- Commercial ML algorithms
- Industry-specific models
- Pay-per-use or subscription
```

### Training Cost Considerations

```
Training is EXPENSIVE!
├── GPU instances: $12-30/hour
├── Multiple instances: Cost multiplies
└── Training time: Hours to days

Cost optimization:
✅ Use Spot Instances (up to 90% savings)
✅ Choose right instance type
✅ Optimize training time
✅ Use distributed training efficiently
```

---

## 3. MODEL DEPLOYMENT

### Deployment Options

```
┌─────────────────────────────────────────────────────┐
│ DEPLOYMENT OPTIONS                                  │
└─────────────────────────────────────────────────────┘

Option 1: Real-time Endpoints
├── Persistent endpoint (always running)
├── Low latency predictions
├── Auto-scaling support
└── Use case: Web apps, APIs

Option 2: Batch Transform
├── Process entire datasets at once
├── No persistent endpoint
├── Lower cost (pay only during processing)
└── Use case: Nightly predictions, bulk processing

Option 3: Serverless Inference
├── Auto-scales to zero
├── Pay per request
├── No idle costs
└── Use case: Intermittent traffic

Option 4: Edge Deployment (SageMaker Neo)
├── Deploy to edge devices
├── IoT devices, mobile phones
├── No internet required
└── Use case: Offline predictions, low latency
```

### Real-time Endpoint Architecture

```
┌─────────────────────────────────────────────────────┐
│ REAL-TIME ENDPOINT DEPLOYMENT                       │
└─────────────────────────────────────────────────────┘

Model Artifacts
┌──────────────────┐
│ S3 Bucket        │
│ model.tar.gz     │
└────────┬─────────┘
         │
         ▼
Inference Container
┌─────────────────────────────────┐
│ Inference Container (ECR)       │
│ - Loads model from S3           │
│ - Runs on ml.m5.large           │
│ - Handles prediction requests   │
└────────┬────────────────────────┘
         │
         ▼
Endpoint
┌─────────────────────────────────┐
│ HTTPS Endpoint                  │
│ - Load balanced                 │
│ - Auto-scaling                  │
│ - Always available              │
└────────┬────────────────────────┘
         │
         ▼
Your Application
┌─────────────────────────────────┐
│ Web App / Mobile App / API      │
│ Makes prediction requests       │
└─────────────────────────────────┘
```

### Batch Transform Architecture

```
┌─────────────────────────────────────────────────────┐
│ BATCH TRANSFORM                                     │
└─────────────────────────────────────────────────────┘

Input Data                Model
┌──────────────┐         ┌──────────────┐
│ S3 Bucket    │         │ S3 Bucket    │
│ Input Data   │         │ model.tar.gz │
└──────┬───────┘         └──────┬───────┘
       │                        │
       └────────┬───────────────┘
                ▼
        Batch Transform Job
        ┌─────────────────────────┐
        │ - Spins up instances    │
        │ - Loads model           │
        │ - Processes all data    │
        │ - Shuts down            │
        └────────┬────────────────┘
                 │
                 ▼
        Output Predictions
        ┌──────────────┐
        │ S3 Bucket    │
        │ Predictions  │
        └──────────────┘
```

**When to use Batch Transform:**
- Don't need real-time predictions
- Process large datasets periodically
- Cost-sensitive (no always-running endpoint)
- Nightly/weekly prediction jobs

### Advanced Deployment Features

**1. Inference Pipelines**
```
Chain multiple containers:
Preprocessing → Model 1 → Model 2 → Postprocessing

Example:
Input → Feature Engineering → Prediction → Format Output
```

**2. SageMaker Neo (Edge Deployment)**
```
Deploy to edge devices:
├── IoT devices
├── Mobile phones
├── Embedded systems
└── Raspberry Pi

Benefits:
✅ No internet required
✅ Low latency
✅ Privacy (data stays on device)
```

**3. Elastic Inference**
```
Accelerate inference:
- Attach GPU acceleration to CPU instances
- Lower cost than full GPU instances
- Faster predictions
```

**4. Automatic Scaling**
```python
# Auto-scale based on traffic
client.register_scalable_target(
    ServiceNamespace='sagemaker',
    ResourceId='endpoint/my-endpoint/variant/AllTraffic',
    ScalableDimension='sagemaker:variant:DesiredInstanceCount',
    MinCapacity=1,
    MaxCapacity=10
)

# Scale up when traffic increases
# Scale down when traffic decreases
```

**5. Shadow Testing**
```
Test new models safely:
├── Production model serves real traffic
├── New model processes same requests (shadow)
├── Compare results
└── Deploy new model only if better

Benefits:
✅ No risk to production
✅ Real-world testing
✅ Easy rollback
```

**6. Multi-Model Endpoints**
```
Host multiple models on one endpoint:
- Cost savings (share infrastructure)
- Dynamically load models
- Good for many small models
```

---

## Complete Workflow Example

```
┌─────────────────────────────────────────────────────┐
│ END-TO-END SAGEMAKER WORKFLOW                       │
└─────────────────────────────────────────────────────┘

1. DATA PREPARATION
   ┌──────────────┐
   │ Raw Data     │ s3://bucket/raw/
   └──────┬───────┘
          │
          ▼
   ┌──────────────────────┐
   │ Processing Container │ (Pandas, Spark)
   └──────┬───────────────┘
          │
          ▼
   ┌──────────────┐
   │ Clean Data   │ s3://bucket/processed/
   └──────────────┘

2. MODEL TRAINING
   ┌──────────────┐
   │ Clean Data   │ s3://bucket/processed/
   └──────┬───────┘
          │
          ▼
   ┌──────────────────────┐
   │ Training Container   │ (XGBoost, TensorFlow)
   │ ml.p3.8xlarge × 4    │
   └──────┬───────────────┘
          │
          ▼
   ┌──────────────┐
   │ Trained Model│ s3://bucket/model/
   └──────────────┘

3. MODEL DEPLOYMENT
   ┌──────────────┐
   │ Trained Model│ s3://bucket/model/
   └──────┬───────┘
          │
          ▼
   ┌──────────────────────┐
   │ Inference Container  │
   │ ml.m5.large × 2      │
   └──────┬───────────────┘
          │
          ▼
   ┌──────────────┐
   │ HTTPS        │ https://endpoint.aws/
   │ Endpoint     │
   └──────────────┘
```

---

## For the Exam - Key Points

### Data Preparation
- **Primary source**: S3 (most common)
- **High performance**: FSx for Lustre
- **Other sources**: Athena, EMR, Redshift, Keyspaces
- **Format matters**: Each algorithm has preferred format
- **Container-based**: Processing runs in containers

### Training
- **Input**: S3 URL to training data
- **Compute**: Choose instance type and count
- **Code**: ECR container path
- **Output**: Model artifacts to S3
- **Options**: Built-in algorithms, frameworks, custom code
- **Cost**: Training is expensive (use Spot Instances)

### Deployment
- **Real-time**: Persistent endpoint, low latency
- **Batch**: Process datasets, no persistent endpoint
- **Serverless**: Auto-scales, pay per request
- **Edge**: Deploy to devices (SageMaker Neo)
- **Features**: Auto-scaling, shadow testing, multi-model

### Common Scenarios
- **Scenario**: Need real-time predictions → Use real-time endpoint
- **Scenario**: Nightly batch predictions → Use batch transform
- **Scenario**: Deploy to IoT device → Use SageMaker Neo
- **Scenario**: Test new model safely → Use shadow testing
- **Scenario**: High-performance data processing → Use FSx for Lustre
- **Scenario**: Cost optimization → Use Spot Instances for training

---

## Quick Reference

| Stage | Input | Processing | Output |
|-------|-------|------------|--------|
| Data Prep | S3 raw data | Processing container | S3 processed data |
| Training | S3 processed data | Training container | S3 model artifacts |
| Deployment | S3 model | Inference container | Endpoint or predictions |

**Remember:**
1. **Everything flows through S3** (data, models)
2. **Everything runs in containers** (ECR)
3. **Everything is temporary** except endpoints (EC2 spins up/down)
4. **Choose the right tool** for each stage
5. **Cost matters** (Spot Instances, batch vs real-time)
