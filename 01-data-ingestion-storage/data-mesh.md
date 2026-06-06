# Data Mesh

## What is Data Mesh?

**Definition:** A data management approach where individual teams own and manage their own data, offering it as products to other teams in the organization.

**Key Point:** It's about **organization and governance**, NOT technology.

---

## Core Concept

Instead of one central team managing all data:
- **Individual teams own their data** - The team that knows the data best manages it
- **Data as a product** - Teams offer their data to others in the organization
- **Decentralized ownership** - Each domain controls their own data
- **Central governance** - Shared standards and principles across all teams

---

## Key Components

### 1. Data Domains
- Specific teams that own specific data
- Responsible for maintaining data quality and security
- Expose data as "data products" to other teams

### 2. Data Products
- Clean, ready-to-use data offered by domains
- Other teams use these instead of raw data
- Easier access without knowing all the details

### 3. Use Cases
- Teams with analysis needs connect to multiple data products
- Don't need to access raw data directly
- Can combine data from different domains easily

### 4. Federated Governance
- Central standards for security and access control
- Each domain applies these standards to their data
- Consistency across the organization

### 5. Self-Service Infrastructure
- Domains aren't left to build everything from scratch
- Central tools and platforms provided
- Teams can focus on their data, not infrastructure

---

## Simple Example

```
Marketing Team (Domain)
└─ Owns: Customer behavior data
   └─ Offers: Customer insights product
   
Sales Team (Domain)
└─ Owns: Transaction data
   └─ Offers: Sales metrics product

Analytics Team (Use Case)
└─ Needs: Customer + Sales analysis
   └─ Uses: Both data products (no raw data access needed)
```

---

## Why Amazon Loves It

- **Amazon's internal structure** - How Amazon has always operated
- **AWS fits perfectly** - Good solution for implementing data mesh
- **AWS Services that help:**
  - **Lake Formation** - Manage data storage and permissions
  - **AWS Glue** - Centralized data catalog (see what's available)
  - **S3** - Store data products
  - **IAM** - Control access across domains

---

## Key Principles

1. **Domain ownership** - Teams own their data
2. **Data as a product** - Treat data like a product for others to use
3. **Self-service platform** - Infrastructure provided centrally
4. **Federated governance** - Central standards, local implementation

---

## Data Mesh vs Traditional

| Traditional | Data Mesh |
|-------------|-----------|
| Central data team owns everything | Individual teams own their data |
| One big data warehouse/lake | Multiple data domains |
| Users request data from central team | Users access data products directly |
| Bottleneck at central team | Decentralized and scalable |

---

## Important Notes

- **Not a technology** - It's an organizational approach
- **Not a specific architecture** - Can use various tools (AWS, others)
- **Also called:** Domain-based data management
- **Focus:** Who owns data, how it's accessed, how it's governed

---

## Exam Tips

1. Data mesh = **organizational approach**, not a technology
2. **Individual teams own their data** (decentralized)
3. **Central governance** with shared standards
4. Teams offer **data products** to others
5. AWS services like **Lake Formation and Glue** can support data mesh
6. Know the term "**domain-based data management**"
7. Emphasizes **self-service** and **data as a product**
