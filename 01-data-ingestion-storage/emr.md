# Amazon EMR (Elastic MapReduce)

## What is EMR?
- Managed Hadoop framework running on EC2 instances
- Includes Hadoop, Spark, HBase, Presto, Flink, Hive
- EMR notebooks (like Jupyter) for data processing
- Used for processing massive datasets before machine learning

## Node Types
- **Master Node**: Manages cluster, coordinates tasks, monitors health
- **Core Nodes**: Run tasks and store data on HDFS (required for multi-node)
- **Task Nodes**: Only run tasks, no data storage (optional, good for spot instances)

## Cluster Types
- **Transient**: Auto-terminates after predefined steps complete
- **Long Running**: Manual termination, good for ad-hoc queries

## Storage Options
- **HDFS**: Default, fast but ephemeral (data lost when cluster terminates)
- **EMR FS**: Uses S3 as HDFS replacement, persistent storage
- **EBS**: Can back HDFS for additional storage

## Key Features
- Auto-provisions new nodes if core nodes fail
- Add/remove task nodes on-the-fly
- Resize core nodes while running
- Integrates with VPC, S3, CloudWatch, IAM, CloudTrail
- Charged hourly plus EC2 costs