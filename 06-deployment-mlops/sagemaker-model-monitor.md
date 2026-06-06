# SageMaker Model Monitor

## What is it?
Monitors deployed ML models for **ML-specific issues** (not infrastructure). It's a detective that watches your AI's "brain", not the server it runs on.

## Key Problem: Models Get Old with New Data
ML models degrade because:
- **Training**: Model learns from historical data (fixed snapshot)
- **Production**: Real-world data keeps changing over time
- **Result**: Model becomes outdated, like studying for 2019 test but taking it in 2023

**Why Models Degrade:**
- **Data Drift**: New data has different patterns than training data
  - Example: COVID changed shopping behavior, but model trained on pre-COVID data
- **Feature Changes**: Same features, different value ranges due to world changes
- **Missing Features**: Production pipeline breaks, features become null
- **Quality Issues**: New data sources introduce errors/noise

## IMPORTANT: What Model Monitor Does vs Doesn't Do
✅ **Does**: Detects problems and alerts you
❌ **Doesn't**: Fix problems (you must retrain/fix data)

## Model Monitor vs Infrastructure Monitoring
- **SageMaker Model Monitor**: "Is my AI still smart?" (ML performance)
- **CloudWatch**: "Is my server running?" (CPU, memory, uptime)
- **Both can be used together** on SageMaker endpoints

## Core Features

### 1. Data Drift Detection
- Monitors how input data changes over time
- Compares against baseline statistical properties
- Example: Credit scoring model sees different customer demographics

### 2. Model Quality Monitoring
- Tracks model accuracy over time
- Uses ground truth labels from human reviewers
- Compares model predictions vs actual correct answers

### 3. Bias Detection (via SageMaker Clarify)
- Detects unfair treatment across groups (age, income, etc.)
- Explains which features contribute to bias
- Uses metrics like Kullback-Leibler divergence

### 4. Feature Attribution Drift
- Monitors how feature importance changes
- Detects when different features start driving predictions

## How Model Monitor Works

### 1. Baseline Comparison
- Takes snapshot of training data statistics (baseline)
- Compares new incoming data against baseline
- Example: Training data average age = 35, now it's 50 → Alert!

### 2. Statistical Analysis
- Calculates metrics: mean, median, standard deviation, distributions
- Tracks missing value percentages
- If these change significantly → Alert!

### 3. Ground Truth Comparison (for model accuracy)
- Compares model predictions vs actual correct answers
- Example: Model says "approved" but human says "rejected"
- If accuracy drops below threshold → Alert!

### 4. Scheduled Monitoring Jobs
- Runs automatically (hourly, daily, weekly)
- Processes batches of new data
- Generates monitoring reports

### Setup & Alerting
- No code required - visual setup in SageMaker Studio
- Data stored securely in S3
- Metrics sent to CloudWatch (you configure alarms)
- Think of it: Monitor = security camera, CloudWatch = alarm system, You = person who fixes

### Data Capture

Logs everything going in and out of your inference endpoints.

```
Request (input) → Endpoint → Response (output)
       ↓                            ↓
    Captured                     Captured
       ↓                            ↓
         → Stored in S3 as JSON ←
```

What it's used for:
- **Further training**: feed real-world data back into model (continuous learning)
- **Debugging**: see exactly what went in and what came out
- **Monitoring**: compare captured data against baseline over time

Key facts:
- Works with both real-time AND batch inference
- Stored as JSON in S3
- Can be encrypted (security)
- Supported via Boto3 and SageMaker Python SDK
- Part of the feedback loop for continuous model improvement

### Visualization
- Built-in SageMaker Studio dashboards
- Integrates with TensorBoard, QuickSight, Tableau

## Monitoring Types
1. **Data Quality Drift** - Statistical properties of input features
2. **Model Quality Drift** - Prediction accuracy vs ground truth
3. **Bias Drift** - Fairness across different groups
4. **Feature Attribution Drift** - Changes in feature importance

## Exam Key Points
- Automatically detects anomalies and outliers
- Detects new features appearing in data that weren't in training
- Integrates with SageMaker Clarify for bias detection
- Uses CloudWatch for alerts (you must configure alarms)
- Supports multiple bias metrics
- Ground truth integration for accuracy monitoring
- Visual setup, no coding required
- Data stored in S3 (secured with standard S3 security)
- Requires a **monitoring schedule** to run jobs periodically
- Feature attribution drift uses **NDCG** (Normalized Discounted Cumulative Gain) metric
  - Compares feature ranking of training data vs live data
- When alert fires → corrective actions: retrain model or audit data
- Integrates with: TensorBoard, QuickSight, Tableau, SageMaker Studio

### Baselines:
- **Data quality baseline**: statistical properties (mean, std dev, min, max) of training features
- **Model quality baseline**: accuracy, RMSE, precision, recall of the model at deployment time
- Drift is measured RELATIVE to these baselines