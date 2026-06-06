# Amazon Comprehend

## What is Amazon Comprehend?
**Amazon Comprehend** is AWS's **text analysis service** that reads text and tells you what it means.

Think of it as a **smart assistant that reads documents** and answers questions like:
- "What language is this?"
- "Is this positive or negative?"
- "What are the important words?"
- "Who or what is mentioned?"

## What It Does (Core Features)

### **1. Language Detection**
- **Reads text** → **Tells you the language**
- **Example**: "Hola mundo" → **Spanish**

### **2. Sentiment Analysis** 
- **Reads text** → **Tells you if it's positive/negative**
- **Example**: "I love this product!" → **Positive**
- **Example**: "This is terrible" → **Negative**

### **3. Key Phrases**
- **Finds important words/phrases** in text
- **Example**: "I bought a red car yesterday" → **"red car", "yesterday"**

### **4. Entity Recognition**
- **Finds people, places, companies** mentioned in text
- **Example**: "John works at Amazon in Seattle" → **Person: John, Company: Amazon, Place: Seattle**

### **5. Topic Modeling**
- **Groups similar documents** together by topic
- **Example**: 100 news articles → **Groups: Sports, Politics, Technology**

## Two Types of Models

### **Built-in Models (Ready to Use)**
- **No training needed** - Just send text and get results
- **General purpose** - Works for most common use cases
- **Examples**: Sentiment analysis, language detection

### **Custom Models (You Train Them)**
- **Train on your specific data** 
- **Business-specific** - Understands your terminology
- **Two types**: Custom Classification & Custom Entity Recognition

## Custom Classification

### **What It Does:**
**Sorts documents into categories you define**

### **Example:**
**Customer emails** sorted into:
- Complaint
- Question  
- Compliment
- Refund Request

### **How It Works (Step-by-Step):**

#### **Step 1: YOU Manually Label Training Data**
**Important**: Comprehend doesn't know what a "complaint" is - YOU must teach it!

**You create a CSV file with labeled examples:**
```csv
text,label
"Your product is broken! I want a refund!",COMPLAINT
"How do I reset my password?",QUESTION
"Thank you for the great service!",COMPLIMENT
"The delivery was late and damaged",COMPLAINT
"What are your business hours?",QUESTION
"Best purchase ever!",COMPLIMENT
```

**YOU manually:**
- Collect 100-1000 real examples from your business
- Read each document
- Decide which category it belongs to
- Type the label next to each document

#### **Step 2: Upload to S3**
```
s3://my-bucket/training-data/labeled-emails.csv
```

#### **Step 3: Train the Model**
- Go to Comprehend console
- Create custom classifier
- Point to your S3 training data
- Wait for training to complete (can take hours)

#### **Step 4: Comprehend Learns Patterns**
Comprehend analyzes your labeled data and learns:
- **COMPLAINT** emails have words like: broken, refund, terrible, worst, disappointed
- **QUESTION** emails have words like: how, what, when, where, can you
- **COMPLIMENT** emails have words like: great, love, amazing, best, thank you

#### **Step 5: Use on New Data**
```
Input: "This product stopped working after one week"
Output: COMPLAINT (confidence: 92%)
```

### **Training Data Format Options:**

**Option 1: CSV File (Recommended)**
```csv
text,label
"Document text here",CATEGORY_NAME
"Another document",CATEGORY_NAME
```

**Option 2: Separate Folders**
```
s3://my-bucket/training/
├── complaints/
│   ├── email1.txt
│   ├── email2.txt
│   └── email3.txt
├── questions/
│   ├── email1.txt
│   └── email2.txt
└── compliments/
    └── email1.txt
```

### **File Types Supported:**
- Text files (.txt)
- PDFs (.pdf)
- Word documents (.docx)
- Images (extracts text first)

### **Key Point:**
**YOU do the manual labeling work once, then Comprehend automates classification for all future documents!**

### **Why Use Comprehend If You Already Labeled Data?**

**The Value: Label 100 to Automate 100,000!**

#### **Without Comprehend:**
```
Day 1: You manually label 100 emails ✅
Day 2: 50 new emails → YOU manually label ❌
Day 3: 75 new emails → YOU manually label ❌
Year 1: 20,000 emails → YOU manually label ALL ❌❌❌
```
**You're stuck labeling forever!**

#### **With Comprehend:**
```
Day 1: You manually label 100 emails (training) ✅
Day 2: Train model (one-time, automatic) ✅
Day 3: 50 new emails → Comprehend labels automatically ✅
Day 4: 75 new emails → Comprehend labels automatically ✅
Year 1: 20,000 emails → ALL labeled automatically ✅✅✅
```
**You're free! Comprehend does the work!**

#### **Real Numbers:**
- **Manual labeling**: 30 seconds per email
- **100 emails**: 50 minutes (your training work)
- **10,000 emails without Comprehend**: 83 hours (2 work weeks!)
- **10,000 emails with Comprehend**: Automatic (seconds)

#### **The Point:**
**Training data = Teaching (one-time work)**
**Future data = Automated forever (no more work)**

**You teach Comprehend with 100 examples, it automatically handles millions of future documents!**

## Custom Entity Recognition

### **What It Does:**
**Finds specific terms important to your business**

### **Example:**
**Insurance company** wants to find:
- Policy numbers (like "POL-12345")
- Claim IDs (like "CLM-67890")
- Customer escalation phrases (like "speak to manager")

### **How It Works:**
1. **You provide examples** - Documents with these terms highlighted
2. **Comprehend learns** to recognize similar patterns
3. **New documents** → **Automatically finds these terms**

## Processing Options

### **Real-time (Synchronous)**
- **One document at a time**
- **Instant results**
- **Good for**: Live chat analysis, real-time feedback

### **Batch (Asynchronous)**
- **Many documents at once**
- **Takes time to process**
- **Good for**: Analyzing thousands of documents overnight

## Model Sharing Between Accounts

### **What You Can Do:**
- **Train model in Account A**
- **Share it with Account B**
- **Both accounts can use the same model**

### **Requirements:**
- **Same AWS region**
- **Model ARN** (unique identifier)
- **IAM permissions** set up correctly
- **Optional encryption key**

## Common Use Cases

### **Customer Service:**
- **Email sorting** - Route complaints vs questions to different teams
- **Sentiment tracking** - Monitor if customers are getting happier/angrier
- **Escalation detection** - Find angry customers automatically

### **Content Management:**
- **Document organization** - Sort research papers by topic
- **News categorization** - Group articles by subject
- **Knowledge base** - Organize help articles

### **Business Intelligence:**
- **Social media monitoring** - Track brand mentions and sentiment
- **Market research** - Analyze customer feedback
- **Competitor analysis** - Monitor what people say about competitors

## Simple Workflow

### **For Built-in Features:**
```
Your Text → Comprehend → Results (sentiment, entities, etc.)
```

### **For Custom Models:**
```
1. Collect training data → Store in S3
2. Train model → Wait for completion  
3. Use model → Send new text → Get custom results
```

## Key Benefits
- **No machine learning expertise** needed
- **Fully managed** - AWS handles everything
- **Pay per use** - Only pay for what you analyze
- **Scales automatically** - Handle 1 document or 1 million
- **Multiple languages** supported

## Exam Focus Points
- **Difference between custom classification and custom entity recognition**
- **Real-time vs batch processing** options
- **Cross-account model sharing** requirements
- **Integration with S3** for training data and results
- **Use cases** for sentiment analysis and document organization
