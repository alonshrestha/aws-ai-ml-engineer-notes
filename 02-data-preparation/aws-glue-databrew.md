# AWS Glue DataBrew

## Overview
AWS Glue DataBrew is a **visual data preparation tool** for pre-processing large datasets without coding. It focuses on the "Transform" part of ETL operations through a user-friendly interface.

## Key Features
- **Visual UI** for data transformation (no coding required)
- **250+ ready-made transformations** available
- **Recipe-based approach** - save and reuse transformation sequences
- **Multiple data sources** supported
- **S3 output** for processed data

## Data Sources & Destinations

### Input Sources:
- **S3** buckets
- **Data warehouses** (Redshift, Snowflake)
- **Any database**
- **Custom SQL** from Redshift and Snowflake

### Output:
- **S3 only** - all transformed data goes to S3

## Core Concepts

### Recipes
- **Collections of transformations** applied in sequence
- **Reusable** across different projects and datasets
- **Saved as jobs** within larger projects

### Recipe Actions
- Individual transformation steps within recipes
- 250+ predefined actions available
- Can be combined in any order

## Example Transformation: Nest to Map

**Purpose**: Convert selected columns into key-value pairs (JSON format)

**Parameters**:
```json
{
  "sourceColumns": ["age", "weight_kilogram", "height_centimeter"],
  "columnName": "person_data",
  "removeSourceColumns": true
}
```

**Result**:
- Original columns: `age=53, weight_kilogram=100, height_centimeter=180`
- New column: `person_data={"age":53, "weight_kilogram":100, "height_centimeter":180}`
- Original columns deleted (if removeSourceColumns=true)

## Additional Features

### Data Quality Rules
- Built-in data validation during transformation
- Ensures data integrity throughout the process

### Custom SQL Integration
- Create datasets using custom SQL queries
- Works with Redshift and Snowflake

## Security & Compliance

### Encryption:
- **KMS integration** (Customer Master Keys only)
- **SSL in transit** for all data movement

### Access Control:
- **IAM integration** for user permissions
- Role-based access to projects and datasets

### Monitoring:
- **CloudWatch** integration for metrics and alarms
- **CloudTrail** for governance and audit logging

## Use Cases
- **Data cleaning** before analysis
- **Format standardization** across datasets
- **Column transformations** (splitting, combining, converting)
- **Data type conversions**
- **Aggregations and calculations**

## Workflow Example
```
[Raw Data in S3/DB] → [DataBrew Recipes] → [Transformed Data in S3] → [Analytics Tools]
```

## Exam Focus
- **Visual data preparation** tool (key differentiator)
- **250+ transformations** available
- **Recipe-based approach** for reusability
- **S3-only output** (important limitation)
- **Integration with Redshift/Snowflake** for custom SQL
- **Security features** (KMS, IAM, CloudWatch/CloudTrail)