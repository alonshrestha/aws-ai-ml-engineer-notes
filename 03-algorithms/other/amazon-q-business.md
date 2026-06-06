# Amazon Q Business

## What is it?
A fully managed Gen-AI assistant for your **employees**, trained on your **company's internal data**. Think ChatGPT but it only knows your company stuff.

Built on Amazon Bedrock (multiple foundation models), but you don't get to choose which models — it's a higher-level service.

## What is RAG?
RAG = **Retrieval Augmented Generation**

Two ways AI can answer a question:
- **From memory (no RAG)** → AI learned from data during training, then answers from what it remembers. Like answering an exam without notes.
- **By looking up first (RAG)** → AI **searches external data in real-time** when you ask, then answers based on what it found. Like opening a book during the exam.

**Key point:** RAG = searching data **at the time you ask**, not having learned from data before.

**Connected to data ≠ RAG:**
- AI trained on internet data and answering from memory → **not RAG**
- AI actively searching the internet **right now** to answer your question → **RAG**

**Example:**
- "What is the capital of France?" → AI knows this from training, no lookup needed → not RAG
- "What happened in the news today?" → AI searches the web right now, finds articles, then answers → RAG

**RAG data source can be anything the AI is connected to:**
- Company docs in S3, Slack, SharePoint (Q Business)
- The internet (ChatGPT with browsing)
- A PDF you uploaded (ChatGPT with file upload)

In Q Business, RAG means it searches your connected company data sources first, then generates an answer and shows you the source document.

## What Can It Do?
- Answer questions from company data ("What was discussed in last week's meeting?")
- Generate content ("Write a job posting for senior PM role")
- Provide summaries from internal documents
- Perform routine actions (submit time-off requests, send meeting invites)
- Works like RAG — finds the source document, gives you the answer, and shows the source

## Key Components

### 1. Data Connectors (Reading Data)
- Fully managed RAG — connects to 40+ enterprise data sources
- **AWS**: S3, RDS, Aurora, WorkDocs
- **Non-AWS**: Microsoft 365, Google Drive, Gmail, Slack, SharePoint, Salesforce, etc.
- Crawls these sources so you can search and query them

### 2. Plugins (Taking Actions)
- Lets Q Business **interact** with third-party services, not just read
- Example: "Create a Jira issue" → Q Business creates the ticket for you
- Built-in: Jira, ServiceNow, Zendesk, Salesforce
- Custom plugins possible via APIs

### 3. IAM Identity Center (Security)
- Users authenticate through IAM Identity Center (username + password)
- Users only see documents **they have permission to access**
- Can integrate with external identity providers (Google login, Microsoft Active Directory)
- Uses your company's existing login system

### 4. Admin Controls (Guardrails)
- Same concept as Guardrails in Bedrock but for Q Business
- **Block topics** — e.g., block "gaming consoles" so employees can't ask about it
- **Restrict to internal data only** — Q Business answers only from company docs, not general knowledge
- Can be set at **global level** (all topics) or **topic level** (specific subjects)

## Exam Key Points
- Q Business = Gen-AI assistant for employees using company internal data
- Built on Bedrock but no control over which foundation model
- Data Connectors = read data (RAG), Plugins = take actions
- IAM Identity Center handles auth + document-level access control
- Admin Controls = same as Bedrock Guardrails but for Q Business
