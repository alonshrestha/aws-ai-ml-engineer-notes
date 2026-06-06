# Amazon Bedrock Agents (LM Agents / Agentic AI)

## What Is an Agent?

An agent extends a foundation model by giving it **tools** — the ability to access outside data, services, and APIs, then incorporate those results into its response. The model itself decides which tools to use and when, based on plain English descriptions you provide.

## Conceptual Architecture

```
User Request → [Agent Core (FM)] → Response
                    │
         ┌─────────┼─────────┐
         ▼         ▼         ▼
     Planning    Tools     Memory
     Module    (Actions)   (Chat history,
                           external data)
```

- **Planning module** — breaks down a request into sub-questions, decides which tools to invoke
- **Tools** — external functions the agent can call (APIs, databases, knowledge bases)
- **Memory** — conversation history + external data stores

Key insight: the agent figures out how to use each tool and pass parameters **entirely from plain English descriptions**. No hard-coded routing logic needed.

---

## Bedrock-Specific Implementation

### Tools = Action Groups

In Bedrock, tools are called **action groups**. Each action group contains:

1. **A Lambda function** — Python/JS/any Lambda-supported language that does the actual work
2. **Instructions** — plain English telling the FM when to use it (e.g., "Use this function to determine the current weather in a city")
3. **Parameter definitions** for each parameter:
   - Name
   - Description (plain English — tells the FM how to extract/format the value)
   - Type (string, integer, etc.)
   - Required or optional

If a required parameter is missing from the user's prompt, the agent can be configured to **ask the user** for the missing information before calling the tool.

Parameters can be specified via:
- JSON schema (like OpenAI API style)
- Visual table in the Bedrock console UI

### Knowledge Bases as Tools

You can associate one or more **knowledge bases** with an agent. The knowledge base becomes another tool in the agent's toolbox.

- You describe in plain English when to use it (e.g., "Use this for answering questions about self-employment")
- The agent routes relevant queries to the knowledge base automatically
- When RAG is used within an agent context, it's sometimes called **Agentic RAG**

### Code Interpreter (Optional)

When enabled, the agent can **write its own Python code** to:
- Perform mathematical calculations
- Handle complex operations an LLM isn't naturally good at
- Generate charts and visualizations

The agent autonomously decides when to write code vs. use other tools.

---

## Deploying Agents to Production

### Aliases

Before production use, you create an **alias** — a deployed snapshot/endpoint of the agent.

### Throughput Options

| Mode | Description |
|------|-------------|
| **On-Demand Throughput (OT)** | Uses account-level quota. Good for reasonable traffic levels. |
| **Provisioned Throughput (PT)** | Purchase increased throughput for high traffic / large token usage. |

### Invoking an Agent

- Use the **Bedrock Agent Runtime endpoint**
- Call `InvokeAgent` with the **alias ID**
- Endpoint: Agents for Amazon Bedrock Runtime

---

## Putting It All Together (Example)

A complete agent can combine:
1. **Foundation Model** (e.g., Claude, Titan) — the reasoning core
2. **Action Group** — Lambda function (e.g., weather API tool)
3. **Knowledge Base** — vector store for semantic search (e.g., self-employment book)
4. **Guardrails** — content filtering and safety controls

---

## Key Exam Points

- Tools are called **action groups** in Bedrock
- Action groups use **Lambda functions** under the hood
- The FM decides which tools to use based on **plain English instructions** (no hard-coded routing)
- Knowledge bases are just another tool available to the agent
- You must create an **alias** before deploying to production
- Use `InvokeAgent` API with the alias ID via the **Agents for Amazon Bedrock Runtime** endpoint
- Code interpreter allows the agent to write Python for computation and visualization
- Agents can ask users for missing required parameters
