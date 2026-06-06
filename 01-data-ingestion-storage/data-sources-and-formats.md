# Data Sources and Formats

## Data Sources

### 1. JDBC (Java Database Connectivity)
**What:** Interface for accessing external databases using Java.

**Pros:**
- ✅ Platform independent (runs on any device)
- ✅ Common standard for database access

**Cons:**
- ❌ Language dependent (requires Java)

**Use when:** Building extraction tools in Java.

---

### 2. ODBC (Open Database Connectivity)
**What:** Alternative database interface not tied to Java.

**Pros:**
- ✅ Language independent (works with any language)

**Cons:**
- ❌ Platform dependent (needs specific drivers)

**Use when:** Not using Java or need multi-language support.

**Note:** Most data extraction tools support both JDBC and ODBC.

---

### 3. Raw Log Files
**What:** Direct ingestion of log files (e.g., from S3).

**Use when:** Data is already in file format, no database needed.

---

### 4. APIs (Application Programming Interfaces)
**What:** Custom interfaces to external systems.

**Use when:** System provides API instead of database access.

---

### 5. Streams
**What:** Real-time data flows.

**Examples:**
- Apache Kafka
- Amazon Kinesis

**Use when:** Need real-time data ingestion.

---

## Data Formats

### CSV (Comma-Separated Values)

**What:** Text-based, human-readable tabular data.

**Structure:**
```
Name,Age,City
Alice,25,NYC
Bob,30,London
```

**Key Features:**
- Human readable
- Each line = one row
- Values separated by delimiter (comma, tab, pipe, etc.)

**Pros:**
- ✅ Easy to read and edit
- ✅ Works with almost any system
- ✅ Good for data interchange
- ✅ Simple format

**Cons:**
- ❌ Not efficient for large data
- ❌ No built-in schema
- ❌ Commas in data can cause issues

**Use Cases:**
- Small to medium datasets
- Importing/exporting from databases
- Excel spreadsheets
- Data interchange between systems

**Tools:** SQL databases, Excel, Pandas (Python), R, ETL tools

---

### JSON (JavaScript Object Notation)

**What:** Text-based, human-readable format using key-value pairs.

**Structure:**
```json
{
  "name": "Alice",
  "age": 25,
  "city": "NYC"
}
```

**Key Features:**
- Human readable
- Semi-structured (flexible schema)
- Supports nested data
- Different rows can have different fields

**Pros:**
- ✅ Flexible schema
- ✅ Supports nested structures
- ✅ Human readable
- ✅ Widely supported

**Cons:**
- ❌ Not efficient for large data
- ❌ More verbose than binary formats

**Use Cases:**
- Web server ↔ web client communication
- Configuration files
- Semi-structured data
- NoSQL databases
- RESTful APIs

**Tools:** Web browsers, JavaScript, Python, Java, MongoDB

---

### Avro

**What:** Binary format that stores data AND schema together.

**Key Features:**
- Binary (not human readable)
- Schema included with data
- Compact and efficient

**Pros:**
- ✅ Efficient storage (binary)
- ✅ Schema evolution (schema can change)
- ✅ Self-describing (schema included)
- ✅ Good for data transport

**Cons:**
- ❌ Not human readable
- ❌ Wastes space if schema doesn't change

**Use Cases:**
- Big data processing
- Real-time systems
- Schema might change over time
- Data transport between systems

**Tools:** Apache Kafka, Apache Spark, Apache Flink, Hadoop

---

### Parquet ⭐ (Very Important!)

**What:** Columnar storage format (stores by columns, not rows).

**How It Works:**
```
Row-based (CSV):
Row 1: Alice, 25, NYC
Row 2: Bob, 30, London

Column-based (Parquet):
Names: Alice, Bob
Ages: 25, 30
Cities: NYC, London
```

**Key Features:**
- Stores data by columns
- Binary format
- Highly compressed
- Optimized for analytics

**Pros:**
- ✅ Efficient compression
- ✅ Fast column queries
- ✅ Great for analytics
- ✅ Works on distributed systems

**Cons:**
- ❌ Not human readable
- ❌ Slower for full row reads

**Use Cases:**
- Large datasets with many columns
- Analytics (only need specific columns)
- Distributed systems
- Data warehouses

**Why Columnar?**
- Queries often need specific columns, not all data
- Columns have same data type → better compression
- Can distribute columns across servers

**Tools:** Hadoop, Apache Spark, Apache Hive, Apache Impala, **Redshift Spectrum** (AWS)

---

## Quick Comparison

| Format | Type | Human Readable | Best For | Size |
|--------|------|----------------|----------|------|
| **CSV** | Text | ✅ Yes | Small data, interchange | Large |
| **JSON** | Text | ✅ Yes | Semi-structured, APIs | Large |
| **Avro** | Binary | ❌ No | Big data, changing schema | Medium |
| **Parquet** | Binary | ❌ No | Analytics, many columns | Small |

---

## When to Use What?

### Choose CSV When:
- ✅ Small to medium data
- ✅ Need human readability
- ✅ Exchanging data between systems
- ✅ Working with Excel/spreadsheets

### Choose JSON When:
- ✅ Semi-structured data
- ✅ Web APIs
- ✅ Configuration files
- ✅ NoSQL databases
- ✅ Nested data structures

### Choose Avro When:
- ✅ Big data processing
- ✅ Schema might change
- ✅ Real-time streaming
- ✅ Need schema with data

### Choose Parquet When:
- ✅ Large datasets
- ✅ Analytics queries
- ✅ Many columns, query few
- ✅ Distributed processing
- ✅ Data warehouses

---

## Exam Tips

1. **JDBC** = Java-based, platform independent
2. **ODBC** = Language independent, platform dependent
3. **CSV** = Human readable, text-based, small data
4. **JSON** = Semi-structured, key-value pairs, flexible schema
5. **Avro** = Binary, includes schema, good for changing schemas
6. **Parquet** = Columnar, optimized for analytics, very important for AWS!
7. **Parquet + Redshift Spectrum** = Common AWS pattern
8. Columnar storage = stores by columns, not rows
9. Binary formats (Avro, Parquet) = more efficient but not human readable
10. Text formats (CSV, JSON) = human readable but less efficient
