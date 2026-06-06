# 🗺️ ML Pipeline Map — Where Everything You Learned Fits

Use this as your mental map. When you learn something new, ask yourself:
"Which phase does this belong to?" — then it clicks.

```
┌─────────────────────────────────────────────────────────────────────┐
│                    THE COMPLETE ML PIPELINE                         │
│                                                                     │
│   Data → Prepare → Choose Algorithm → Train → Evaluate → Deploy    │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## PHASE 1: UNDERSTAND & COLLECT DATA

```
"What kind of data do I have and where does it live?"
```

| You Learned | What It Does | Real Use |
|---|---|---|
| Three Vs of Data | Volume, Velocity, Variety — helps you pick the right tools | "We have 10TB of streaming data" → you know you need Kinesis, not a CSV upload |
| Structured & Unstructured Data | Know the difference (tables vs images/text/audio) | Structured → XGBoost works great. Unstructured → need CNN, RNN, etc. |
| Data Sources & Formats | CSV, JSON, Parquet, RecordIO, etc. | Each algorithm accepts specific formats — you MUST know which |
| Data Warehouse vs Data Lake | Warehouse = structured/cleaned, Lake = raw/everything | Decides where your data lives before ML |
| Data Mesh | Decentralized data ownership | Architecture pattern for large orgs |

### AWS Services for this phase:

| You Learned | What It Does | When You Use It |
|---|---|---|
| Amazon Athena | Query data in S3 using SQL | "Let me peek at my data before training" |
| AWS Kinesis (comparison) | Real-time streaming data ingestion | Live data flowing in (clickstreams, IoT, logs) |
| EMR / Hadoop / Spark | Process massive datasets | Data too big for a single machine |

```
REAL EXAMPLE:
  You have 500GB of customer transaction logs in S3
  → Athena to explore: "SELECT * FROM transactions LIMIT 100"
  → EMR/Spark to process all 500GB into clean features
  → Output goes to S3, ready for Phase 2
```

---

## PHASE 2: PREPARE & TRANSFORM DATA

```
"Clean it, fix it, engineer features, make it ML-ready"
```

| You Learned | What It Does | Real Use |
|---|---|---|
| ETL / ELT Deep Dive | Extract, Transform, Load pipelines | Raw data → clean data ready for ML |
| Feature Engineering | Create useful input features from raw data | Turn "birthdate" into "age", "timestamp" into "day_of_week" |
| Feature Engineering Techniques | Specific methods (scaling, encoding, binning) | One-hot encode categories, normalize numbers |
| TF-IDF Data Preprocessing | Turn text into numbers | "This movie is great" → [0.0, 0.3, 0.0, 0.7] |
| Missing Data Imputation | Handle missing values | Fill blanks with mean/median or drop rows |
| Outliers Handling | Deal with extreme values | Remove or cap that one $999,999 transaction |
| Unbalanced Data Handling | Fix when one class dominates | 99% "not fraud" vs 1% "fraud" — model just says "not fraud" always |

### AWS Services for this phase:

| You Learned | What It Does | When You Use It |
|---|---|---|
| AWS Glue | Serverless ETL service | Automated data cleaning pipelines |
| AWS Glue DataBrew | Visual data preparation (no code) | Non-engineers cleaning data |
| AWS Glue Data Quality | Validate data quality rules | "Make sure no column has >5% nulls" |
| SageMaker Data Wrangler | Visual feature engineering in SageMaker | Transform, analyze, and prepare data for ML |
| SageMaker Data Wrangler Lab | Hands-on practice | You did the lab! |
| SageMaker Feature Store | Save & reuse engineered features | Team shares features across projects |
| EMR + TF-IDF Lab | Process text at scale | You ran TF-IDF on EMR |

```
REAL EXAMPLE:
  Raw customer data has:
  → Missing ages → Impute with median
  → "City" column → One-hot encode it
  → Salary outlier of $10M → Cap at $500K
  → 99% non-fraud → Use SMOTE or scale_pos_weight
  → Save features to Feature Store for reuse
```

---

## PHASE 3: CHOOSE THE RIGHT ALGORITHM

```
"What problem am I solving and which algorithm fits?"
```

### Classification (Is it A or B?)

| You Learned | What It Does | Use When |
|---|---|---|
| XGBoost | Boosted decision trees | Tabular data, Kaggle-style problems, fraud detection |
| Linear Learner | Linear/logistic regression | Simple classification or regression |
| KNN | Find K nearest neighbors | Recommendations, simple classification |
| Factorization Machines | Sparse data classification | Recommendation systems, click prediction |
| IP Insights | Detect unusual IP behavior | Security — "is this login suspicious?" |

### Computer Vision (Images)

| You Learned | What It Does | Use When |
|---|---|---|
| CNN (theory) | How convolutional neural networks work | Foundation knowledge for all image tasks |
| Image Classification | Label entire image | "Is this a cat or dog?" |
| Object Detection | Find AND locate objects in image | "Where are the cars in this photo?" |
| Semantic Segmentation | Label every pixel | Self-driving cars, medical imaging |

### Text & Language (NLP)

| You Learned | What It Does | Use When |
|---|---|---|
| RNN (theory) | How recurrent neural networks work | Foundation for all sequence/text tasks |
| BlazingText | Word embeddings + text classification | Fast text classification, Word2Vec |
| Seq2Seq | Sequence to sequence translation | Language translation, text summarization |
| LDA | Topic modeling | "What topics are in these 10,000 documents?" |
| Neural Topic Model | Topic modeling (neural network version) | Same as LDA but uses neural nets |
| Amazon Comprehend | Text analysis service (managed) | Sentiment, entities, language detection — no ML needed |
| Amazon Transcribe | Speech to text | Convert audio to text |

### Clustering & Dimensionality

| You Learned | What It Does | Use When |
|---|---|---|
| K-Means | Group similar data points | Customer segmentation |
| PCA | Reduce number of features | Too many features, speed up training |

### Time Series & Anomaly

| You Learned | What It Does | Use When |
|---|---|---|
| DeepAR | Forecast future values | Predict sales, demand, stock prices |
| Random Cut Forest | Detect anomalies | "Something weird happened in the data" |

### Other

| You Learned | What It Does | Use When |
|---|---|---|
| Object2Vec | Learn relationships between pairs | "Are these two sentences similar?" |
| LightGBM | Gradient boosting (like XGBoost) | Alternative to XGBoost, sometimes faster |
| SageMaker Canvas | No-code ML | Business users building models without code |
| Amazon Q Business | AI assistant for business | Q&A over company documents |
| Amazon Augmented AI (A2I) | Human review of ML predictions | When you need humans to verify ML output |

```
REAL EXAMPLE:
  Problem: "Predict if a customer will churn"
  → Tabular data with features → XGBoost or Linear Learner
  
  Problem: "Detect defective products on assembly line"
  → Images → Image Classification or Object Detection
  
  Problem: "Translate English to Spanish"
  → Text sequences → Seq2Seq
```

---

## PHASE 4: TRAIN & TUNE THE MODEL

```
"Make it learn WELL — not just learn"
```

| You Learned | What It Does | Real Use |
|---|---|---|
| Activation Functions | Controls neuron output (ReLU, sigmoid, etc.) | Choosing wrong one = model won't learn |
| Tuning Neural Networks | Hyperparameter tuning (learning rate, batch size, epochs) | Finding the sweet spot for best performance |
| Regularization Techniques | Prevent overfitting (dropout, early stopping) | Model memorizing data? → Apply these |
| SageMaker Input Modes | File mode vs Pipe mode for feeding data | Large dataset? → Pipe mode streams it |
| EC2 for AI | Which instances for training | GPU (P3, G5) for deep learning, M5 for XGBoost |

```
REAL EXAMPLE:
  You trained XGBoost for fraud detection:
  → Training accuracy: 99%, Test accuracy: 78%
  → OVERFITTING!
  → Reduce max_depth, increase subsample
  → Add dropout if using neural network
  → Enable early stopping
  → Now: Training 94%, Test 92% ← much better!
  
  Instance choice:
  → XGBoost → M5 (memory heavy)
  → CNN for images → P3 or G5 (GPU needed)
```

---

## PHASE 5: EVALUATE THE MODEL

```
"Is my model actually good? Is it fair? Can I explain it?"
```

| You Learned | What It Does | Real Use |
|---|---|---|
| SageMaker Clarify | Explain predictions + detect bias | "WHY did the model deny this loan?" |
| SageMaker Ground Truth | Create labeled training data | Need humans to label images/text for training |

```
REAL EXAMPLE:
  Loan approval model deployed:
  → Clarify shows: "Model relies 60% on zip code"
  → That's a proxy for race → BIAS detected
  → Fix: remove zip code, retrain
  → Clarify now shows fair distribution ✅
```

---

## PHASE 6: DEPLOY & SERVE

```
"Put it in production, serve predictions"
```

| You Learned | What It Does | Real Use |
|---|---|---|
| SageMaker Deployment & Scaling | Endpoints, auto-scaling, A/B testing | Real-time predictions at scale |
| SageMaker Overview | The full SageMaker platform | Ties everything together |
| SageMaker Domain | Multi-user SageMaker setup | Team collaboration on ML projects |
| SageMaker Workflow | ML pipeline orchestration | Automate the entire pipeline end-to-end |

```
REAL EXAMPLE:
  Fraud model is ready:
  → Deploy to SageMaker real-time endpoint
  → Auto-scale: 2 instances normally, 10 during Black Friday
  → A/B test: 90% traffic to v1, 10% to v2 (new model)
  → v2 performs better → shift 100% traffic
```

---

## PHASE 7: MONITOR & MAINTAIN

```
"Is it still working well over time?"
```

| You Learned | What It Does | Real Use |
|---|---|---|
| SageMaker Model Monitor | Detect data drift & quality issues | Model accuracy dropping? Data changed? |

```
REAL EXAMPLE:
  6 months after deployment:
  → Model Monitor alerts: "Input data distribution shifted"
  → Fraud patterns changed (new attack types)
  → Retrain with fresh data → redeploy
  → Back to high accuracy ✅
```

---

## 🧭 THE FULL PICTURE — ONE GLANCE

```
┌──────────────────────────────────────────────────────────────────────────┐
│                                                                          │
│  PHASE 1: COLLECT          Three Vs, Structured/Unstructured,           │
│  "Get the data"            Athena, Kinesis, EMR, Data Lake/Warehouse    │
│       │                                                                  │
│       ▼                                                                  │
│  PHASE 2: PREPARE          Glue, DataBrew, Data Wrangler,              │
│  "Clean & transform"       Feature Engineering, Imputation,             │
│       │                    Outliers, TF-IDF, Feature Store              │
│       ▼                                                                  │
│  PHASE 3: ALGORITHM        XGBoost, Linear Learner, CNN, RNN,          │
│  "Pick the right tool"     BlazingText, DeepAR, K-Means, PCA...        │
│       │                                                                  │
│       ▼                                                                  │
│  PHASE 4: TRAIN & TUNE     Activation Functions, Regularization,        │
│  "Make it learn well"      Hyperparameter Tuning, Input Modes, EC2     │
│       │                                                                  │
│       ▼                                                                  │
│  PHASE 5: EVALUATE         Clarify (bias + explainability),            │
│  "Is it good & fair?"      Ground Truth (labeling)                      │
│       │                                                                  │
│       ▼                                                                  │
│  PHASE 6: DEPLOY           Endpoints, Auto-scaling, A/B testing,       │
│  "Put it in production"    SageMaker Workflow                           │
│       │                                                                  │
│       ▼                                                                  │
│  PHASE 7: MONITOR          Model Monitor (data drift, quality)          │
│  "Keep it healthy"         → loops back to Phase 1 if retrain needed   │
│       │                                                                  │
│       └──────────── 🔄 back to Phase 1 when needed ────────────────────│
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

---

**How to use this map:** Every time you learn something new, ask yourself:
1. Which phase does this belong to?
2. What problem does it solve in that phase?
3. What AWS service runs it?

If you can answer those three, you truly understand it — not just memorized it. 🎯
