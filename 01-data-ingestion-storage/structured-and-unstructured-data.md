# Data Types: Structured, Unstructured, and Semi-Structured

## Overview

Understanding the three types of data is fundamental for data engineering and AWS certification exams. Each type has different characteristics and requires different handling approaches.

---

## 1. Structured Data

**Definition:** Data that is organized in a defined manner with a clear schema, typically found in relational databases.

### Characteristics
- **Easily queryable** - Can use SQL queries directly
- **Organized** - Data is in rows and columns
- **Consistent structure** - No surprises, predictable format
- **Defined data types** - Each column has a specific type (string, integer, date, etc.)

### Examples
- **Database tables** - Oracle, MySQL, PostgreSQL, Amazon Redshift
- **CSV files** - With consistent columns and data types
- **Excel spreadsheets** - Well-organized with defined rows and columns

### Key Point
You already know what columns exist, their names, data types, and how they relate to each other.

---

## 2. Unstructured Data

**Definition:** Data that has no predefined structure or schema. It's raw data that needs processing before it can be queried.

### Characteristics
- **Cannot query directly** - Needs pre-processing first
- **No fixed format** - Can come in any format
- **Requires indexing** - Must extract meaning and structure before use
- **Needs metadata extraction** - Information about the data must be derived

### Examples
- **Text files** - Raw text with no fixed format (Wikipedia articles, Reddit posts, books)
- **Media files** - Videos, audio files, images
- **Documents** - Emails, Word documents

### Key Point
You need to extract structure first (like transcripts, metadata, topics, creation dates) before you can search or analyze this data.

---

## 3. Semi-Structured Data

**Definition:** Data that sits between structured and unstructured. It has some level of structure through tags, hierarchies, or patterns, but it's not as rigid as structured data.

### Characteristics
- **Flexible** - Can contain different types of information
- **Tagged or categorized** - Has some organizational elements
- **Inconsistent schema** - Structure exists but may vary throughout the document
- **Requires parsing** - Need to extract the structure that's embedded within

### Examples
- **JSON files** - Can have varying schemas within the same document
- **XML files** - Tagged data with flexible structure
- **Log files** - Apache logs, server logs (most common in data engineering)
  - May have missing data
  - Different lines may have different formats
  - Requires parsing to understand each line
- **Email headers** - Has structured parts (date, subject) and unstructured parts (body)

### Key Point
The structure is in there somewhere, but it might not be consistent throughout. You need to tease it out.

---

## Quick Comparison

| Type | Structure | Queryability | Examples |
|------|-----------|--------------|----------|
| **Structured** | Fully defined schema | Directly queryable with SQL | Database tables, CSV files |
| **Unstructured** | No schema | Needs pre-processing | Videos, images, raw text |
| **Semi-Structured** | Partial/flexible schema | Needs parsing first | JSON, XML, log files |

---

## Why This Matters for AWS Exams

- Different AWS services handle different data types
- **Structured data** → Amazon RDS, Redshift, Aurora
- **Unstructured data** → Amazon S3, Amazon Rekognition (images), Amazon Transcribe (audio)
- **Semi-structured data** → Amazon Athena (query JSON/logs), AWS Glue (ETL), Amazon OpenSearch

Understanding these distinctions helps you choose the right AWS service for your data engineering tasks.
