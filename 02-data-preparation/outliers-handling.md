# Outliers Handling (Feature Engineering)

## What Are Outliers?

**Outliers = Extreme values that are WAY different from the rest of your data**

**Simple example:**
```
Heights: 5'2", 5'5", 5'8", 5'10", 6'1", 8'5"
                                        ↑
                                    Outlier!
```

---

## 🎯 Why Outliers Matter

**They can mess up your analysis:**

**Example: Average salary**
```
10 people earn: $30,000 each
1 billionaire earns: $1,000,000,000

Average WITH billionaire: $90,909,091 (misleading!)
Average WITHOUT billionaire: $30,000 (realistic!)
```

---

## 📊 How to Find Outliers

### Method 1: Standard Deviation (Most Common)

**Step 1: Find the average (mean)**
```
Data: 1, 4, 5, 4, 8
Mean: (1+4+5+4+8) ÷ 5 = 4.4
```

**Step 2: Calculate standard deviation**
```
Standard deviation = 2.24 (how spread out the data is)
```

**Step 3: Define normal range**
```
Normal range = Mean ± (1 or 2 × Standard Deviation)

Using 1 standard deviation:
Normal: 4.4 - 2.24 to 4.4 + 2.24
Normal: 2.16 to 6.64

Outliers: 1 (too low) and 8 (too high)
```

---

## 🔢 Standard Deviation Explained Simply

**What it measures:** How spread out your data is

**Small standard deviation:**
```
Data: 10, 11, 10, 11, 10
Everything is close together ✅
```

**Large standard deviation:**
```
Data: 1, 50, 2, 48, 3
Data is all over the place ❌
```

**Formula (don't memorize, just understand):**
1. Find the mean
2. Find how far each point is from the mean
3. Square those differences (makes negatives positive)
4. Average them
5. Take square root

**Result:** One number that tells you "how spread out" your data is

---

## 🎯 Simple Rules for Outliers

### Rule 1: Within 1 Standard Deviation
```
68% of data falls here
This is "normal"
```

### Rule 2: Within 2 Standard Deviations
```
95% of data falls here
Outside this = likely an outlier
```

### Rule 3: Within 3 Standard Deviations
```
99.7% of data falls here
Outside this = definitely an outlier
```

---

## 🤔 Should You Remove Outliers?

### ✅ Remove When:

**1. Data errors**
```
Ages: 25, 30, 28, 999
999 is clearly a typo ✅ Remove it
```

**2. Bots/spam**
```
User ratings: Most people rate 1-10 movies
One "user" rated 10,000 movies (probably a bot) ✅ Remove it
```

**3. Not relevant to your goal**
```
Modeling typical customer behavior
Billionaire customers behave differently ✅ Remove them
```

---

### ❌ Keep When:

**1. Real data you care about**
```
Studying income inequality
Billionaires are part of the story ❌ Keep them
```

**2. Important edge cases**
```
Fraud detection
Fraudulent transactions ARE the outliers ❌ Keep them!
```

**3. Specifically asked for mean (not median)**
```
Someone wants the MEAN income
Billionaires affect the mean ❌ Keep them
```

---

## 📈 Visual Example

**Box and Whisker Plot:**
```
    |----[====]----| ●
    ↑    ↑   ↑    ↑ ↑
   Min  Q1  Med  Q3 Outlier
   
Outlier = Beyond 1.5 × IQR (Interquartile Range)
```

---

## 🔧 AWS Solution: Random Cut Forest

**AWS has a built-in outlier detection algorithm**

**Where it's available:**
- Amazon QuickSight
- Amazon Kinesis Analytics
- Amazon SageMaker

**What it does:**
- Automatically finds outliers in your data
- Uses machine learning (you don't need to calculate standard deviation)
- Works on streaming data too

**Exam tip:** If question mentions outliers + AWS → Answer is probably "Random Cut Forest"

---

## 💡 Real-World Examples

### Example 1: Web Traffic
```
Normal: 100-500 requests/hour
Outlier: 50,000 requests/hour

Could be:
- DDoS attack ✅ Remove
- Viral content ❌ Keep
- Bot traffic ✅ Remove

Decision: Investigate first!
```

### Example 2: Movie Ratings
```
Normal users: Rate 5-20 movies
Power user: Rated 5,000 movies

Problem: This one user influences everyone's recommendations

Solution: ✅ Remove (or weight differently)
```

### Example 3: Income Study
```
Goal: Find MEDIAN income ❌ Keep outliers (median ignores them)
Goal: Find MEAN income ❌ Keep outliers (they affect mean)
Goal: Model typical person ✅ Remove outliers (billionaires aren't typical)
```

---

## 🎓 Quick Decision Tree

```
Is the outlier real data?
├─ No (error/typo) → Remove ✅
└─ Yes
   ├─ Is it relevant to your goal?
   │  ├─ No → Remove ✅
   │  └─ Yes → Keep ❌
   └─ Does it skew results unfairly?
      ├─ Yes → Consider removing ✅
      └─ No → Keep ❌
```

---

## 📝 Python Example (Simple)

```python
import numpy as np

# Your data
data = [1, 4, 5, 4, 8, 100]  # 100 is an outlier

# Calculate mean and standard deviation
mean = np.mean(data)  # 20.33
std = np.std(data)    # 37.8

# Define outliers (2 standard deviations)
lower_bound = mean - 2 * std  # -55.27
upper_bound = mean + 2 * std  # 95.93

# Remove outliers
clean_data = [x for x in data if lower_bound <= x <= upper_bound]
# Result: [1, 4, 5, 4, 8] (100 removed)
```

---

## 🎯 Certification Exam Tips

### Key Points to Remember:

1. **Standard deviation is the main tool**
   - 1 SD = 68% of data
   - 2 SD = 95% of data
   - 3 SD = 99.7% of data

2. **Random Cut Forest for AWS**
   - If exam mentions outliers + AWS
   - Answer is likely Random Cut Forest

3. **Think about the business goal**
   - Don't blindly remove outliers
   - Ask: "Does this make sense for what I'm trying to do?"

4. **Box plots show outliers visually**
   - Points beyond 1.5 × IQR are outliers

### Sample Exam Questions:

**Q1:** "You have income data with some billionaires. You want to find the typical person's income. What should you do?"

**Answer:** Remove the billionaire outliers ✅

**Q2:** "Which AWS service can detect outliers in streaming data?"

**Answer:** Random Cut Forest (in Kinesis Analytics) ✅

**Q3:** "Your data has values 2 standard deviations from the mean. Are these outliers?"

**Answer:** Yes, likely outliers ✅

---

## ⚠️ Common Mistakes

❌ Removing all outliers without thinking
❌ Keeping obvious errors because "it's real data"
❌ Not visualizing data before deciding
❌ Forgetting that outliers might be what you're looking for (fraud detection!)
❌ Not knowing Random Cut Forest for AWS exams

---

## 🎯 Summary (TL;DR)

**Outliers = Extreme values far from the rest**

**How to find:**
- Use standard deviation (2-3 SD away = outlier)
- Use box plots (beyond 1.5 × IQR)
- Use AWS Random Cut Forest

**Should you remove?**
- Errors/bots: YES ✅
- Real data you care about: NO ❌
- Think about your goal first!

**AWS exam:** Random Cut Forest is the answer for outlier detection
