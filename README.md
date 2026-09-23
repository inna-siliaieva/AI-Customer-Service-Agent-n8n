# AI Customer Service Agent with Tools in n8n

An AI-powered customer service workflow built in **n8n**.

The agent understands customer requests, selects the appropriate tool, retrieves external data through HTTP requests, and returns a relevant response. It also uses conversation memory to preserve context across messages.

---

## Project Overview

This workflow uses an **AI Agent architecture** in which the agent dynamically decides which tool to use based on the user's request.

The agent can:

- Check order status
- Retrieve customer account and subscription information
- Look up product features and pricing
- Remember previous messages in the same chat session
- Select the appropriate tool automatically
- Extract required parameters from natural-language requests

---

## Workflow Architecture

```text
TriggerChat
    |
    v
FeedbackAgent
    |
    +-- GroqModel
    +-- Simple Memory
    +-- ToolGetOrderStatus
    +-- ToolGetCustomerInfo
    +-- ToolGetProductInfo
```

### Main Nodes

| Node | Purpose |
|---|---|
| `TriggerChat` | Starts the workflow from the n8n chat interface |
| `FeedbackAgent` | Analyzes customer requests and selects the appropriate tool |
| `GroqModel` | Language model connected to the AI Agent |
| `Simple Memory` | Preserves conversation context during the chat session |
| `ToolGetOrderStatus` | Retrieves order and delivery information |
| `ToolGetCustomerInfo` | Retrieves customer account and subscription details |
| `ToolGetProductInfo` | Retrieves product features, pricing, and specifications |

---

## AI Model

The workflow uses a **Groq Chat Model**:

```text
openai/gpt-oss-120b
```

The model interprets incoming requests, determines whether a tool is required, selects the appropriate tool, and generates the final customer response.

---

## Tool Selection Logic

### Order Status Tool

Used for order, delivery, and shipment questions.

Required parameter:

```text
order_id
```

Example:

```text
Hi, can you check on my recent order?
Order id: ORD-011
```

### Customer Information Tool

Used for account and subscription questions.

Required parameter:

```text
customer_id
```

Example:

```text
I'm customer CUST-010. What subscription plan am I currently on?
```

### Product Information Tool

Used for product features, pricing, specifications, and comparisons.

Required parameter:

```text
product_name
```

Example:

```text
Can you tell me about the Enterprise License features and pricing?
```

---

## Dynamic AI Parameters

The agent extracts required values from natural-language requests and passes them to the appropriate HTTP tool.

```text
{{ $fromAI('order_id', 'Order ID', 'string') }}
```

```text
{{ $fromAI('customer_id', 'Customer ID', 'string') }}
```

```text
{{ $fromAI('product_name', 'Product Name', 'string') }}
```

This removes the need for hard-coded request values and allows the workflow to work dynamically with user input.

---

## Conversation Memory

The workflow uses **Simple Memory** to retain information across multiple messages within the same chat session.

Example:

```text
User: I'm customer CUST-010.
User: What customer ID did I give you earlier?
Agent: You provided the customer ID CUST-010 earlier.
```

This allows the agent to reuse previously supplied information without asking the customer to repeat it.

---

## Demonstration

### Workflow Overview

![Workflow Overview](screenshots/01-workflow-overview.png)

### Order Status Tool Selection

The AI Agent identifies an order-related request and calls `ToolGetOrderStatus`.

![Order Status Tool](screenshots/02-order-status-tool.png)

### Customer Information Tool Selection

The AI Agent identifies an account-related request and calls `ToolGetCustomerInfo`.

![Customer Info Tool](screenshots/03-customer-info-tool.png)

### Product Information Tool Selection

The AI Agent identifies a product-related request and calls `ToolGetProductInfo`.

![Product Info Tool](screenshots/04-product-info-tool.png)

### Conversation Memory

The agent remembers information supplied earlier in the same conversation without requiring another external lookup.

![Memory Context](screenshots/05-memory-context.png)

---

## Technologies Used

- n8n
- AI Agent
- Groq Chat Model
- HTTP Request Tool
- REST API
- Header Authentication
- Query Parameters
- Simple Memory
- Prompt Engineering
- Tool Descriptions
- AI-defined parameters with `$fromAI()`

---

## Skills Demonstrated

- Building multi-tool AI Agent workflows in n8n
- Connecting language models to automation workflows
- Integrating external APIs
- Configuring authenticated HTTP requests
- Designing tool descriptions for agent decision-making
- Extracting structured parameters from natural-language input
- Testing AI-driven tool selection
- Implementing conversational memory
- Debugging workflow configuration
- Documenting automation workflows

---

## Repository Structure

```text
AI-Customer-Service-Agent-n8n/
├── README.md
├── workflow/
│   └── ai-customer-service-agent.json
└── screenshots/
    ├── 01-workflow-overview.png
    ├── 02-order-status-tool.png
    ├── 03-customer-info-tool.png
    ├── 04-product-info-tool.png
    └── 05-memory-context.png
```

---

## Security

API keys, credentials, authentication headers, assessment IDs, and private environment identifiers are not included in the public workflow file.

Credentials should be configured directly inside n8n before running the workflow.

---

## Project Status

**Completed**

The workflow demonstrates:

- AI-based tool selection
- API integration
- Dynamic parameter extraction
- Conversation memory
- Multi-tool customer support automation

---

## Author

**Inna Siliaieva**
