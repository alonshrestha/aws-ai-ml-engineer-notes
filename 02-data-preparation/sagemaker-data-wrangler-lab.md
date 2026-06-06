# Data Wrangler Lab - Employee Absenteeism Prediction

## Lab Overview

**Instructor:** Used SageMaker Canvas (called it "Data Wrangler" but actually used Canvas end-to-end)

**Business Problem:** Predict employee absenteeism hours based on employee characteristics

**Dataset:** Employee data from UCI repository (~699 rows)

**Warning:** "Don't follow along - this costs real money and UI changes daily"

---

## Step-by-Step Lab Walkthrough

### Step 1: Setup and Access
```
1. Opened SageMaker Studio
2. Already had domain setup (single user configuration)
3. Launched Canvas from Studio
4. Chose "Import and Prepare" data option
```

### Step 2: Data Import
**Data source:** S3 bucket "sagemaker-sundog"
**File:** "absentee-train.csv" 
**Data type:** Tabular CSV data

**Data preview showed columns:**
- Distance from work
- Service time  
- Age
- Average workload per day
- Hit target
- Disciplinary action
- Education level
- Drinking habits
- Smoking habits
- Pet ownership
- **Target:** Absenteeism time in hours

**Problem spotted:** Workload average column had commas in numbers, treated as string

### Step 3: Automated Data Analysis (10 minutes, ML m5.4xlarge)
**Canvas automatically analyzed the data:**

**✅ Good news:**
- No missing data found
- 699 total rows

**❌ Issues discovered:**
- **33% duplicate rows** (suspicious - likely fake data)
- **Outliers detected** (but seemed reasonable)
- **Poor quick model performance** (low predictive power)

**Key insight:** "With that many features, 33% duplicates is very unlikely by chance"

### Step 4: Data Transformation Using Natural Language

#### Transformation 1: Fix Data Type Issue
**Problem:** Workload average treated as string due to commas
```
User prompt: "Transform the workload average per day feature from a string 
to a long type, accounting for the use of commas in the numbers"

Canvas generated Python code:
- Strip out commas from numbers
- Convert to long integer type
- Added to transformation pipeline
```

#### Transformation 2: Remove Suspicious Duplicates
**Problem:** 33% duplicate rows seemed fake
```
User prompt: "Remove duplicate rows"

Canvas generated Python code:
df.drop_duplicates()
- Added to transformation pipeline
```

### Step 5: Visual Data Flow Created
**Canvas showed transformation pipeline:**
```
S3 Source → Data Types → Fix Workload Column → Remove Duplicates → Clean Data
```

**Instructor noted:** "We've built up a flow for processing our data"

### Step 6: Export and Model Creation
```
1. Named processed dataset: "absenteeism processed"
2. Exported transformed data
3. Created model: "absenteeism model"
4. Selected problem type: "Predictive analysis" (tabular data)
5. Target column: "Absenteeism time in hours"
```

**Canvas recommendation:** Changed from regression to classification
**Reason:** Better to classify hours as categories than predict exact numbers

### Step 7: Model Training
**Options available:**
- Quick build: 2-15 minutes (chosen)
- Standard build: Longer but more accurate

**Training completed:** Model built successfully

### Step 8: Model Results and Business Insights

#### Top Predictors Discovered:
1. **Transportation expense** (biggest predictor)
   - People who pay more to get to work call in sick more
2. **Workload average per day** (second biggest)
   - People who work more hours call in sick more

#### Minimal Impact Factors:
- Drinking habits: No correlation
- Smoking habits: No correlation  
- Weight and height: Minimal impact
- Age: Some impact but small

**Business insight:** "Work stress and commute costs drive absenteeism"

### Step 9: Making Predictions
```
1. Uploaded prediction dataset: "absentee-predictor.csv"
2. Same columns as training data but no absenteeism column
3. Model predicted hours for each employee
4. Got confidence scores for predictions
5. Could download results or send to QuickSight
```

**Example prediction:** "This person will be out 2 hours this month"

### Step 10: Additional Canvas Features Explored

#### Ready-to-Use Models:
- **Object detection** using Amazon Rekognition
- **Sentiment analysis** using Comprehend
- **Text detection** using Textract
- **Language detection**

**Demo:** Uploaded photo, Rekognition identified:
- Face, person, hat, sunglasses
- Correctly identified as male
- Made one error (identified hose as person)

#### Generative AI Chat:
- Built-in chatbot interface
- Default model: Claude 2
- Standard conversational AI capabilities

---

## Key Lab Takeaways

### What Actually Happened vs What Was Said
**Said:** "Let's use Data Wrangler"
**Reality:** Used Canvas end-to-end (which includes Data Wrangler-like features)

### Canvas vs Pure Data Wrangler:
**Canvas (what was used):**
- Visual transformations → Runs code immediately → Builds models → Makes predictions

**Pure Data Wrangler:**
- Visual transformations → Generates code → You run code separately

### Business Value Discovered:
```
Key Finding: Transportation costs and workload are main drivers of absenteeism

Actionable Insights:
- Provide transportation subsidies to reduce absenteeism
- Monitor workload to prevent burnout
- Personal habits (drinking/smoking) don't matter for prediction
```

### Technical Lessons:
1. **Natural language prompts** work for data transformations
2. **Automated analysis** catches data quality issues
3. **Visual pipelines** make transformations transparent
4. **AutoML** can recommend better model types
5. **Duplicate detection** reveals fake/suspicious data

---

## Cost and Resource Management

### Lab Costs Incurred:
- **Data analysis:** ML m5.4xlarge for 10 minutes (~$5-10)
- **Model training:** Quick build 2-15 minutes (~$5-15)
- **Canvas interface:** Running time (~$5-10)
- **Total estimated:** $15-35 for the demo

### Instructor's Warning:
```
"For a dataset this small (699 rows), I would have been better off 
analyzing it on my desktop for free using Python code. Don't use 
expensive cloud systems if you don't need to."
```

### Critical Reminders:
- **Always log out** when done (instances keep running)
- **Shut down endpoints** (they rack up charges)
- **Check for running resources** before leaving
- **Small datasets** don't need cloud processing

---

## Lab Architecture (What Actually Ran)

```
┌─────────────────────────────────────────────────────────────┐
│                    CANVAS END-TO-END FLOW                  │
└─────────────────────────────────────────────────────────────┘

Step 1: Data Import
┌─────────────────┐
│ S3 Bucket       │
│ absentee-       │ 
│ train.csv       │
└─────────────────┘
         │
         ▼
Step 2: Canvas Analysis (ML m5.4xlarge - 10 min)
┌─────────────────┐
│ Automated       │
│ Data Quality    │
│ Analysis        │
└─────────────────┘
         │
         ▼
Step 3: Natural Language Transformations
┌─────────────────┐
│ "Fix workload   │
│ column commas"  │ → Python code generated & executed
│ "Remove dupes"  │ → Python code generated & executed
└─────────────────┘
         │
         ▼
Step 4: AutoML Model Training (Quick Build)
┌─────────────────┐
│ Classification  │
│ Model Training  │
│ (2-15 minutes)  │
└─────────────────┘
         │
         ▼
Step 5: Predictions
┌─────────────────┐
│ New employee    │
│ data → Model    │
│ → Absenteeism   │
│ predictions     │
└─────────────────┘
```

---

## Exam-Relevant Points

### What the Lab Demonstrated:
✅ **Canvas capabilities:** Complete no-code ML platform
✅ **Natural language processing:** Prompts generate Python code
✅ **Automated analysis:** Finds data quality issues automatically
✅ **Visual workflows:** Shows transformation pipeline
✅ **AutoML:** Recommends optimal model types
✅ **Business insights:** Discovers actionable patterns in data

### What Pure Data Wrangler Would Do:
✅ **Generate code only:** Export Python for you to run separately
✅ **Visual transformations:** Same interface for designing transforms
✅ **No model building:** Stops at data preparation
✅ **No predictions:** You handle ML training separately

### Key Distinction for Exam:
**Canvas = Complete ML platform with Data Wrangler-like features**
**Data Wrangler = Code generation tool only**

**The lab showed Canvas end-to-end, not standalone Data Wrangler!**