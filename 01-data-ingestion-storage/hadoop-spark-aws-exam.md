# Hadoop and Spark - AWS Exam Focus

## Hadoop Core Components

### 1. Hadoop Common/Core
- Base libraries and utilities for all Hadoop modules
- File system abstraction and OS-level utilities

### 2. HDFS (Hadoop Distributed File System)
- **Distributed storage** across cluster nodes
- **Data replication** (default 3 copies) for fault tolerance
- **Block-based storage** (default 128MB blocks)
- Data persists only during cluster lifetime

### 3. YARN (Yet Another Resource Negotiator)
- **Resource manager** for cluster resources
- Enables multiple processing frameworks beyond MapReduce
- **Node managers** on each cluster node

### 4. MapReduce
- **Batch processing framework** from Google's original paper
- **Map phase**: Transform/filter data into key-value pairs
- **Reduce phase**: Aggregate intermediate results
- **Slower** due to disk-based operations

## Apache Spark

### Why Spark > MapReduce:
- **100x faster** with in-memory processing
- **DAG (Directed Acyclic Graph)** execution engine
- **Smart dependency management** and query optimization
- **Multi-language support**: Java, Scala, Python, R

### Spark Architecture:
- **Driver Program**: Contains SparkContext, coordinates execution
- **Cluster Manager**: YARN, Spark Standalone, or Mesos
- **Executors**: Run on worker nodes, execute tasks and cache data
- **Tasks**: Units of work sent to executors

### Core Spark Components:

#### Spark Core
- **RDD (Resilient Distributed Datasets)**: Fault-tolerant data structures
- **Lazy evaluation**: Operations not executed until action called
- **In-memory caching**: Key performance advantage
- **Partitioning**: Data distribution across nodes

#### Spark SQL
- **DataFrames/Datasets**: Higher-level APIs than RDDs
- **Catalyst optimizer**: Cost-based query optimization
- **Columnar storage** support (Parquet, ORC)
- **SQL interface**: Query data using standard SQL

#### Spark Streaming
- **Micro-batch processing**: Real-time data in small batches
- **DStreams**: Discretized streams of data
- **Integration**: Kafka, Kinesis, HDFS, TCP sockets
- **Same code**: Batch and streaming analytics

#### MLlib
- **Distributed ML algorithms**: Classification, regression, clustering
- **Scalable implementations**: Logistic regression, Naive Bayes, K-means
- **Pipelines**: ML workflow management
- **Feature transformation**: Distributed feature engineering

#### GraphX
- **Graph processing**: Social networks, recommendation systems
- **ETL and analysis**: Graph data structures at scale
- **Iterative algorithms**: PageRank, connected components

## AWS EMR Integration

### EMR (Elastic MapReduce)
- **Managed Hadoop/Spark** service
- **Auto-scaling**: Dynamic cluster resizing
- **Spot instances**: Cost optimization
- **Multiple frameworks**: Hadoop, Spark, Hive, HBase, Presto

### Key AWS Integrations:
- **S3**: Primary data storage, input/output for jobs
- **Kinesis**: Real-time data streaming with Spark Streaming
- **DynamoDB**: NoSQL data source/sink
- **Redshift**: Data warehouse integration
- **Glue**: ETL service, metadata catalog

### Zeppelin Notebooks
- **Interactive development**: Browser-based notebooks
- **Visualization**: Charts and graphs with matplotlib/seaborn
- **Multi-language**: SQL, Scala, Python support
- **Data exploration**: Familiar data science environment

## Exam Key Points

### Performance Concepts:
- **In-memory vs disk**: Spark's main advantage over MapReduce
- **Broadcast variables**: Efficiently share read-only data
- **Accumulators**: Distributed counters and sums
- **Partitioning strategy**: Affects performance significantly

### Use Case Selection:
- **Batch ETL**: Hadoop MapReduce or Spark
- **Real-time analytics**: Spark Streaming + Kinesis
- **Machine learning**: Spark MLlib for distributed training
- **Interactive queries**: Spark SQL with DataFrames
- **Graph analysis**: GraphX for network data

### Architecture Decisions:
- **HDFS**: Temporary storage during processing
- **S3**: Persistent data lake storage
- **EMR**: Managed clusters vs self-managed
- **Spot instances**: Cost optimization for fault-tolerant workloads

### Integration Patterns:
- **Data ingestion**: Kinesis → Spark Streaming → S3
- **Batch processing**: S3 → EMR Spark → Redshift
- **ML pipeline**: S3 → Spark MLlib → SageMaker
- **Real-time**: Kinesis → Spark → DynamoDB