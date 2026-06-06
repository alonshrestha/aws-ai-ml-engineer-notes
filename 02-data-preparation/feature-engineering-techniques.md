# Feature Engineering Techniques - Quick Reference

## 1. Binning

**What:** Convert numerical data into categorical data by grouping values into ranges.

**Example:**
- Ages: 22, 25, 28 → "20-somethings" category
- Ages: 31, 35, 39 → "30-somethings" category

**When to Use:**
- Measurements have uncertainty/imprecision
- Want to reduce noise in data
- Need to cover up measurement errors

**Types:**
- **Regular binning:** Fixed ranges (0-10, 11-20, 21-30)
- **Quantile binning:** Each bin has equal number of samples

## 2. Transforming Data

**What:** Apply mathematical functions to features to make them better suited for algorithms.

**Common Transformations:**
- **Logarithmic:** Makes exponential trends linear
- **Square:** Captures super-linear relationships
- **Square root:** Captures sub-linear relationships

**Real Example - YouTube:**
- Original feature: `x` (time since last video watched)
- Also feed in: `x²` and `√x`
- Creates 3 features from 1 to capture non-linear patterns

**Why:** Models often struggle with non-linear data; transformations help reveal patterns.

## 3. Encoding

**What:** Transform data into specific format required by your model.

### One-Hot Encoding

**Most Common Example:** Converting categories into binary vectors.

**How it Works:**
- Digit "8" → `[0, 0, 0, 0, 0, 0, 0, 0, 1, 0]`
- Position 9 (counting from 0) = 1, all others = 0

**Why:** Deep learning neurons are either on (1) or off (0), can't process raw numbers directly.

**Structure:**
- Create one bucket per category
- 1 = this category exists
- 0 = not this category

## 4. Scaling & Normalization

**What:** Adjust feature data to comparable ranges, ideally normally distributed around zero.

**Why Needed:**
- Features with larger magnitudes get more weight
- Example: Income ($50,000) vs Age (30) - income would dominate without scaling

**When Required:**
- Most deep learning and neural networks
- Most ML models (except decision trees)

**Tools:**
- **scikit-learn:** `MinMaxScaler` in pre-processor module
- **Remember:** Scale results back up if predicting numeric values (not categories)

## 5. Shuffling

**What:** Randomize the order of training data.

**Why:**
- Eliminates residual signals from data collection order
- Prevents bias from how data was originally organized
- Often significantly improves model quality

**When:** Before training most algorithms.

## Quick Comparison Table

| Technique | Input Type | Output Type | Main Purpose |
|-----------|-----------|-------------|--------------|
| Binning | Numerical | Categorical | Reduce noise, handle uncertainty |
| Transforming | Numerical | Numerical | Capture non-linear patterns |
| One-Hot Encoding | Categorical | Binary vectors | Format for neural networks |
| Scaling | Numerical | Normalized numerical | Balance feature importance |
| Shuffling | Any | Same (reordered) | Remove collection bias |

## Exam Tips

- **Binning:** Know quantile binning ensures equal samples per bin
- **Transforming:** YouTube example - feeding x, x², √x together
- **One-Hot Encoding:** Understand the visual representation (likely on exam)
- **Scaling:** Remember to reverse-scale predictions when needed
- **Shuffling:** Simple but often forgotten - can dramatically improve results

## Key Takeaway

Feature engineering is about finding the balance between:
- **Too little information:** Model can't learn patterns
- **Too much information:** Curse of dimensionality, overfitting

Always consider what information you're adding or removing with each technique.
