# SageMaker: Docker Containers & Production Variants

> Domain: ML Implementation & Operations

---

## SageMaker + Docker — The Core Idea

**Everything in SageMaker runs inside a Docker container registered with Amazon ECR.**

What can live in these containers:
- Pre-built deep learning models
- Pre-built scikit-learn or Spark ML models
- Your own code built on supported frameworks (TensorFlow, MXNet, PyTorch, Chainer)
- Completely custom training/inference code in any language
- Extended versions of SageMaker's pre-built algorithm images

**Key point:** SageMaker doesn't care what's inside the container. Any script, any language, any runtime — as long as the Docker container conforms to the expected directory structure.

---

## Distributing TensorFlow

TensorFlow does **NOT** automatically distribute across multiple machines.

To distribute TensorFlow training across a fleet:
- **Horovod** — framework for distributed training
- **Parameter Servers** — another distribution approach

> 🎯 Exam hint: Remember "Horovod" for distributing TensorFlow across multiple machines/GPUs.

---

## Docker Container Structure

### Training Container

```
/opt/ml/
├── input/
│   ├── config/
│   │   ├── hyperparameters.json
│   │   └── resourceconfig.json
│   └── data/
│       └── <channel_name>/     ← training data channels
├── code/
│   └── train.py               ← YOUR TRAINING SCRIPT GOES HERE
├── model/
│   └── (model artifacts for deployment)
└── output/
    └── failure                 ← error messages go here
```

**Remember:** Training code lives at `/opt/ml/code/`

### Inference/Deployment Container

```
/opt/ml/
└── model/
    └── (model files for serving)
```

---

## Full Docker Image Structure

```
/working-directory/
├── nginx.conf          ← Config for the Nginx front-end web server
├── predictor.py        ← Flask web server that makes predictions (customize this)
├── serve/              ← Launches Gunicorn server (runs multiple Flask instances)
├── train/              ← Invoked when container runs for training
└── wsgi.py             ← Small wrapper to invoke Flask app for serving
```

| File | Purpose |
|------|---------|
| `nginx.conf` | Configures the Nginx web server for deployment |
| `predictor.py` | Flask app that handles prediction requests — **you customize this** |
| `serve/` | Starts Gunicorn, which runs multiple instances of predictor.py |
| `train/` | Your training algorithm — modify this for custom training |
| `wsgi.py` | Wrapper that invokes Flask for serving results |

You can have **separate** training and inference images, or **combine** them. Either works.

---

## Dockerfile Example

```dockerfile
FROM tensorflow/tensorflow          # Base image with TensorFlow

RUN pip install sagemaker-containers  # SageMaker compatibility layer

COPY train.py /opt/ml/code/train.py  # Copy training script to expected location

ENV SAGEMAKER_PROGRAM train.py       # Define the entrypoint script
```

**The only required environment variable:** `SAGEMAKER_PROGRAM` — tells SageMaker which script to run.

### Other Useful Environment Variables

| Variable | Purpose |
|----------|---------|
| `SAGEMAKER_PROGRAM` | Entrypoint script name (REQUIRED) |
| `SAGEMAKER_TRAINING_MODULE` | Where to load TensorFlow/MXNet/etc modules |
| `SAGEMAKER_SERVICE_MODULE` | Module for serving |
| `SM_MODEL_DIR` | Where model checkpoints are saved → pushed to S3 |
| `SM_CHANNEL_TRAIN` | Where training data channel comes from |
| `SM_CHANNEL_TEST` | Where test data channel comes from |
| `SM_CHANNEL_VALIDATION` | Where validation data channel comes from |
| `SM_HPS` | Hyperparameters (used by automatic model tuning) |

---

## Using Your Own Image — Code Example

```python
# Build the Docker image (can be done from a notebook)
!cd docker && docker build -t foo .

# Use it in SageMaker
from sagemaker.estimator import Estimator

estimator = Estimator(image_name='foo', ...)
estimator.fit()  # Runs training
```

That's it. Build image → reference by name → call fit.

---

## The SageMaker Training Flow (Review)

```
ECR (Docker Images)
    │
    ├── Training Image ──→ Training Job ──→ Model Artifacts → S3
    │                           ↑
    │                      S3 (training data)
    │
    └── Inference Image ──→ Model Deployment ──→ Endpoint (fleet of servers)
                                ↑
                           S3 (model artifacts)
```

---

---

## Production Variants

**What it is:** Run multiple versions of your model on live traffic simultaneously, with controlled traffic distribution.

**Why:** Not all models can be evaluated offline effectively. Recommender systems, for example — past behavior accuracy ≠ future performance.

### How It Works

- Each model version = a **production variant**
- You assign **variant weights** to control traffic split
- Example: 90% to old model, 10% to new model
- Gradually increase new model's weight as confidence grows
- If new model fails → change weights back instantly (rollback)

### Typical Flow

```
Day 1:  Old model = 90%,  New model = 10%
Day 3:  Old model = 70%,  New model = 30%
Day 7:  Old model = 30%,  New model = 70%
Day 10: Old model = 0%,   New model = 100%  → discard old variant
```

### Use Cases
- A/B testing models in production
- Validating performance on real-world traffic
- Safe rollout when offline metrics aren't sufficient
- Quick rollback if unforeseen problems appear

---

## Production Variants vs Deployment Guardrails vs Shadow Tests

| Feature | Production Variants | Deployment Guardrails | Shadow Tests |
|---------|--------------------|-----------------------|--------------|
| Traffic to new model | Real (users see responses) | Real (users see responses) | Silent (users don't see) |
| Traffic control | Manual weight adjustment | Automated (canary/linear/all-at-once) | % allocation |
| Rollback | Manual (change weights) | Automatic | Manual |
| Purpose | A/B test on live traffic | Safe deployment rollout | Pre-production evaluation |
| User impact | Yes — some users get new model | Yes — controlled shift | No — shadow is invisible |

---

## Exam Tips

- **Docker + ECR** = how all SageMaker models are packaged and deployed
- Training code goes in `/opt/ml/code/` — remember this path
- `SAGEMAKER_PROGRAM` = only required env variable for custom containers
- **Horovod** = distribute TensorFlow across multiple machines
- **Production Variants** = A/B testing with variant weights on live traffic
- Production Variants give responses to real users (unlike Shadow Tests)
- You can combine training + inference in one image OR separate them
- SageMaker is framework-agnostic — any language, any runtime, as long as it's in Docker
