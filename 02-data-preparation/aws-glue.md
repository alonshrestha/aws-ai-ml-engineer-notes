# AWS Glue

## Overview
AWS Glue is a **serverless** system for building table definitions and performing ETL (Extract, Transform, Load) operations automatically. It serves as the central metadata repository for data lakes and extracts structure from unstructured data.

## Key Features
- **Serverless** - No infrastructure to maintain
- **Automatic schema discovery** from unstructured data
- **Custom ETL jobs** using Apache Spark (fully managed)
- **Trigger-driven processing** (scheduled, on-demand, or event-based)

## Main Components

### 1. Glue Crawler & Data Catalog
- **Scans data in S3** and infers schema automatically
- **Populates Glue Data Catalog** with table definitions
- Can run **periodically or on-demand**
- Discovers:
  - Column names and data types
  - Table structure
  - Data location

### 2. ETL Jobs
- Uses **Apache Spark** under the hood (no cluster management needed)
- Processes data after schema discovery
- Fully managed execution

### 3. AWS Glue Studio (The Visual Builder)
**Visual interface for creating ETL workflows without coding**

#### Key Features:
- **Drag-and-drop workflow creator** (also called "Visual ETL")
- **No coding required** for most tasks
- **Parallel processing** - multiple paths can run simultaneously
- **Visual job dashboard** for monitoring

#### How It Works:
1. **Pick Data Source**: S3, Kinesis, databases, even third-party (Salesforce, QuickBooks)
2. **Add Transforms**: Filter, join, change schema, custom code
3. **Choose Destination**: S3, databases, other services

#### Benefits:
- Create complex workflows visually
- Supports partitioned data automatically
- Real-time job monitoring
- Works with competing platforms (Snowflake, BigQuery)

## Integration with Other Services
Glue enables SQL querying of unstructured S3 data through:
- **Amazon Athena**
- **Amazon Redshift**
- **Amazon EMR** (Hive)
- Other SQL databases

Data visualization possible with **Amazon QuickSight** after cataloging.

## S3 Partitioning Strategy

### Key Principle
Organize S3 data structure based on **primary query patterns** for optimal performance.

### Examples

#### Time-based Queries
```
s3://bucket/year/month/date/device/
```
Best for queries filtering by date ranges.

#### Device-based Queries  
```
s3://bucket/device/year/month/date/
```
Best for queries filtering by specific devices.

### Important Notes
- **Data remains in S3** - no duplication
- Glue provides the "glue" between unstructured data and structured analytics tools
- Partition strategy directly impacts query performance
- Plan data organization upfront based on expected query patterns

## Exam Focus
- Major component of AWS Big Data certification
- Understand how Glue connects different services
- Know partitioning best practices
- Recognize use cases for schema discovery and ETL
- Glue Studio may appear in future exams (newer feature)