# AWS Glue Data Quality

## Overview
AWS Glue Data Quality is a **quality inspection step** that can be injected into Glue jobs to automatically evaluate incoming data quality and take action when rules are violated.

## Key Features
- **Automatic rule creation** - Analyzes good data to infer quality rules
- **Manual rule creation** - Using Data Quality Definition Language (DQDL)
- **Integration with ETL jobs** - Add as transformation step
- **Flexible responses** - Fail job or log to CloudWatch

## How It Works
1. **Analyze source data** to understand normal patterns
2. **Create quality rules** (row counts, column completeness, value ranges)
3. **Integrate into Glue jobs** as quality check step
4. **Take action** when rules fail (stop job or alert)

## Rule Creation Methods

### Automatic Recommendations
- Tell Glue Data Quality to analyze your known good data
- Automatically infers rules based on patterns
- Creates DQDL rules for expected ranges, standard deviations, etc.

### Manual Rules (DQDL)
Data Quality Definition Language examples:
- **Row count validation**: `RowCount between 1000 and 5000`
- **Column completeness**: `IsComplete "customer_id"`
- **Value length**: `ColumnLength "description" between 10 and 100`
- **Standard deviation**: `StandardDeviation "price" < 50`
- **Data type validation**: `DataType "date_field" = "DATE"`

## Response Options

### Fail Job
- Stops entire ETL pipeline when quality rules fail
- Good for critical data quality requirements
- Risk: False positives from tight thresholds

### Log to CloudWatch
- Reports quality issues without stopping job
- Allows manual review and action
- Better for monitoring and alerting

## Integration Example
```
[Data Source] → [Transform] → [Data Quality Check] → [Output]
```
Data Quality step can be inserted anywhere in the ETL pipeline.

## Benefits
- **Prevents bad data** from corrupting downstream analysis
- **Automatic threshold detection** from historical data
- **CloudWatch integration** for monitoring and alerting
- **Customizable failure responses** based on business needs

## Common Use Cases
- Validate expected row counts after data ingestion
- Check for missing values in critical columns
- Monitor data drift in statistical properties
- Ensure data format consistency
- Detect outliers that could skew analysis

## Exam Notes
- Recent feature (2024+) - likely to appear on exams
- Understand integration with Glue ETL jobs
- Know difference between automatic and manual rule creation
- Understand DQDL syntax basics
- Know response options (fail vs. log)