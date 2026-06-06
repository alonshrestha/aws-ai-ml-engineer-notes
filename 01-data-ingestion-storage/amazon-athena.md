# Amazon Athena

## Overview
Amazon Athena is a **serverless interactive query service** that allows you to query data in S3 using SQL. It provides a SQL interface for your data lake without needing to load data into a separate database.

## Key Features
- **Serverless** - No infrastructure to manage
- **SQL interface** for S3 data
- **No data movement** - Data stays in S3
- **Built on Presto** (originally, now evolved)
- **Pay-per-query** pricing model

## Supported Data Formats

### Human Readable:
- **CSV** (Comma Separated Values)
- **TSV** (Tab Separated Values) 
- **JSON** (JavaScript Object Notation)

### Binary/Optimized:
- **ORC** (Optimized Row Columnar) - Columnar & Splittable
- **Parquet** - Columnar & Splittable
- **Avro** - Splittable only

### Compression Support:
- Snappy
- Zlib
- LZ4
- Gzip

## Data Format Properties (Exam Important)

### Columnar Formats:
- **ORC** and **Parquet** - Better for column-specific queries
- Faster performance when querying specific columns

### Splittable Formats:
- **ORC**, **Parquet**, and **Avro** - Can be processed in parallel
- Better performance for large datasets

### Human Readable:
- **CSV**, **TSV**, **JSON** - Can open in text editor
- Easier to debug and understand

## Data Structure Support
- **Unstructured** data
- **Semi-structured** data  
- **Structured** data
- Can infer schema automatically or use provided schema

## Common Use Cases

### Log Analysis:
- **Web logs** querying (preferred over Elasticsearch)
- **CloudTrail logs** analysis
- **CloudFront logs** investigation
- **VPC flow logs** monitoring
- **ELB access logs** analysis

### Data Pipeline:
- **Staging data analysis** before loading into Redshift
- **Data exploration** before committing to data warehouse
- **Ad-hoc querying** of data lake contents

## Integration & Connectivity

### Database Interfaces:
- **ODBC** drivers
- **JDBC** drivers
- Can be treated like any relational database

### Notebook Integration:
- **Jupyter** notebooks
- **Zeppelin** notebooks
- **RStudio** integration

### Visualization:
- **Amazon QuickSight** integration
- Any BI tool supporting JDBC/ODBC

## Architecture Pattern
```
[Data in S3] → [Glue Crawler] → [Glue Data Catalog] → [Athena Query Engine] → [SQL Results] → [Visualization/Analysis Tools]
```

## AWS Glue Integration

### How It Works:
1. **Glue Crawler** scans S3 data and discovers schema
2. **Glue Data Catalog** stores table definitions and metadata
3. **Athena** automatically sees catalog tables and creates queryable tables
4. **Other services** can also use the same catalog (Redshift, EMR, Hive-compatible apps)

### Detailed Process:
- **Glue Crawler** populates the Glue Data Catalog for S3 data
- Extracts **columns and table definitions** from unstructured data
- Use **Glue Console** to refine definitions as needed
- **Athena automatically sees** anything in Glue Data Catalog
- **Creates tables automatically** for SQL querying

### Compatible Services:
- **RDS** - Can use Glue Data Catalog
- **Redshift** - Integration available
- **Redshift Spectrum** - Direct integration
- **EMR** - Full compatibility
- **Apache Hive Metastore** - Glue can act as Hive metastore

### Benefits:
- **Unified metadata repository** across multiple services
- **Automatic schema discovery** and maintenance
- **Schema versioning** handled automatically
- **Partition management** for better performance
- **SQL interface** on top of Glue structure

## Workgroups

### Purpose:
- **Organize users/teams** into separate groups
- **Control query access** and permissions
- **Track costs** by team or application
- **Isolate query history** between groups

### Key Features:
- **Data scan limits** - Prevent expensive queries per workgroup
- **Individual query history** - Each workgroup sees only its own queries
- **Custom IAM policies** - Fine-grained permissions per group
- **Encryption settings** - Per-workgroup security configuration
- **Integration** with IAM, CloudWatch, and SNS for alerts

### Use Cases:
- **Team isolation** - Separate access for different teams
- **Cost control** - Track and limit spending by group
- **Query limits** - Prevent wildly inefficient queries
- **Security boundaries** - Different encryption/access per group
- **Notification setup** - Alerts when limits are hit

## Cost Model

### Pricing:
- **$5 per terabyte scanned** (pay-as-you-go)
- **Successful queries** - Charged
- **Canceled queries** - Charged
- **Failed queries** - Free
- **DDL operations** (CREATE, ALTER, DROP) - Free

### Cost Optimization:
- **Columnar formats** (ORC, Parquet) - Save 30-90% by scanning only needed columns
- **Data partitioning** - Reduce scanned data by filtering partitions (by date, hour, etc.)
- **Compression** - Reduce data size and scan costs
- **Query optimization** - Write efficient queries to scan less data

### Why Columnar Saves Money:
- **Selective column reading** - Only scans required columns
- **Better performance** - Faster queries on subset of columns
- **Reduced data scanning** - Less data = lower costs
- **Examples**: ORC and Parquet formats

## Fine-Grained Access Control (Glue Data Catalog)

### What is Fine-Grained Access?
- **IAM-based database and table level security** for specific operations
- **NOT traditional fine-grained** (no row/column/cell level filtering)
- **Operation-level control** - What users can DO with databases/tables
- **Broader than data filters** - Controls structural operations

### What You CAN Control:
- **Database operations**: CREATE, ALTER, DROP databases
- **Table operations**: CREATE, DROP, REPAIR tables
- **Discovery operations**: SHOW DATABASES, SHOW TABLES
- **Metadata access**: GET partitions, GET table definitions

### What You CANNOT Control:
- **Row-level security** - Use Lake Formation for this
- **Column-level security** - Use Lake Formation for this
- **Cell-level security** - Use Lake Formation for this
- **Specific table versions** - Applies to all versions

### Minimum Required Permissions:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "athena:*",
        "glue:GetDatabase",
        "glue:GetTable",
        "glue:GetPartitions"
      ],
      "Resource": "*"
    }
  ]
}
```

### Example: Restrict DROP TABLE Operation
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "glue:GetPartition",
        "glue:GetPartitions", 
        "glue:GetTable",
        "glue:GetDatabase",
        "glue:DeleteTable",
        "glue:DeletePartition"
      ],
      "Resource": [
        "arn:aws:glue:region:account:catalog",
        "arn:aws:glue:region:account:database/specific_database",
        "arn:aws:glue:region:account:table/specific_database/specific_table"
      ]
    }
  ]
}
```

### Common Operations and Required Actions:

#### **CREATE TABLE:**
- `glue:CreateTable`
- `glue:GetDatabase`

#### **DROP TABLE:**
- `glue:DeleteTable`
- `glue:DeletePartition`
- `glue:GetTable`
- `glue:GetPartitions`
- `glue:GetDatabase`

#### **SHOW TABLES:**
- `glue:GetTables`
- `glue:GetDatabase`

#### **MSCK REPAIR TABLE:**
- `glue:CreatePartition`
- `glue:GetTable`
- `glue:GetPartitions`

### Key Points for Exam:
- **Database/table level** security only
- **Operation-based** restrictions (CREATE, DROP, ALTER, etc.)
- **IAM actions mapping** to SQL operations
- **Regional scope** - Policies needed per region
- **Not data-level filtering** - Use Lake Formation for row/column security

### Best Practices:
- **Principle of least privilege** - Grant only needed permissions
- **Resource-specific ARNs** - Don't use wildcards unnecessarily
- **Combine with workgroups** - Layer security controls
- **Document mappings** - Keep track of operation → IAM action mappings

## Security

### Access Control:
- **IAM policies** - AmazonAthenaFullAccess, AWSQuickSightAthenaAccess
- **S3 bucket policies** - Control data access
- **Access Control Lists (ACLs)** - Additional S3 access control
- **Cross-account access** - Via S3 bucket policies (Athena in one account can access S3 in another)

### Encryption:

#### Results Encryption (in S3 staging):
- **SSE-S3** - Server-side encryption with S3-managed keys
- **SSE-KMS** - Server-side encryption with KMS keys
- **CSE-KMS** - Client-side encryption with KMS keys

#### In-Transit:
- **TLS** - All traffic between Athena and S3

## Anti-Patterns (What NOT to use Athena for)

- **Highly formatted reports** - Use QuickSight instead
- **Visualization/dashboards** - Use QuickSight for charts and graphs
- **ETL operations** - Use Glue ETL or Apache Spark instead
- **Real-time analytics** - Use Kinesis Analytics instead

### Why These Are Anti-Patterns:
- **Athena = SQL query engine only** - Not for formatting or visualization
- **QuickSight = Visualization tool** - Better for charts, graphs, formatted reports
- **Glue ETL = Transform data** - Better for extract, transform, load operations
- **Separate tools for separate purposes** - Use right tool for the job

## Performance Optimization (Exam Important)

### Three Key Performance Rules:

#### 1. **Use Columnar Data Formats**
- **ORC or Parquet** - Best performance for Athena
- **Pre-process data** using Glue ETL or other tools
- **Convert existing data** to columnar format for better performance
- **Significant performance improvement** over row-based formats

#### 2. **File Size Optimization**
- **Small number of large files** > Large number of small files
- **Optimal**: Few large columnar files
- **Avoid**: Many tiny files (causes overhead)
- **Best practice**: Combine small files into larger ones

#### 3. **Use Partitions Strategically**
- **Partition by query patterns** - How you typically filter data
- **Common partitions**: Date, hour, region, category
- **Example**: If querying by date range, partition by date
- **Reduces data scanned** = Better performance + Lower costs

### Adding Partitions to Existing Data:

#### **MSCK REPAIR TABLE Command**
- **Purpose**: Add partition metadata to existing unpartitioned data
- **Use case**: When you partition existing S3 data after the fact
- **Command**: `MSCK REPAIR TABLE table_name`
- **Benefit**: Don't need to recreate entire dataset

#### **When to Use**:
- **Existing data** in S3 that wasn't originally partitioned
- **Performance improvement** needed on large datasets
- **Cost optimization** for frequently queried data

### Performance Best Practices Summary:
1. **Columnar formats** (ORC/Parquet) for better column scanning
2. **Fewer, larger files** instead of many small files
3. **Strategic partitioning** based on query patterns
4. **MSCK REPAIR TABLE** for adding partitions to existing data

## ACID Transaction Support (New Feature)

### What is ACID Support?
- **Strict transaction guarantees** for concurrent users
- **Row-level modifications** without conflicts
- **Powered by Apache Iceberg** under the hood
- **Concurrent user safety** - Multiple users can modify same data safely

### How to Enable:
```sql
CREATE TABLE my_table (
  id INT,
  name STRING
)
TABLE_TYPE = 'ICEBERG'
```

### Key Benefits:

#### **Concurrent Operations:**
- **Multiple users** can modify same row simultaneously
- **No custom record locking** needed
- **Automatic conflict resolution**
- **Safe concurrent writes/deletes**

#### **Time Travel Operations:**
- **Recover recently deleted data** using SELECT statements
- **Historical data access** for recovery purposes
- **Built-in versioning** from ACID transaction logs

### Compatibility:
- **Amazon EMR** - Full support
- **Apache Spark** - Native integration
- **Any Apache Iceberg** compatible tool
- **Cross-service consistency**

### Comparison with Lake Formation:

#### **Two Ways to Get ACID Support:**
1. **Lake Formation Governed Tables** + Athena queries
2. **Athena Iceberg Tables** (direct creation)

#### **Lake Formation vs Athena ACID:**
- **Lake Formation**: Auto-compaction included
- **Athena ACID**: Manual compaction required

### Maintenance - Table Compaction:

#### **Why Compaction is Needed:**
- **ACID overhead** builds up over time
- **Performance degradation** without maintenance
- **Storage bloat** from transaction logs

#### **Manual Compaction Command:**
```sql
OPTIMIZE TABLE my_table 
REWRITE DATA USING BIN_PACK 
WHERE catalog = 'my_catalog';
```

#### **When to Run:**
- **Periodically** (weekly/monthly depending on usage)
- **When performance degrades**
- **After heavy write operations**

### Exam Focus:
- **Table type = ICEBERG** to enable ACID
- **Two ways**: Lake Formation Governed Tables OR Athena Iceberg
- **Manual compaction** required for Athena ACID (vs auto in Lake Formation)
- **BIN_PACK command** for performance maintenance
- **Time travel** capability for data recovery

## CTAS (Create Table As Select)

### What is CTAS?
- **Creates new table** from query results
- **Similar to materialized views** in Redshift
- **Data format conversion** tool
- **Quick data transformation** method

### Primary Use Cases:

#### **Format Conversion:**
- **Convert to columnar formats** (Parquet, ORC) for better performance
- **Add compression** (Snappy, Gzip) to reduce storage costs
- **Optimize existing data** for Athena queries

#### **Data Subsetting:**
- **Create smaller tables** from large datasets
- **Filter and transform** data in one step
- **Reorganize data structure** for specific use cases

### Syntax Examples:

#### **Convert to Parquet with Compression:**
```sql
CREATE TABLE new_table
WITH (
  format = 'PARQUET',
  compression = 'SNAPPY'
)
AS SELECT * FROM old_table;
```

#### **Convert to ORC with Custom Location:**
```sql
CREATE TABLE my_orc_table
WITH (
  format = 'ORC',
  external_location = 's3://my-athena-results/orc-data/'
)
AS SELECT * FROM old_table;
```

### Key Benefits:
- **Performance optimization** - Convert to columnar formats
- **Cost reduction** - Better compression and scanning efficiency
- **Data transformation** - Quick and dirty ETL alternative
- **Storage flexibility** - Specify custom S3 locations
- **No separate ETL tools** - Use pure SQL for transformations

### Important Notes:
- **Creates physical data** in S3 (not just metadata)
- **Can specify custom S3 paths** for output
- **Useful for one-time transformations** or regular optimization
- **Alternative to complex ETL pipelines** for simple transformations

## Key Benefits
- **No ETL required** - Query data where it sits
- **Cost effective** - Pay only for queries run
- **Scalable** - Handles petabyte-scale datasets
- **Fast setup** - No infrastructure provisioning
- **Standard SQL** - Familiar query language
- **Serverless** - No servers to manage
- **Automatic integration** - Works seamlessly with Glue Data Catalog
- **CTAS support** - Easy data format conversion and optimization

## Additional Costs to Consider
- **Athena charges** - $5 per TB scanned
- **AWS Glue charges** - Separate billing for crawlers and catalog
- **S3 storage charges** - Data still stored in S3
- **Data transfer costs** - If applicable

## Exam Focus
- **Serverless** SQL querying of S3 data
- **Data format properties**: Columnar (ORC, Parquet) vs Splittable (ORC, Parquet, Avro) vs Human Readable (CSV, TSV, JSON)
- **No data movement** - queries data in place
- **Integration capabilities** with notebooks and BI tools
- **Log analysis** use cases (preferred over Elasticsearch)
- **JDBC/ODBC** connectivity for standard database integration