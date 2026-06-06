# Unbalanced Data Handling (Feature Engineering)

## What is Unbalanced Data?

**Definition:** Large discrepancy between positive and negative cases in training data.

**Example: Fraud Detection**
- Fraud cases: 0.01% (100 out of 1,000,000 transactions)
- Non-fraud cases: 99.99% (999,900 transactions)

**The Problem:** Model learns to always predict "No fraud" and gets 99.99% accuracy but catches zero fraud!

---

## Key Terminology ⚠️

**Don't get confused by "positive" and "negative":**

- **Positive case** = The thing you're trying to detect (even if it's bad)
- **Negative case** = Everything else

**Examples:**
- Fraud detection: Fraud = Positive, Non-fraud = Negative
- Disease detection: Disease = Positive, Healthy = Negative
- Spam detection: Spam = Positive, Not spam = Negative

**Remember:** Positive ≠ Good, Negative ≠ Bad. It's just what you're looking for!

---

## Solutions (Worst to Best)

### 1. Undersampling ❌

**What it does:** Remove majority class samples to balance the dataset

**Example:**
- Before: 999,900 non-fraud + 100 fraud
- After: 100 non-fraud + 100 fraud

**Pros:**
- Simple to implement
- Reduces training time
- Balances the dataset

**Cons:**
- **Throws away data** (usually bad!)
- Loses valuable information
- Only use when you have too much data to process

**When to use:** Only when hardware can't handle the full dataset

---

### 2. Oversampling ⚠️

**What it does:** Copy minority class samples to balance the dataset

**Example:**
- Before: 999,900 non-fraud + 100 fraud
- After: 999,900 non-fraud + 999,900 fraud (copied 9,999 times each)

```python
# Simple oversampling
fraud_cases = df[df['is_fraud'] == 1]
fraud_copies = fraud_cases.sample(n=999900, replace=True)
balanced_df = pd.concat([non_fraud_cases, fraud_copies])
```

**Pros:**
- Simple to implement
- Keeps all original data
- Actually works with neural networks

**Cons:**
- Just makes exact copies (no new information)
- Can lead to overfitting
- Dataset becomes huge

**When to use:** Quick fix, but SMOTE is better

---

### 3. SMOTE ✅✅ (Best Answer!)

**What it does:** **S**ynthetic **M**inority **O**versampling **TE**chnique

**How it works:**
1. For each minority sample, find K nearest neighbors
2. Create new synthetic samples between the original and its neighbors
3. Uses KNN + averaging (like imputation!)

**Example:**
Original fraud case: Age=30, Amount=$500, Location=NYC
Neighbor fraud case: Age=32, Amount=$600, Location=NYC
**New synthetic case:** Age=31, Amount=$550, Location=NYC

```python
from imblearn.over_sampling import SMOTE

smote = SMOTE(random_state=42)
X_balanced, y_balanced = smote.fit_resample(X, y)
```

**Pros:**
- Creates realistic new samples (not just copies)
- Based on actual data patterns
- **Best answer on certification exams**
- Reduces overfitting compared to simple oversampling

**Cons:**
- More complex than simple oversampling
- Can create unrealistic combinations in some cases

**When to use:** Production systems, certification exams

---

### 4. Threshold Adjustment ✅

**What it does:** Change the probability threshold for classification

**How ML models work:**
- Model outputs probability: "85% chance this is fraud"
- You set threshold: "If >50%, classify as fraud"
- You can adjust this threshold!

**Example:**
- Default threshold: 50%
- Too many false positives? Raise to 80%
- Missing too much fraud? Lower to 20%

**Trade-offs:**
- **Higher threshold:** Fewer false positives, more false negatives
- **Lower threshold:** More false positives, fewer false negatives

**For fraud detection:**
- Better to flag innocent transaction (false positive) than miss fraud (false negative)
- So use **lower threshold** (like 20%)

**Pros:**
- No need to retrain model
- Easy to adjust in production
- Can optimize for business needs

**Cons:**
- Doesn't fix underlying data imbalance
- Still need to choose the right threshold

---

## Quick Comparison

| Method | Data Quality | Complexity | Exam Answer? | Best For |
|--------|-------------|------------|--------------|----------|
| Undersampling | ❌ Loses data | Easy | ❌ Rarely | Hardware limits only |
| Oversampling | ⚠️ Duplicates | Easy | ⚠️ OK | Quick experiments |
| SMOTE | ✅ Creates new | Medium | ✅✅ Best | Production systems |
| Threshold Tuning | ✅ No data change | Easy | ✅ Good | Post-training optimization |

---

## Real-World Example: Credit Card Fraud

**Dataset:**
- 1,000,000 transactions
- 100 fraudulent (0.01%)
- 999,900 legitimate (99.99%)

**Without balancing:**
```
Model prediction: "Never fraud"
Accuracy: 99.99% ✅ (looks great!)
Fraud caught: 0% ❌ (completely useless!)
```

**With SMOTE:**
```
Model prediction: Actually detects patterns
Accuracy: 95% ⚠️ (looks worse)
Fraud caught: 85% ✅ (actually useful!)
```

**Key insight:** Lower accuracy can mean better real-world performance!

---

## AWS Implementation

### Using SageMaker

```python
from imblearn.over_sampling import SMOTE
from sklearn.ensemble import RandomForestClassifier

# Apply SMOTE
smote = SMOTE(random_state=42)
X_balanced, y_balanced = smote.fit_resample(X_train, y_train)

# Train model on balanced data
model = RandomForestClassifier()
model.fit(X_balanced, y_balanced)

# Adjust threshold for predictions
probabilities = model.predict_proba(X_test)[:, 1]
predictions = (probabilities > 0.3).astype(int)  # Lower threshold
```

### Using SageMaker Built-in Algorithms

Many AWS algorithms handle class imbalance automatically:
- **XGBoost:** `scale_pos_weight` parameter
- **Linear Learner:** `positive_example_weight_mult` parameter

---

## Certification Exam Tips

### Key Points to Remember

1. **SMOTE is usually the best answer**
   - Creates synthetic samples using KNN
   - Better than simple oversampling
   - If you see it as an option, likely correct

2. **Undersampling is rarely best**
   - Only when you have too much data to process
   - Throwing away data is usually wrong

3. **Understand positive vs negative terminology**
   - Positive = what you're detecting (even if it's bad)
   - Don't confuse with good/bad

4. **Threshold adjustment is important**
   - Lower threshold = catch more positives (more false positives)
   - Higher threshold = fewer false positives (miss more positives)

5. **Consider business impact**
   - Missing fraud vs annoying customers
   - Missing disease vs unnecessary tests

### Sample Exam Questions

**Q1:** "You have a fraud detection dataset with 0.1% fraud cases. What's the BEST approach?"

**Wrong answers:**
- Remove non-fraud cases to balance ❌
- Just use the imbalanced data ❌

**Correct answer:**
- Use SMOTE to create synthetic fraud samples ✅

**Q2:** "Your model has high accuracy but misses most fraud cases. What should you do?"

**Answer:** Lower the classification threshold ✅

**Q3:** "What does 'positive case' mean in fraud detection?"

**Answer:** Fraud transactions (the thing you're trying to detect) ✅

---

## Common Mistakes to Avoid

❌ Thinking "positive" means good outcome
❌ Using undersampling when you have enough data
❌ Ignoring class imbalance and trusting high accuracy
❌ Not considering business cost of false positives vs false negatives
❌ Forgetting that SMOTE is usually the best answer on exams