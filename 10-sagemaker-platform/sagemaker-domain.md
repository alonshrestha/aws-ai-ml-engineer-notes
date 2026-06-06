# SageMaker Domain - Organizational Container

## What is a SageMaker Domain?

**SageMaker Domain** = The organizational container for all your SageMaker work. Think of it as your "SageMaker workspace" that holds everything.

**Key concept:** Before you can use SageMaker Studio, notebooks, or any modern SageMaker features, you MUST create a domain first.

**Simple analogy:** Domain is like an office building. Users are employees. Each employee has their own desk (private space) and access to shared conference rooms (shared spaces).

## Domain Components

```
┌─────────────────────────────────────────────────────┐
│ SAGEMAKER DOMAIN                                    │
│ (The umbrella for everything)                       │
├─────────────────────────────────────────────────────┤
│                                                     │
│ ┌─────────────────────────────────────────────┐   │
│ │ EFS VOLUME (Shared Storage)                 │   │
│ │ - Managed by SageMaker                      │   │
│ │ - Carved up for users and shared spaces     │   │
│ └─────────────────────────────────────────────┘   │
│                                                     │
│ ┌─────────────────────────────────────────────┐   │
│ │ AUTHORIZED USERS                            │   │
│ │ - List of who can access this domain        │   │
│ │ - IAM-based authentication                  │   │
│ └─────────────────────────────────────────────┘   │
│                                                     │
│ ┌─────────────────────────────────────────────┐   │
│ │ CONFIGURATION                               │   │
│ │ - VPC settings                              │   │
│ │ - Security policies                         │   │
│ │ - Encryption settings                       │   │
│ └─────────────────────────────────────────────┘   │
│                                                     │
│ ┌─────────────────────────────────────────────┐   │
│ │ USER PROFILES                               │   │
│ │ - Individual users                          │   │
│ │ - Each has private EFS directory            │   │
│ │ - Each can launch SageMaker Studio          │   │
│ └─────────────────────────────────────────────┘   │
│                                                     │
│ ┌─────────────────────────────────────────────┐   │
│ │ SHARED SPACES                               │   │
│ │ - Shared EFS directories                    │   │
│ │ - Collaborative workspaces                  │   │
│ │ - Shared notebooks and data                 │   │
│ └─────────────────────────────────────────────┘   │
│                                                     │
└─────────────────────────────────────────────────────┘
```

## The EFS Volume (Storage Layer)

**What:** Amazon EFS (Elastic File System) - shared network file storage

**Purpose:** Store notebooks, data, models, and other files

**How it's organized:**

```
EFS VOLUME (Managed by SageMaker)
├── User1/                    # Private directory for User 1
│   ├── notebooks/
│   ├── data/
│   └── models/
├── User2/                    # Private directory for User 2
│   ├── notebooks/
│   ├── data/
│   └── models/
├── User3/                    # Private directory for User 3
│   ├── notebooks/
│   └── experiments/
└── Shared/                   # Shared space for all users
    ├── team-notebooks/
    ├── common-datasets/
    └── shared-models/
```

**Key points:**
- One EFS volume per domain
- Automatically carved up for each user
- Private directories for each user profile
- Shared directories for collaboration
- Persistent storage (doesn't disappear when you stop working)

## User Profiles

**What:** Represents an individual person using SageMaker

**What each user gets:**

```
USER PROFILE (e.g., "data-scientist-alice")
├── Private EFS Directory
│   └── /home/alice/
│       ├── notebooks/
│       ├── experiments/
│       └── data/
├── Personal Applications
│   ├── SageMaker Studio instance
│   ├── Jupyter notebooks
│   └── Code editors
└── Access to Shared Resources
    ├── Shared spaces
    ├── Team notebooks
    └── Common datasets
```

**Example:**
- Alice (Data Scientist) - User Profile 1
- Bob (ML Engineer) - User Profile 2
- Carol (Data Analyst) - User Profile 3

Each has their own workspace but can collaborate in shared spaces.

## Shared Spaces

**What:** Collaborative workspaces where multiple users can work together

**Use cases:**
- Team projects
- Shared notebooks
- Common datasets
- Collaborative experiments

**Example:**

```
SHARED SPACE: "Customer Churn Project"
├── Shared EFS Directory
│   ├── project-notebooks/
│   │   ├── data-exploration.ipynb (Alice's work)
│   │   ├── model-training.ipynb (Bob's work)
│   │   └── evaluation.ipynb (Carol's work)
│   ├── datasets/
│   │   └── customer-data.csv
│   └── models/
│       └── churn-model-v1.tar.gz
└── Communal IDE
    └── SageMaker Studio (accessible by all team members)
```

## VPC Configuration (Network Layer)

### Default Configuration (Two VPCs)

```
┌─────────────────────────────────────────────────────┐
│ SAGEMAKER DOMAIN                                    │
├─────────────────────────────────────────────────────┤
│                                                     │
│  ┌──────────────────┐      ┌──────────────────┐   │
│  │ SAGEMAKER VPC    │      │ YOUR VPC         │   │
│  │ (AWS Managed)    │      │ (You Configure)  │   │
│  │                  │      │                  │   │
│  │ For:             │      │ For:             │   │
│  │ - Internet       │      │ - Private EFS    │   │
│  │   access         │      │   access         │   │
│  │ - Public data    │      │ - Encrypted      │   │
│  │ - External APIs  │      │   traffic        │   │
│  └────────┬─────────┘      └────────┬─────────┘   │
│           │                         │             │
│           └─────────┬───────────────┘             │
│                     │                             │
│              ┌──────▼──────┐                      │
│              │ EFS VOLUME  │                      │
│              │ (Your Data) │                      │
│              └─────────────┘                      │
│                                                     │
└─────────────────────────────────────────────────────┘

Traffic Flow:
- Internet traffic → SageMaker VPC (AWS managed)
- Private data → Your VPC (encrypted)
```

**What you need to configure:**
- Your VPC ID
- Subnets (ideally all of them for high availability)
- Security groups (firewall rules)

### VPC-Only Mode (More Secure)

```
┌─────────────────────────────────────────────────────┐
│ SAGEMAKER DOMAIN (VPC-Only Mode)                    │
├─────────────────────────────────────────────────────┤
│                                                     │
│              ┌──────────────────┐                   │
│              │ YOUR VPC         │                   │
│              │ (You Control)    │                   │
│              │                  │                   │
│              │ ALL traffic goes │                   │
│              │ through here     │                   │
│              │                  │                   │
│              │ - Private EFS    │                   │
│              │ - No internet    │                   │
│              │ - Full control   │                   │
│              └────────┬─────────┘                   │
│                       │                             │
│                ┌──────▼──────┐                      │
│                │ EFS VOLUME  │                      │
│                │ (Your Data) │                      │
│                └─────────────┘                      │
│                                                     │
└─────────────────────────────────────────────────────┘

Benefits:
✅ Complete network control
✅ No internet access (more secure)
✅ Compliance-friendly
✅ All traffic through your VPC
```

**When to use VPC-Only:**
- High security requirements
- Compliance needs (HIPAA, PCI-DSS)
- No need for internet access
- Want full network control

## Domain Hierarchy

```
┌─────────────────────────────────────────────────────┐
│ AWS ACCOUNT                                         │
│                                                     │
│  ┌───────────────────────────────────────────────┐ │
│  │ SAGEMAKER DOMAIN                              │ │
│  │                                               │ │
│  │  ┌─────────────────────────────────────────┐ │ │
│  │  │ USER PROFILE: Alice                     │ │ │
│  │  │ ├── Private EFS: /home/alice/           │ │ │
│  │  │ ├── SageMaker Studio                    │ │ │
│  │  │ └── Jupyter Notebooks                   │ │ │
│  │  └─────────────────────────────────────────┘ │ │
│  │                                               │ │
│  │  ┌─────────────────────────────────────────┐ │ │
│  │  │ USER PROFILE: Bob                       │ │ │
│  │  │ ├── Private EFS: /home/bob/             │ │ │
│  │  │ ├── SageMaker Studio                    │ │ │
│  │  │ └── Jupyter Notebooks                   │ │ │
│  │  └─────────────────────────────────────────┘ │ │
│  │                                               │ │
│  │  ┌─────────────────────────────────────────┐ │ │
│  │  │ SHARED SPACE: Team Project              │ │ │
│  │  │ ├── Shared EFS: /shared/team-project/   │ │ │
│  │  │ ├── Collaborative notebooks             │ │ │
│  │  │ └── Shared datasets                     │ │ │
│  │  └─────────────────────────────────────────┘ │ │
│  │                                               │ │
│  └───────────────────────────────────────────────┘ │
│                                                     │
└─────────────────────────────────────────────────────┘
```

## Creating a Domain (What You Configure)

```
AWS Console → SageMaker → Domains → Create Domain

Step 1: Domain Settings
┌─────────────────────────────────────────┐
│ Domain name: [my-ml-team]               │
│ Authentication: [IAM]                   │
└─────────────────────────────────────────┘

Step 2: Network Configuration
┌─────────────────────────────────────────┐
│ VPC: [vpc-12345]                        │
│ Subnets: [subnet-a, subnet-b, subnet-c] │
│ Security groups: [sg-67890]             │
│ Network access: [Public internet] or    │
│                 [VPC only]              │
└─────────────────────────────────────────┘

Step 3: Storage
┌─────────────────────────────────────────┐
│ EFS: [Auto-created by SageMaker]        │
│ Encryption: [Enabled]                   │
└─────────────────────────────────────────┘

Step 4: Users
┌─────────────────────────────────────────┐
│ Add users:                              │
│ - alice@company.com                     │
│ - bob@company.com                       │
│ - carol@company.com                     │
└─────────────────────────────────────────┘
```

## Real-World Analogy

**Domain = Office Building**
- Building has shared infrastructure (elevators, conference rooms)
- Each employee has a private office (user profile)
- Shared spaces for collaboration (meeting rooms)
- Building security (VPC, IAM)
- Shared storage room (EFS volume)

**User Profile = Employee**
- Private office/desk
- Personal files and workspace
- Access badge to enter building
- Can use shared resources

**Shared Space = Conference Room**
- Multiple employees can work together
- Shared whiteboards and materials
- Collaborative projects

## Key Benefits

### 1. Organization
```
Before Domain:
- Scattered notebooks
- No collaboration
- Inconsistent setup

With Domain:
✅ Centralized workspace
✅ Easy collaboration
✅ Consistent configuration
```

### 2. Security
```
✅ VPC isolation
✅ IAM authentication
✅ Encrypted storage (EFS)
✅ Network controls
✅ Audit logging
```

### 3. Collaboration
```
✅ Shared spaces for teams
✅ Share notebooks easily
✅ Common datasets
✅ Team projects
```

### 4. Resource Management
```
✅ Centralized billing
✅ Shared compute resources
✅ Consistent policies
✅ Easy user management
```

## For the Exam - Key Points

### What is a Domain?
- Organizational container for all SageMaker work
- Required before using SageMaker Studio
- Contains users, shared spaces, and configuration

### Components
- **EFS Volume**: Shared storage, carved up per user
- **User Profiles**: Individual users with private workspaces
- **Shared Spaces**: Collaborative workspaces
- **VPC Configuration**: Network settings

### VPC Modes
- **Default**: Two VPCs (SageMaker-managed + yours)
- **VPC-Only**: All traffic through your VPC (more secure)

### Storage
- One EFS volume per domain
- Private directories per user
- Shared directories for collaboration
- Persistent (doesn't disappear)

### Common Scenarios
- **Scenario**: Team needs to collaborate on ML project → Use shared spaces
- **Scenario**: Need high security → Use VPC-only mode
- **Scenario**: Multiple data scientists → Create user profiles for each
- **Scenario**: Starting with SageMaker Studio → Must create domain first

## Quick Reference

| Component | Purpose | Key Feature |
|-----------|---------|-------------|
| Domain | Container for everything | Organizational unit |
| EFS Volume | File storage | Shared, persistent |
| User Profile | Individual user | Private workspace |
| Shared Space | Team collaboration | Shared notebooks/data |
| VPC | Network isolation | Security and control |

## Remember

1. **Domain = Required first step** for SageMaker Studio
2. **One EFS volume** per domain (carved up for users)
3. **User profiles** = individual users with private space
4. **Shared spaces** = team collaboration areas
5. **Two VPC modes**: Default (internet + private) or VPC-only (more secure)
6. **You configure**: VPC, subnets, security groups
7. **SageMaker manages**: EFS volume, domain infrastructure

**Bottom line:** Domain is the organizational wrapper around all your SageMaker work. It provides structure, security, storage, and collaboration features for your ML team.
