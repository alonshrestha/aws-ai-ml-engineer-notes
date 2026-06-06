# Missing Data Imputation (Feature Engineering)

## What is Missing Data?

**Real-world datasets always have missing values.** Some rows will have empty cells where data should be.

**Example Dataset:**

| Name | Age | Income | City |
|------|-----|--------|------|
| John | 25 | 50000 | NYC |
| Sarah | ??? | 60000 | LA |
| Mike | 30 | ??? | Boston |
| Lisa | 28 | 55000 | ??? |

**Problem:** ML models can't train on missing data. You must fill in the blanks (impute) or remove rows.

---

## Imputation Methods (Worst to Best)

### 1. Drop Rows with Missing Data ❌

**What it does:** Delete any row that has missing values

```python
df.dropna()  # One line of code
```

**Pros:**
- Fastest and easiest (1 line of code)
- No guessing needed

**Cons:**
- Loses data (bad for small datasets)
- Can bias your dataset
- **Never the best answer on exams**

**Bias Example:**
- People with very high/low incomes might not report income
- Dropping these rows removes all high/low earners from your model
- Model becomes inaccurate for those groups

**When to use:** Quick experiments only, never production

---

### 2. Mean/Median Replacement ⚠️

**What it does:** Replace missing values with the column's mean (or median)

**Example:**

| Age | Income |
|-----|--------|
| 25 | 50000 |
| ??? | 60000 |
| 30 | ??? |
| 28 | 55000 |

**Mean of Age column:** (25 + 30 + 28) ÷ 3 = 27.67
**Mean of Income column:** (50000 + 60000 + 55000) ÷ 3 = 55000

**After imputation:**

| Age | Income |
|-----|--------|
| 25 | 50000 |
| **27.67** | 60000 |
| 30 | **55000** |
| 28 | 55000 |

```python
df['Age'].fillna(df['Age'].mean())
df['Income'].fillna(df['Income'].mean())
```

**Pros:**
- Fast and simple
- Doesn't change the overall mean of the dataset
- Keeps all rows

**Cons:**
- Ignores relationships between columns
- Can create nonsensical data
- Doesn't work for categorical data
- **Not the best answer on exams**

#### Mean vs Median: When to Use Which?

**Use Mean when:** No outliers in data

**Use Median when:** Outliers exist

**Example with outliers:**

Income data: $30K, $35K, $40K, $45K, $5M (billionaire)

- **Mean:** $1.03M (skewed by billionaire) ❌
- **Median:** $40K (not affected by outlier) ✅

**For categorical data:** Use most frequent value instead

---

### 3. Substitute Similar Field ⚠️

**What it does:** Copy data from a related column

**Example:**
- Dataset: Movie reviews
- Columns: "Review Summary" and "Full Review"
- If "Full Review" is empty, copy "Review Summary" into it

**When to use:** When you have related fields that can substitute for each other

---

### 4. K-Nearest Neighbors (KNN) ✅

**What it does:** Find similar rows and average their values

**How it works:**

1. Find K most similar rows (e.g., K=5)
2. Average the values from those rows
3. Use that average to fill missing data

**Example:**

Row with missing income:
- Age: 30, City: NYC, Education: Bachelor's

Find 5 most similar people:
- Person A: Age 29, NYC, Bachelor's, Income: $55K
- Person B: Age 31, NYC, Bachelor's, Income: $58K
- Person C: Age 30, Boston, Bachelor's, Income: $52K
- Person D: Age 32, NYC, Bachelor's, Income: $60K
- Person E: Age 28, NYC, Master's, Income: $65K

**Average income:** ($55K + $58K + $52K + $60K + $65K) ÷ 5 = $58K

**Fill missing value with $58K**

**Pros:**
- Considers relationships between features
- More accurate than mean replacement
- Works well for numerical data

**Cons:**
- Harder to implement than mean replacement
- Best for numerical data, not categorical
- Computationally expensive

**For categorical data:** Use mode (most common value) from K neighbors or use Hamming distance

---

### 5. Deep Learning Model ✅✅

**What it does:** Train a neural network to predict missing values

**How it works:**
1. Build a model where missing feature is the target
2. Use other features as inputs
3. Train model to predict the missing value
4. Use predictions to fill missing data

**Example:**
- Missing: Income
- Inputs: Age, Education, City, Years of Experience
- Train neural network: Age + Education + City + Experience → Income
- Predict income for rows with missing values

**Pros:**
- Most accurate method
- Excellent for categorical data
- Captures complex relationships

**Cons:**
- Most complicated (lots of code)
- Requires tuning
- Computationally expensive
- Takes time to implement

**When to use:** Production systems where accuracy matters most

---

### 6. Regression-Based Imputation ✅

**What it does:** Use linear/non-linear regression to predict missing values

**How it works:**
- Build regression model: Other features → Missing feature
- Use model to predict missing values

**Example:**
```
Income = β₀ + β₁(Age) + β₂(Education) + β₃(Experience)
```

**Pros:**
- Finds linear/non-linear relationships
- More accurate than mean replacement
- Simpler than deep learning

**Cons:**
- Assumes relationships are linear (unless using non-linear regression)
- Requires statistical knowledge

---

### 7. MICE (Multiple Imputation by Chained Equations) ✅✅✅

**What it does:** State-of-the-art imputation technique using multiple regressions

**How it works:**
1. Fill all missing values with initial guesses (e.g., mean)
2. For each column with missing data:
   - Build regression model using other columns
   - Predict missing values
   - Update the column
3. Repeat steps 2-3 multiple times until values stabilize
4. Create multiple complete datasets
5. Average results across datasets

**Pros:**
- Most sophisticated method
- Handles complex relationships
- Accounts for uncertainty
- **Best answer on certification exams**

**Cons:**
- Complex to implement
- Computationally intensive
- Requires understanding of statistics

**Exam tip:** If you see "MICE" as an option for imputation, it's likely the correct answer!

---

## Best Solution: Get More Data! 🎯

**The real best approach:** Collect better quality data from the start

**Why:**
- No imputation is perfect
- Real data is always better than guessed data
- Prevents bias and inaccuracy

**How:**
- Go back to data source
- Improve data collection process
- Make fields required
- Validate data at entry time

**Remember:** All imputation methods are workarounds. Better data quality beats any imputation technique!

---

## Quick Comparison Table

| Method | Speed | Accuracy | Complexity | Exam Answer? |
|--------|-------|----------|------------|--------------|
| Drop rows | ⚡⚡⚡ | ❌ | Easy | ❌ Never |
| Mean/Median | ⚡⚡⚡ | ⚠️ | Easy | ❌ Rarely |
| KNN | ⚡⚡ | ✅ | Medium | ✅ Good |
| Deep Learning | ⚡ | ✅✅ | Hard | ✅ Best for categorical |
| Regression | ⚡⚡ | ✅ | Medium | ✅ Good |
| MICE | ⚡ | ✅✅✅ | Hard | ✅✅ Best overall |
| Get more data | N/A | ✅✅✅ | N/A | ✅✅✅ Ideal |

---

## AWS Implementation

### Small-Scale (SageMaker + scikit-learn)

```python
from sklearn.impute import SimpleImputer, KNNImputer
import pandas as pd

# Mean imputation
imputer = SimpleImputer(strategy='mean')
df_imputed = imputer.fit_transform(df)

# KNN imputation
knn_imputer = KNNImputer(n_neighbors=5)
df_imputed = knn_imputer.fit_transform(df)
```

### Large-Scale (EMR + Spark)

```python
from pyspark.ml.feature import Imputer

imputer = Imputer(
    inputCols=['Age', 'Income'],
    outputCols=['Age_imputed', 'Income_imputed']
).setStrategy('mean')

df_imputed = imputer.fit(df).transform(df)
```

---

## AWS Certification Exam Tips

### Key Points to Remember

1. **Dropping data is almost never the best answer**
   - Only acceptable for quick experiments
   - Can bias your dataset

2. **Mean/Median replacement is simple but not best**
   - Ignores feature relationships
   - Use median when outliers exist

3. **KNN is good for numerical data**
   - Considers feature relationships
   - Not ideal for categorical data

4. **Deep Learning is best for categorical data**
   - Most accurate but most complex
   - Neural networks excel at classification

5. **MICE is state-of-the-art**
   - If it's an option, likely the correct answer
   - Handles complex relationships

6. **Getting more data is always best**
   - Better than any imputation method
   - Prevents bias and inaccuracy

### Sample Exam Questions

**Q1:** "You have a dataset with 10% missing values across multiple features. What's the BEST approach for production?"

**Wrong answers:**
- Drop all rows with missing data ❌
- Use mean replacement ❌

**Correct answers:**
- Use MICE ✅
- Use KNN imputation ✅
- Train a deep learning model ✅

**Q2:** "Your income column has missing values and contains billionaires (outliers). What should you use?"

**Answer:** Median replacement (not mean) ✅

**Q3:** "What's the most accurate way to impute categorical data?"

**Answer:** Deep Learning / Neural Networks ✅

---

## Common Mistakes to Avoid

❌ Using mean when outliers exist (use median instead)
❌ Thinking dropping data is acceptable for production
❌ Ignoring relationships between features
❌ Using mean replacement for categorical data
❌ Not checking if dropping data biases your dataset
