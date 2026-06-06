# ETL and ELT Deep Dive

## What is ETL/ELT?

**ETL:** Extract → Transform → Load (Data Warehouse approach)  
**ELT:** Extract → Load → Transform (Data Lake approach)

The letters mean the same thing, just in different order.

---

## E - Extract

**Definition:** Retrieving raw data from its source.

### Data Sources
- External databases
- CRM systems (like Salesforce)
- Flat files (log files)
- APIs
- Other data repositories

### Key Considerations

**1. Data Integrity**
- Ensure no data is missing or corrupted during extraction
- Handle failed API calls (retry policies)
- Monitor extraction process

**2. Velocity (Speed)**
- **Real-time** - Extract as data is created
- **Near real-time** - Within minutes
- **Batch** - Once a night or once a week

Choose based on your requirements and how the data will be queried.

---

## T - Transform

**Definition:** Converting raw data into a format suitable for your target warehouse/lake.

**When it happens:**
- **Data Warehouse (ETL)** - Transform BEFORE loading (upfront)
- **Data Lake (ELT)** - Transform AFTER loading (on-demand)

### Common Transformations

**1. Cleansing**
- Fix errors in data
- Remove duplicate data
- Handle missing data (impute or drop)

**2. Enrichment**
- Add additional data from other sources
- Combine multiple extractions together

**3. Format Changes**
- Convert date strings → datetime format
- Convert text → integers or binary
- One-hot encoding for ML (A technique to convert categorical text data into numerical format that machine learning algorithms can understand.)

**4. Aggregations/Computations**
- Calculate totals or averages
- Create summary statistics

**5. Encoding/Decoding**
- Decrypt or decode incoming data
- Unzip compressed files
- Convert to columnar format (Parquet, ORC)

**6. Missing Data Handling**
Options:
- Drop rows with null values
- Impute placeholder values
- Create rejection reports

Choice depends on your requirements.

---

## L - Load

**Definition:** Moving transformed data into the target data warehouse or repository.

### Loading Methods

**1. Batch Loading**
- Load all data at once
- Scheduled (nightly, weekly)
- Good for large volumes

**2. Streaming**
- Load in real-time as data becomes available
- Continuous flow
- Good for time-sensitive data

### Key Considerations

**1. Data Integrity**
- Ensure disk writes don't fail
- Handle backups and retries
- Don't drop data silently

**2. Volume and Velocity**
- How much data?
- How fast does it need to be available?
- What are analysis requirements?

---

## Pipeline Management

### Why We Need It
ETL/ELT involves many steps that need to:
- Happen in the right order
- Run on the right schedule
- Be automated and reliable

### AWS Services for Pipeline Management

**1. AWS Glue**
- Automatic ETL/ELT
- Event-driven (responds to data arrival)
- Built-in workflows

**2. Orchestration Services**
- **Amazon EventBridge** - Event-driven automation
- **Amazon MWAA** - Managed Apache Airflow
- **AWS Step Functions** - Workflow orchestration
- **AWS Lambda** - Serverless functions

---

## ETL vs ELT Summary

| Aspect | ETL (Warehouse) | ELT (Lake) |
|--------|----------------|------------|
| **Transform When** | Before loading | After loading |
| **Transform Where** | External ETL tool | Inside data store |
| **Best For** | Structured data | All data types |
| **Flexibility** | Less flexible | More flexible |
| **Speed** | Slower initial load | Faster initial load |

---

## Key Concepts to Remember

### The Three V's
- **Volume** - How much data?
- **Velocity** - How fast?
- **Variety** - What types?

These determine your ETL/ELT strategy.

### Data Integrity
Must be ensured at BOTH:
- **Extract phase** - Don't lose data getting it
- **Load phase** - Don't lose data writing it

### Requirements Drive Decisions
Always ask:
- How will data be queried?
- How fast does it need to be available?
- What's the data volume?
- What's the data quality requirement?

---

## Exam Tips

1. **ETL** = Transform BEFORE load (Data Warehouse)
2. **ELT** = Transform AFTER load (Data Lake)
3. Know the three phases: **Extract, Transform, Load**
4. **AWS Glue** is the primary ETL/ELT service
5. Orchestration services: **EventBridge, MWAA, Step Functions, Lambda**
6. Data integrity must be maintained during **extract AND load**
7. Choose batch vs streaming based on **velocity requirements**
8. Transform includes: cleansing, enrichment, format changes, aggregations
9. Missing data can be: dropped, imputed, or reported
10. Pipeline management = automation + orchestration
