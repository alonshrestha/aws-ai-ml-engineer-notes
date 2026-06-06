# AWS Kinesis: Data Streams vs Firehose

## The Simple Difference

**Kinesis Data Streams:** You control everything, you write the consumer code.

**Kinesis Firehose:** Automatic delivery to destinations, no code needed.

---

## Visual Comparison

```
DATA STREAMS (Manual):
Producer → Data Stream → YOU write code → Destination
                         (Lambda/EC2/Flink)

FIREHOSE (Automatic):
Producer → Firehose → Automatic delivery → S3/Redshift/Elasticsearch
```

---

## Key Differences Table

| Feature | Data Streams | Firehose |
|---------|-------------|----------|
| **You write consumer code?** | ✅ Yes | ❌ No (automatic) |
| **Real-time?** | ✅ Yes (milliseconds) | ⚠️ Near real-time (60+ seconds) |
| **Destinations** | Anywhere (you code it) | Only S3, Redshift, Elasticsearch, HTTP |
| **Complexity** | More complex | Simpler |
| **Use case** | Custom processing | Just save data |

---

## When to Use Each

### Use **Data Streams** when:
- Need real-time processing (< 1 second)
- Need custom logic (filtering, transformations)
- Multiple consumers reading same data
- Building complex streaming apps

**Example:** Real-time fraud detection
```
Transactions → Data Stream → Your Lambda → Block if fraud
```

### Use **Firehose** when:
- Just want to save data to S3/Redshift
- Don't need real-time (60 seconds delay is OK)
- Don't want to write consumer code
- Simple ETL (extract, transform, load)

**Example:** Save logs to S3
```
App logs → Firehose → Automatically saved to S3
```

---

## Memory Trick

**Data Streams** = **Stream** of data you **process yourself**
- Think: "I need to **stream** and **code** my own logic"

**Firehose** = **Fire** and forget to storage
- Think: "Just **fire** it to S3, I don't care how"

---

## Common Exam Scenario

**Question:** "You need to save clickstream data to S3 for later analysis. What's the simplest solution?"

**Answer:** Firehose (automatic delivery to S3, no code needed)

**Question:** "You need to process transactions in real-time and send alerts within 200ms. What do you use?"

**Answer:** Data Streams (real-time, you write the alert logic)

---

## Quick Decision Tree

```
Do you need to write custom processing code?
├─ YES → Data Streams
└─ NO → Is your destination S3/Redshift/Elasticsearch?
    ├─ YES → Firehose
    └─ NO → Data Streams
```

---

## Apache Flink & AWS MSF

### Apache Flink
**What it is:** A framework for processing streaming data in real-time.

**Simple analogy:** It's like a factory assembly line that processes data as it flows through.

**Example use case:**
```
Kinesis Stream → Flink → Process data → Save to database
(raw clicks)    (filter, (calculate    (store results)
                 aggregate) click rates)
```

### AWS MSF (Managed Service for Apache Flink)
**What it is:** AWS runs Flink for you - you don't manage servers.

**Why use it:**
- ❌ **Without MSF:** You install Flink, manage servers, handle scaling, monitor health
- ✅ **With MSF:** AWS does all that, you just write the data processing code

---

## Kinesis Troubleshooting Quick Reference

### Producer Problems (Sending Data)

**Writes too slow:**
- Hit service limits → Check throughput exceptions
- Hot shards → Use better partition keys
- Small batches → Batch records with Put Records API
- 500/503 errors → Add retry logic
- Connection errors → Check network/VPC
- Timeouts → Increase timeout settings
- Throttling → Check hot shards, use exponential backoff

### Consumer Problems (Reading Data)

**Reading issues:**
- Skipped records → Check for unhandled exceptions
- Duplicate processing → Adjust failover time
- Reads too slow → Add more shards
- Lambda not invoked → Check IAM permissions
- Lambda timeout → Increase max execution time
- Throttling errors → Add shards, use enhanced fan-out
- High latency → Add shards, increase retention period

### Key Patterns
1. **Throttling/slow** → Hot shards or need more shards
2. **500 errors** → Add retry logic
3. **Connection issues** → Check network/VPC/permissions
4. **Enhanced fan-out** → Helps with read throughput

---

## Batching Explained

**Problem:** Sending data one record at a time is slow.

**Solution:** Group multiple records together and send at once.

**Bad - One at a time:**
```python
# Slow - 100 API calls
for record in records:
    kinesis.put_record(
        StreamName='my-stream',
        Data=record,
        PartitionKey='key'
    )
```

**Good - Batching:**
```python
# Fast - 1 API call
kinesis.put_records(
    StreamName='my-stream',
    Records=[
        {'Data': record1, 'PartitionKey': 'key'},
        {'Data': record2, 'PartitionKey': 'key'},
        {'Data': record3, 'PartitionKey': 'key'},
        # ... up to 500 records
    ]
)
```

**Why faster:**
- Fewer network calls (1 instead of 100)
- Less overhead (authentication once)
- Better throughput
