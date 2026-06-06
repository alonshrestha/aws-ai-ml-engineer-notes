# SageMaker Data Wrangler

## What It Actually Is

**Data Wrangler = Visual ETL Code Generator for Machine Learning**

**Simple explanation:** You design transformations visually → Data Wrangler generates Python code → You run that code in your ML pipeline

**Key point:** Data Wrangler is a CODE GENERATOR ONLY, not a runtime service

---

## Data Wrangler vs Canvas (Important Distinction)

### Data Wrangler (Standalone)
```
What it does:
1. Visual interface for designing transformations
2. Generates Python/PySpark code
3. Exports code to Jupyter notebooks
4. YOU run the code separately
5. Data Wrangler is done - no runtime involvement
```

### Canvas (What was used in the lab)
```
What it does:
1. Visual interface for designing transformations
2. Generates code behind the scenes (Data Wrangler-style)
3. RUNS the code immediately
4. Continues to build models and make predictions
5. Complete no-code ML platform
```

**Lab Reality:** The instructor said "Data Wrangler" but actually used Canvas end-to-end

---

## Real Lab Example: Employee Absenteeism Prediction

### The Business Problem
**Goal:** Predict how many hours employees will be absent based on their characteristics

**Dataset:** Employee data (699 rows) with features:
- Distance from work, age, workload average
- Transportation costs, education level
- Lifestyle factors (drinking, smoking, pets)
- **Target:** Absenteeism time in hours

### Code Generation in Action (Data Wrangler-Style)

#### Natural Language Prompts Generated Python Code:

**Transformation 1: Fix Data Type**
```
User prompt: "Transform the workload average per day feature from a string 
to a long type, accounting for the use of commas in the numbers"

Canvas generated Python code (behind the scenes):
df['workload_average'] = df['workload_average'].str.replace(',', '').astype('int64')

Canvas executed this code immediately
```

**Transformation 2: Remove Duplicates**
```
User prompt: "Remove duplicate rows"

Canvas generated Python code:
df = df.drop_duplicates()

Canvas applied transformation instantly
```

### What Pure Data Wrangler Would Export:
```python
# Data Wrangler would give you this code to run separately:
import pandas as pd

def transform_data():
    # Load training data
    df = pd.read_csv('s3://bucket/absentee-train.csv')
    
    # Apply transformations (from your visual design)
    df['workload_average'] = df['workload_average'].str.replace(',', '').astype('int64')
    df = df.drop_duplicates()
    
    # Save processed data
    df.to_csv('s3://bucket/processed-absentee-train.csv')
    return df

# YOU would run this code
# YOU would build the model separately
# YOU would make predictions separately
```

---

## Why 2 Files in ML? (Critical Concept from Lab)

### The Core ML Principle: Learning vs Testing

**Training file = Textbook with answers**
**Prediction file = Test without answers**

### File 1: Training Data (absentee-train.csv)
**Purpose:** Teach the model patterns
```
Employee_ID | Age | Workload | Transport_Cost | Absenteeism_Hours
1001       | 25  | 40       | 50            | 2  ← ANSWER
1002       | 45  | 60       | 100           | 8  ← ANSWER  
1003       | 30  | 35       | 25            | 1  ← ANSWER

Model learns: "High workload + high transport cost = more sick days"
```

### File 2: Prediction Data (absentee-predictor.csv)
**Purpose:** Test the model on NEW employees
```
Employee_ID | Age | Workload | Transport_Cost | Absenteeism_Hours
2001       | 28  | 55       | 75            | ???  ← PREDICT THIS
2002       | 35  | 30       | 20            | ???  ← PREDICT THIS
2003       | 50  | 65       | 120           | ???  ← PREDICT THIS

Model predicts: "Based on what I learned, Employee 2001 will be sick 5 hours"
```

**Why not use same file?** The model already knows the answers - it would be cheating!

---

## The Complete Process (Step by Step)

### Step 1: Import Data from Multiple Sources
**Supported sources:**
- S3 (CSV, JSON, Parquet files)
- Amazon Athena (query data lakes)
- Amazon Redshift (data warehouse)
- SageMaker Feature Store
- Lake Formation
- Any JDBC database
- SaaS applications (Salesforce, etc.)
- External systems (Databricks)

### Step 2: Visualize and Explore Data (LOOK at the data)
**Built-in data analysis:**
```
┌─────────────────────────────────────┐
│ Data Wrangler - Data Preview        │
├─────────────────────────────────────┤
│ Employee Dataset Preview:           │
│                                     │
│ Age | Workload | Transport | Absent │
│ 25  | 40,000   | 50       | 2      │
│ 45  | 60,000   | 100      | 8      │
│ 30  | ??       | 25       | 1      │
│                                     │
│ Issues Found:                       │
│ - Workload has commas (string!)     │
│ - Missing values in some rows       │
│ - 33% duplicate rows (suspicious)   │
└─────────────────────────────────────┘
```

**What you can check (just LOOKING, not changing):**
- Data distribution and outliers
- Missing values (how many?)
- Data types (are they correct?)
- Column statistics
- Data quality issues

**This is just ANALYSIS - you're not changing the data yet!**

### Step 3: Transform Data (CHANGE the data for ML)

**Why transform? Raw data is usually not ready for ML models:**

#### Problem 1: Data Type Issues
```
Original data:
Workload: ["40,000", "60,000", "35,000"]  (strings with commas)

ML models need clean numbers!

Transformation:
Workload: [40000, 60000, 35000]  (clean integers)
```

#### Problem 2: Duplicate Data
```
Original data:
33% duplicate rows (suspicious - likely fake data)

Transformation:
Remove duplicates → Clean, unique data only
```

**Common transformations (300+ available):**
- **Handle missing data:** Fill, drop, or impute
- **One-hot encoding:** Convert categories to binary features
- **Normalization:** Scale numerical values
- **Feature engineering:** Create new features from existing ones
- **Data cleaning:** Remove duplicates, fix formats

**Custom transformations:**
- Write your own using Pandas code
- Use PySpark for big data
- Use PySpark SQL for complex queries

### Step 4: Quick Model Testing (Unique Feature!)
**What it does:**
- Train a quick ML model using your transformed data
- Test different transformation approaches
- See which transformations improve model performance
- Optimize your data preparation before full training

**Lab example:**
```
Canvas found: Transportation expense and workload are biggest predictors
Business insight: Work stress and commute costs drive absenteeism
```

### Step 5: Export Generated Code
**Data Wrangler outputs:**
- **Jupyter Notebook:** Python code with all your transformations
- **Python script:** Standalone code for production
- **SageMaker Pipeline:** Integration with ML workflows

**Key point:** Data Wrangler creates the code, doesn't run in production

---

## Lab Business Insights Discovered

### Top Predictors of Absenteeism:
1. **Transportation expense** (biggest predictor)
   - People who pay more to get to work call in sick more
2. **Workload average per day** (second biggest)
   - People who work more hours call in sick more

### Minimal Impact Factors:
- Drinking/smoking habits: No correlation
- Weight, height, age: Minimal impact

**Actionable business insights:**
- Provide transportation subsidies to reduce absenteeism
- Monitor workload to prevent burnout
- Personal habits don't matter for prediction

---

## Where Data Wrangler Fits in ML Pipeline

### Pure Data Wrangler Workflow:
```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│ Raw Data    │───▶│ Data        │───▶│ Generated   │───▶│ You Build   │
│ (S3, RDS)   │    │ Wrangler    │    │ Code        │    │ ML Model    │
│             │    │ (Design)    │    │ (Export)    │    │ Separately  │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
```

### Canvas Workflow (What Actually Happened in Lab):
```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│ Raw Data    │───▶│ Canvas      │───▶│ Canvas      │───▶│ Canvas      │
│ (S3)        │    │ (Transform) │    │ (Build      │    │ (Make       │
│             │    │             │    │  Model)     │    │  Predictions│
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
```

**Integration points:**
- **Input:** Various data sources
- **Output:** SageMaker Processing jobs
- **Output:** SageMaker Pipelines
- **Output:** Feature Store (for reusable features)
- **Output:** Jupyter notebooks for experimentation

---

## Key Differences from AWS Glue

### Data Wrangler vs Glue DataBrew
| Feature | Data Wrangler | Glue DataBrew |
|---------|---------------|---------------|
| **Purpose** | ML-focused ETL | General ETL |
| **Interface** | SageMaker Studio | Standalone service |
| **Quick Model** | ✅ Test ML models | ❌ No ML testing |
| **Target** | Data scientists | Data engineers |
| **Integration** | Deep SageMaker integration | Broader AWS integration |

**When to use Data Wrangler:**
- Preparing data specifically for ML models
- Need to test transformations with quick models
- Working within SageMaker ecosystem
- Data scientists doing feature engineering

**When to use Glue DataBrew:**
- General data preparation (not ML-specific)
- Data engineering workflows
- Need to output to various destinations
- Working outside SageMaker ecosystem

---

## Common Issues and Troubleshooting

### 1. Permission Issues
**Problem:** "Access denied" errors
**Solution:** Ensure SageMaker Studio user has:
- `AmazonSageMakerFullAccess` policy
- Access to your data sources (S3, Redshift, etc.)
- Proper IAM roles for cross-service access

### 2. Resource Limits
**Error:** "Instance type not available"
**Real meaning:** You've hit your quota limit
**Solution:**
1. Go to AWS Service Quotas
2. Find Amazon SageMaker
3. Look for "Studio Kernel Gateway apps"
4. Request increase for ML m5.4xlarge instances
5. Wait for approval (usually quick)

### 3. Data Source Connection Issues
**Problem:** Can't connect to databases/services
**Solution:** Check:
- Network connectivity (VPC, security groups)
- Database credentials and permissions
- Service endpoints and ports
- Data source-specific IAM policies

---

## Cost Considerations

### What You Pay For
**Data Wrangler costs:**
- **Compute:** ML instances while using the interface (~$1-5/hour)
- **Storage:** S3 costs for input/output data
- **Data transfer:** Moving data between services

**Lab costs:** ~$15-35 for the demo (699 rows)

**Cost optimization:**
- Shut down Data Wrangler sessions when not in use
- Use smaller datasets for experimentation
- Generate code once, run many times (no ongoing Data Wrangler costs)

**Important:** Once you export the code, you don't need Data Wrangler anymore - just run the generated code.

**Lab lesson:** "For 699 rows, desktop processing would be free - only use cloud for large datasets"

---

## Key Exam Points

✅ **Data Wrangler:** Code generator only, exports to notebooks/pipelines
✅ **Canvas:** Complete no-code ML platform with Data Wrangler-like features
✅ **Lab used Canvas:** Not standalone Data Wrangler
✅ **Natural language:** Both can use prompts to generate transformations
✅ **Visual workflow:** Both show transformation pipelines
✅ **Code generation:** Both create Python/PySpark code from visual designs
✅ **Key difference:** Data Wrangler stops at code export, Canvas continues to ML
✅ **Two-file ML:** Training (with answers) vs Prediction (without answers)
✅ **Business insights:** Can discover actionable patterns in data

---

## Decision Tree

### Use Standalone Data Wrangler When:
- Need to generate code for existing ML pipelines
- Want to integrate with SageMaker Processing/Pipelines
- Have data scientists who will write additional ML code
- Need custom model architectures

### Use Canvas When:
- Want complete no-code ML solution
- Business users need to build models
- Quick prototyping and experimentation
- Don't want to write any code

### Lab Lesson:
**The instructor demonstrated Canvas capabilities while calling it "Data Wrangler" - this is common because Canvas includes Data Wrangler-style features, but it's actually a much more comprehensive platform that runs the code and builds models automatically.**