# Feature Engineering - AWS AI/ML Exam Focus

## What is Feature Engineering?

**Feature Engineering** is the process of selecting, transforming, and creating features (data attributes) to improve machine learning model performance.

**Features** = The input attributes/columns used to train your model (age, height, income, etc.)

## Core Feature Engineering Tasks

### 1. Feature Selection
- Choose relevant features that impact your prediction target
- Remove irrelevant or redundant features
- Use domain knowledge to identify important attributes

### 2. Feature Transformation

**Why transform?** ML algorithms work better when data is in the right format and scale.

#### Normalization/Scaling
Make numbers comparable by adjusting their ranges.
- **Example**: Age (0-100) and salary ($20k-$200k) have different scales
- **Solution**: Scale both to 0-1 range so neither dominates the model
- **Common methods**: Min-Max scaling, Standardization (Z-score)

#### Encoding
Turn categories into numbers that algorithms can understand.
- **Example**: Colors (Red, Blue, Green) → Numbers
- **One-hot encoding**: Red=[1,0,0], Blue=[0,1,0], Green=[0,0,1]
- **Label encoding**: Red=0, Blue=1, Green=2 (use when order matters)

#### Mathematical Transforms
Change the shape of your data distribution.
- **Log transform**: Compress large values (income: $1M → 6, $10M → 7)
- **Square root**: Similar to log but gentler
- **Polynomial**: Create x², x³ to capture curved relationships

#### Binning
Group continuous numbers into categories.
- **Example**: Age 25 → "Young Adult" (18-30)
- **Why**: Simplifies patterns, reduces noise
- **Use case**: Income ranges, age groups, temperature bands

### 3. Handling Missing Data
- **Imputation**: Fill missing values with mean, median, mode
- **Deletion**: Remove rows/columns with missing data
- **Indicator variables**: Flag missing values as separate feature

### 4. Feature Creation

Create new features from existing ones - like combining ingredients to make something new!

#### Derived Features
Create new features by doing math on existing ones.

**Example:**
- You have: Height (meters) and Weight (kg)
- You create: BMI = Weight / (Height²)
- Now you have 3 features instead of 2!
- **Why?** BMI might predict health better than height/weight alone.

#### Interaction Features
Multiply features together to capture how they work together.

**Example:**
- You have: Study Hours = 5, IQ = 120
- You create: Study × IQ = 600
- This captures "smart people who study a lot"
- **Why?** Sometimes the combination matters more than individual values.

#### Time-Based Features
Break dates into useful pieces.

**Example:**
- You have: Purchase Date = "2024-12-25"
- You create:
  - Day of week = Wednesday
  - Month = December
  - Season = Winter
  - Is Holiday? = Yes
- **Why?** People shop differently on weekends vs weekdays, or during holidays.

#### Text Features
Turn words into numbers the model can use.

**Word Counts:**
- Text: "I love this product, love it!"
- Feature: love_count = 2

**TF-IDF (Term Frequency-Inverse Document Frequency):**
- Measures how important a word is in a document
- "The" appears everywhere → low score
- "Quantum" appears rarely → high score

**Sentiment Scores:**
- Text: "This is amazing!"
- Sentiment score = 0.95 (very positive)

**Why?** Models can't read text, they need numbers.

## The Curse of Dimensionality

### The Problem: Too Many Features = Poor Performance

Imagine finding your friend in different spaces:
- **1D (a line):** Easy! They're either left or right
- **2D (a room):** Harder - anywhere on the floor
- **3D (a building):** Even harder - any floor, any room
- **1000D (1000 features):** Nearly impossible! Data points are so spread out, patterns disappear

#### Why Too Many Features Hurt:

**1. Sparse Data (Data Spread Too Thin)**
- You have 100 data points
- With 2 features: Points are close together, easy to see patterns
- With 1000 features: 100 points lost in massive space, too far apart
- **Analogy:** 100 people in a room = crowded. 100 people in a country = you'll never meet anyone.

**2. Computational Complexity (Slow & Expensive)**
- More features = more calculations
- Training takes forever, costs more money
- **Example:** 10 features = 1 minute. 1000 features = 10 hours.

**3. Overfitting (Memorizing Instead of Learning)**
- Model finds fake patterns in noise
- Memorizes training data perfectly but fails on new data
- **Analogy:** Student who memorizes answers instead of understanding - fails on new questions.

**4. Harder Optimization (Too Many Possibilities)**
- Model searches through too many combinations
- Like finding a needle in a haystack that keeps getting bigger

### The Solutions: Dimensionality Reduction

**Goal:** Keep enough features to be accurate, but not so many that you hurt performance.

#### 1. Manual Selection
You (the human) pick important features based on your knowledge.

**Example - Predicting house prices:**
- Keep: Square footage, location, bedrooms
- Remove: Owner's favorite color, day of week listed
- **Why it works:** You know what matters from experience.

#### 2. PCA (Principal Component Analysis)
Math that combines features into fewer "super features" that capture most information.

**Example:**
- You have: Height, Weight, Age, Shoe Size (4 features)
- PCA creates: "Body Size Factor" (1 feature capturing most info from all 4)
- Result: 4 features → 1 feature, keeping 90% of information!
- **How:** Finds directions where data varies most and keeps those.

#### 3. K-means Clustering
Group similar features together, pick one representative from each group.

**Example:**
- You have: Temperature_Morning, Temperature_Noon, Temperature_Evening
- They're all similar (measuring temperature)
- Keep just one: Average_Temperature
- **Why it works:** No need for 3 features saying almost the same thing.

#### 4. Feature Importance
Let the model tell you which features actually matter.

**How it works:**
- Train model with all features
- Model ranks: "Feature A = 40% important, Feature B = 30%, Feature C = 2%..."
- Keep top ones, drop useless ones

**Example - Predicting customer churn:**
- Model says: Contract length = 50% important, Favorite color = 0.1% important
- Drop favorite color!

**Bottom line:** More features ≠ better model. Too many = confused, slow, overfitting model.

## AWS Services for Feature Engineering

### SageMaker Data Wrangler
- **Visual interface** for data preparation and feature engineering
- **Built-in transformations**: 300+ pre-built data transforms
- **Feature insights**: Automatically detect data quality issues
- **Export options**: Generate code for production pipelines

### SageMaker Processing Jobs
- **Distributed processing** for large-scale feature engineering
- **Custom containers**: Use any framework (Pandas, Spark, Scikit-learn)
- **Batch transformation**: Process entire datasets efficiently

### AWS Glue
- **ETL service** for data transformation at scale
- **Crawlers**: Automatically discover and catalog data schemas
- **Job scheduling**: Automate feature engineering pipelines
- **Integration**: Works with S3, Redshift, RDS

### SageMaker Feature Store
- **Centralized repository** for ML features
- **Online/Offline storage**: Real-time and batch feature serving
- **Feature versioning**: Track feature evolution over time
- **Feature sharing**: Reuse features across teams and projects

## Best Practices for AWS Exam

### Feature Selection Strategies
- **Correlation analysis**: Remove highly correlated features
- **Univariate selection**: Statistical tests for feature importance
- **Recursive feature elimination**: Iteratively remove least important features
- **L1 regularization**: Automatically zeros out irrelevant features

### Common Transformations
- **Standardization**: Mean=0, StdDev=1 for algorithms sensitive to scale
- **Min-Max scaling**: Scale to [0,1] range
- **Log transformation**: Handle skewed distributions
- **One-hot encoding**: Convert categories to binary features

### Missing Data Strategies
- **Mean/Median imputation**: For numerical features
- **Mode imputation**: For categorical features
- **Forward/Backward fill**: For time series data
- **Model-based imputation**: Use ML to predict missing values

### Performance Considerations
- **Feature engineering pipelines**: Automate transformations
- **Cross-validation**: Ensure transforms don't leak future information
- **Monitoring**: Track feature drift in production
- **A/B testing**: Compare feature engineering approaches

## Exam Key Points

### When to Use What
- **High cardinality categories**: Use target encoding or embeddings
- **Skewed distributions**: Apply log or Box-Cox transformations
- **Time series**: Create lag features, rolling statistics
- **Text data**: Use TF-IDF, word embeddings, or BERT features
- **Images**: Extract features using pre-trained CNNs

### AWS-Specific Knowledge
- **SageMaker built-in algorithms**: Know which require specific feature formats
- **Real-time inference**: Consider feature computation latency
- **Batch transform**: Use for offline feature generation
- **Multi-model endpoints**: Share feature engineering across models

### Common Pitfalls
- **Data leakage**: Don't use future information to predict past
- **Overfitting**: Too many features relative to training samples
- **Inconsistent preprocessing**: Ensure train/test transforms match
- **Feature drift**: Monitor feature distributions in production