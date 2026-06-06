# Building Wikipedia Search with TF-IDF on AWS EMR

## 🎯 What We're Building

**Goal:** Create a simple Wikipedia search engine using TF-IDF (not deep learning!)

**Tech Stack:**
- AWS EMR (Elastic MapReduce)
- Apache Spark (PySpark)
- EMR Studio + Workspace + Notebook
- Real Wikipedia data

---

## 🏗️ The Architecture (Hierarchy)

```
EMR Studio (Top level - collaboration environment)
    ↓
Workspace (Contains your notebook)
    ↓
EMR Serverless Application (Runs Spark)
    ↓
Your Code (PySpark notebook)
```

---

## ⚠️ Important Setup Notes

### Before You Start:

1. **DO NOT use root account!**
   - Log in as IAM user
   - Root account has permission bugs with EMR Studio

2. **It can be flaky**
   - Sometimes serverless cluster doesn't spin up
   - If nothing happens, try again
   - This is normal (unfortunately)

3. **Clean up after!**
   - Delete everything or you'll get surprise bills
   - AWS makes it easy to leave things running

---

## 📋 Step-by-Step Guide

### Step 1: Create EMR Studio

**Navigate:**
```
AWS Console → EMR → Studios → Create Studio
```

**Settings:**
- Workload type: Interactive (pre-selected)
- Studio name: `tfidf` (or your choice)
- Let it create S3 bucket automatically
- Let it create service role automatically
- Workspace name: `tfidf`

**What's happening:**
- AWS creates serverless Spark environment
- No need to manage clusters manually
- Everything is automated

**Click:** Create Studio and Launch Workspace

---

### Step 2: Wait for Workspace

**What you'll see:**
- Workspace launching in new browser tab
- Takes 1-2 minutes
- Status: "Attaching serverless interactive app"

**When ready:**
- You'll see Jupyter-like interface
- PySpark kernel will be available

---

### Step 3: Upload the Notebook

**In the workspace:**
1. Click "Upload files" button
2. Select `tfidf.ipynb` from course materials
3. Overwrite default notebook
4. Double-click to open

**Wait for:**
- Cluster to attach (shows at top)
- Select PySpark kernel
- Status changes to "Attached"

---

### Step 4: Upload Data to S3

**Why:** Your workspace needs access to Wikipedia data

**Find your bucket:**
```
EMR Studio → Select your studio → Workplace Storage
Copy the S3 bucket name
```

**Upload data:**
1. Go to S3 console
2. Navigate to that bucket
3. Upload `subset-small.tsv` from course materials
4. Copy the S3 URI (e.g., `s3://your-bucket/subset-small.tsv`)

**Update notebook:**
- Paste your S3 URI into the first code cell
- Replace the placeholder path

---

### Step 5: Run the Notebook

**Restart kernel first:**
```
Kernel → Restart Kernel and Clear All Outputs
```

**Then run each cell (Shift+Enter):**

#### Cell 1: Load Raw Data
```python
# Loads Wikipedia data from S3
# Shows: ID, Title, Date, Article Text
```

**What you see:** Raw Wikipedia articles (starts with "Anarchism", "Autism", etc.)

---

#### Cell 2: Add Column Names
```python
# Gives columns readable names
# ID, title, time, document
```

**What you see:** Same data, but with proper column headers

---

#### Cell 3: Check for Null Values
```python
# Counts documents with null values
# Result: 1 (we found one!)
```

**Why:** TF-IDF can't handle null documents

---

#### Cell 4: Clean the Data
```python
# Removes null documents
# Verifies: 0 null documents remain
```

**What you see:** Confirmation that data is clean

---

#### Cell 5: Tokenize Words
```python
# Converts words to numbers
# Each unique word gets a hash value
```

**What you see:**
- List of words per document
- Numerical representations (hashes)

**Why:** ML algorithms need numbers, not words

---

#### Cell 6: Calculate TF (Term Frequency)
```python
# Counts how often each word appears in each document
```

**What you see:** Word frequencies for each article

---

#### Cell 7: Calculate IDF (Inverse Document Frequency)
```python
# Calculates how rare/common each word is across all documents
```

**What you see:** TF-IDF scores for each word in each document

---

#### Cell 8: Search for "Gettysburg"

**Step 8a: Get hash value**
```python
# Finds the number that represents "Gettysburg"
# Result: 116,775
```

**Step 8b: Extract TF-IDF scores**
```python
# Gets Gettysburg's score for each document
```

**Step 8c: Sort and display results**
```python
# Orders documents by Gettysburg relevance
```

**Results you'll see:**
1. Abraham Lincoln ✅
2. Abner Doubleday (Civil War general) ✅
3. American Civil War ✅
4. Austin, Texas ✅
5. Assyria ⚠️ (starts getting sketchy)

**Success!** Top results make sense!

---

## 🎓 What We Accomplished

**From scratch, we:**
1. ✅ Loaded raw Wikipedia data
2. ✅ Cleaned the data (removed nulls)
3. ✅ Tokenized words into numbers
4. ✅ Calculated TF-IDF scores
5. ✅ Built a working search engine
6. ✅ Got meaningful results

**All using:**
- AWS EMR Studio
- Serverless Spark
- No cluster management needed!

---

## 🧹 CRITICAL: Clean Up (Don't Skip!)

### Step 1: Delete EMR Serverless Application

```
EMR Console → Manage Applications → Select your app
→ Stop Application (wait for it to stop)
→ Delete Application
```

---

### Step 2: Delete Workspace

```
EMR Console → Workspaces → Select your workspace
→ Delete → Confirm
```

---

### Step 3: Delete Studio

```
EMR Console → Studios → Select your studio
→ Delete → Confirm
```

---

### Optional: Clean Up S3 and IAM

**S3 bucket:**
- Won't cost much, but you can delete it
- Find it in S3 console

**IAM role:**
- Auto-created role can be deleted
- Find it in IAM console

**Why clean up:**
- Avoid surprise charges
- EMR can be expensive if left running
- AWS makes it easy to forget

---

## 🎯 Key Concepts for Exam

### TF-IDF (Term Frequency - Inverse Document Frequency)

**What it does:**
- Finds important words in documents
- Common words (the, and, is) get low scores
- Rare, meaningful words get high scores

**Formula (don't memorize):**
```
TF-IDF = (How often word appears in document) × 
         (How rare word is across all documents)
```

---

### EMR Studio Components

**Studio:**
- Top-level collaboration environment
- Multiple users can work together
- Contains workspaces

**Workspace:**
- Individual notebook environment
- Attached to serverless application
- Where you write code

**EMR Serverless:**
- Runs Spark automatically
- No cluster management
- Scales as needed

---

### When to Use TF-IDF

**Good for:**
- ✅ Document search
- ✅ Finding similar documents
- ✅ Keyword extraction
- ✅ Simple text analysis

**Not good for:**
- ❌ Understanding context
- ❌ Semantic meaning
- ❌ Complex language tasks
- ❌ (Use deep learning instead)

---

## 🎓 Exam Tips

### Key Points:

1. **EMR Studio is for collaboration**
   - Multiple users
   - Shared notebooks
   - Managed environment

2. **EMR Serverless = No cluster management**
   - Automatic scaling
   - Pay for what you use
   - Easier than managing EC2 clusters

3. **TF-IDF is "old school" but effective**
   - No deep learning needed
   - Fast and simple
   - Good baseline for search

4. **Always clean up resources**
   - Delete studio, workspace, serverless app
   - Exam may ask about cost optimization

---

## ⚠️ Troubleshooting

### Problem: Permission Errors
**Solution:** Make sure you're logged in as IAM user, not root

### Problem: Notebook doesn't run
**Solution:** Restart kernel, try again. Serverless can be flaky.

### Problem: No output when running cells
**Solution:** Serverless cluster didn't spin up. Delete and recreate.

### Problem: Out of memory errors
**Solution:** Try running the cell again. Single XL instance is borderline.

---

## 🎯 Summary

**What we learned:**
- How to use EMR Studio
- How to run PySpark notebooks
- How TF-IDF works in practice
- How to build a simple search engine
- How to clean up AWS resources

**Exam relevance:**
- EMR Studio concepts
- Serverless vs cluster management
- TF-IDF use cases
- Cost optimization (cleanup)

**Time to complete:** 30-45 minutes (including cleanup)

**Cost:** ~$1-2 if you clean up immediately
