# Amazon Transcribe

## What is Amazon Transcribe?
**Amazon Transcribe** automatically **converts speech to text**.

**Simple:** You give it audio → It gives you text

## How It Works
- **Uses ASR** (Automatic Speech Recognition)
- **Deep learning powered**
- **Fast and accurate** conversion

## Core Features

### **1. Automatic Speech to Text**
```
Audio Input: "Hello, my name is John"
Text Output: "Hello, my name is John"
```

### **2. PII Redaction (Personally Identifiable Information)**
- **Automatically removes** sensitive information
- **Examples removed**: Names, ages, social security numbers, addresses, phone numbers

```
Audio: "My name is John Smith and my SSN is 123-45-6789"
Output: "My name is [PII] and my SSN is [PII]"
```

### **3. Automatic Language Identification**
- **Detects multiple languages** in same audio
- **Examples**: French, English, Spanish mixed together
- **Automatically switches** between languages

## Common Use Cases

### **Customer Service:**
- **Transcribe customer calls** for analysis
- **Quality assurance** monitoring
- **Compliance recording**

### **Media & Entertainment:**
- **Closed captioning** for videos
- **Subtitling** for movies/shows
- **Automatic captions** for YouTube, streaming

### **Content Management:**
- **Generate metadata** for media assets
- **Create searchable archives** of audio/video
- **Index podcast content**

## Improving Accuracy (Two Methods)

### **Method 1: Custom Vocabularies (For Words)**

#### **Problem:**
```
Audio: "We use AWS Microservices"
Transcribe Output: "We use USA my crow services" ❌
```

#### **Solution: Add Custom Vocabulary**
**What it does:** Teaches Transcribe **specific words** you use frequently

**Examples:**
- Brand names (AWS, Netflix, Salesforce)
- Acronyms (API, ML, CI/CD)
- Technical terms (Kubernetes, PostgreSQL)
- Product names

**How to use:**
1. Create list of special words/phrases
2. Add pronunciation hints
3. Upload to Transcribe

```
After Custom Vocabulary:
Audio: "We use AWS Microservices"
Transcribe Output: "We use AWS Microservices" ✅
```

### **Method 2: Custom Language Models (For Context)**

#### **Problem:**
```
Context 1 (Bird watching): "I saw a crow service"
Context 2 (IT company): "I use a microservice"
```
**Same sound, different meaning based on context!**

#### **Solution: Train Custom Language Model**
**What it does:** Teaches Transcribe the **context** of your domain

**How it works:**
- Provide **domain-specific text data** (lots of it)
- Transcribe learns **word relationships** in your field
- Understands **context** to pick correct word

**Examples:**
- **Medical field**: Learns medical terminology context
- **Legal field**: Learns legal jargon context
- **IT field**: Learns "microservice" is one word, not "my crow service"

#### **Key Difference:**

| Custom Vocabulary | Custom Language Model |
|------------------|----------------------|
| **Teaches NEW words** | **Teaches CONTEXT** |
| "What is AWS?" | "When to use microservice vs my crow service" |
| Word-level | Sentence/context-level |
| Small word list | Large text corpus |

### **Best Practice:**
**Use BOTH together for highest accuracy!**
- Custom Vocabulary: Teaches the words (AWS, microservices)
- Custom Language Model: Teaches the context (IT domain)

## Toxicity Detection (Exam Important!)

### **What It Does:**
**Detects toxic/harmful content** in audio

### **How It Works:**
Uses **TWO types of analysis:**

#### **1. Speech Cues (Audio Analysis)**
- **Tone of voice** - Is person angry?
- **Pitch changes** - Aggressive speaking?
- **Volume** - Yelling or shouting?

#### **2. Text-Based Cues (Content Analysis)**
- **Profanity** - Curse words
- **Hate speech** - Discriminatory language
- **Threats** - Threatening language

**Power:** Combines BOTH audio tone + text content for better detection

### **Toxicity Categories:**
- **Sexual harassment**
- **Hate speech**
- **Threats**
- **Abuse**
- **Profanity**
- **Insult**
- **Graphic content**

### **Use Cases:**
- **Content moderation** for social platforms
- **Call center monitoring** for customer service
- **Compliance** for regulated industries
- **Safety monitoring** in online communities

## Simple Workflow

### **Basic Transcription:**
```
Audio File → Transcribe → Text Output
```

### **With Custom Vocabulary:**
```
Audio File + Custom Vocabulary → Transcribe → Accurate Text
```

### **With Custom Language Model:**
```
Audio File + Custom Language Model → Transcribe → Context-Aware Text
```

### **With Both (Best):**
```
Audio File + Custom Vocabulary + Custom Language Model → Transcribe → Highly Accurate Text
```

## Key Benefits
- **Fully managed** - No infrastructure to manage
- **Automatic** - No manual transcription needed
- **Fast** - Real-time or batch processing
- **Accurate** - Deep learning powered
- **Multilingual** - Supports many languages
- **Secure** - PII redaction built-in

## Exam Focus Points
- **ASR** = Automatic Speech Recognition
- **PII redaction** - Automatically removes sensitive info
- **Custom vocabularies** - For specific words/terms
- **Custom language models** - For domain context
- **Use BOTH** for highest accuracy
- **Toxicity detection** - Combines audio tone + text content
- **Toxicity categories** - Sexual harassment, hate speech, threats, abuse, profanity, insult, graphic
- **Use cases** - Customer service calls, closed captioning, searchable archives
