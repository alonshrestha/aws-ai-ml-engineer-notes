# AWS AI/ML Engineer Associate — Study Notes

Personal study notes I made while preparing for the **AWS Certified Machine Learning Engineer – Associate** and **AWS AI Practitioner** exams.

I passed both exams using these notes.

Each topic includes exam tips to help you focus on what matters most. They are not official AWS documentation.

## How to Use

Start with the [ML Pipeline Map](00-overview/ml-pipeline-map.md). It shows how every topic fits into the end-to-end ML workflow.

## Structure

| Folder | Topics |
|--------|--------|
| [00-overview](00-overview/) | ML pipeline map, design principles |
| [01-data-ingestion-storage](01-data-ingestion-storage/) | Data types, S3, Kinesis, EMR, Athena |
| [02-data-preparation](02-data-preparation/) | Feature engineering, ETL, Glue, Data Wrangler |
| [03-algorithms](03-algorithms/) | SageMaker built-in algorithms (classification, CV, NLP, clustering, time series) |
| [04-training-tuning](04-training-tuning/) | Neural networks, hyperparameters, distributed training, EC2 instances |
| [05-evaluation](05-evaluation/) | Metrics, Clarify, Debugger, Ground Truth |
| [06-deployment-mlops](06-deployment-mlops/) | Endpoints, inference options, model monitor, pipelines |
| [07-generative-ai](07-generative-ai/) | Transformers, GPT, LLMs, RAG, embeddings |
| [08-amazon-bedrock](08-amazon-bedrock/) | Bedrock agents, guardrails, fine-tuning, knowledge bases |
| [09-responsible-ai](09-responsible-ai/) | AI governance, fairness, compliance |
| [10-sagemaker-platform](10-sagemaker-platform/) | SageMaker overview, domains, workflows |

## Exam Info

- **AWS Certified Machine Learning Engineer – Associate** (MLA-C01)
- **AWS Certified AI Practitioner** (AIF-C01)

## Topics Not Covered Here

These topics are also important for the exam but I already had prior knowledge so I didn't make notes:

- **Docker & Containers** — ECR, ECS/EKS for model serving, custom SageMaker containers
- **AWS KMS & Encryption** — KMS keys for S3/SageMaker/Bedrock, envelope encryption, at-rest vs in-transit
- **S3 Security & Access Control** — bucket policies, VPC endpoints, S3 access points, encryption options (SSE-S3, SSE-KMS, CSE)
- **IAM for ML** — SageMaker execution roles, least-privilege policies, cross-account access
- **VPC & Networking** — running SageMaker in a VPC, PrivateLink, NAT gateways for training jobs

## Attribution

These notes were written while studying the following course:

- **AWS Certified Machine Learning Engineer Associate — Hands On** by Stéphane Maarek & Frank Kane (Sundog Education) on Udemy

Apart from these notes, I also practiced with the test questions provided by the same courses on Udemy.

All AWS service descriptions and technical details are based on publicly available [AWS documentation](https://docs.aws.amazon.com/).