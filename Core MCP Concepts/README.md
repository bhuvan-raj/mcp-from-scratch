

# 📘 Module 3: Core MCP Concepts

---

## 1. Overview

Module 3 introduces the **core building blocks of MCP**, which are essential for designing safe, scalable, and reliable AI–tool integrations.

* **Tools** – Functional actions exposed by MCP server
* **Resources** – Data or assets tools operate on
* **Prompts** – Structured instructions from LLM to server
* **Transports** – Communication channels between client and server
* **JSON Schema fundamentals** – Standardized tool input/output definitions
* **Input and output validation** – Ensures safety, determinism, and auditability

These concepts are foundational for **Module 4: MCP Server Implementation**.

---

## 2. Tools

### 2.1 What is a Tool?

A **tool** is a capability exposed by the MCP server that an LLM client can invoke.

**Examples:**

* Read a file: `read_file`
* Query Kubernetes logs: `get_pod_logs`
* Inspect cloud resources: `list_s3_buckets`

### 2.2 Tool Components

* **Name** – Unique identifier
* **Description** – Explains what the tool does
* **Input schema** – Defines required inputs
* **Output schema** – Defines expected outputs
* **Permissions** – Controls who/what can access it

### 2.3 Best Practices

* Single responsibility per tool
* Clear and descriptive names
* Strict input/output schemas
* Whitelist sensitive commands

---

## 3. Resources

### 3.1 What are Resources?

Resources are **data sources or assets** that tools interact with.

**Examples:**

* Files: `/var/log/syslog`
* APIs: AWS, GCP, or Azure endpoints
* Kubernetes clusters
* Databases

### 3.2 Resource Access Principles

* Resources are **always accessed through tools**
* Access controlled via **permissions and ACLs**
* Can be **static** (config files) or **dynamic** (live logs, metrics)

---

## 4. Prompts

### 4.1 Role of Prompts

Prompts are **structured instructions** sent by the LLM to the MCP server to invoke tools.

**Example:**

```json
{
  "tool": "read_file",
  "input": {
    "path": "/var/log/syslog",
    "lines": 50
  }
}
```

* `"tool"` → identifies the tool
* `"input"` → conforms to tool input schema

### 4.2 Best Practices

* Always follow JSON schema strictly
* Include all required fields
* Avoid free-text instructions to MCP server

---

## 5. Transports

Transport defines the **communication channel** between the **MCP client** and **MCP server**.

| Transport Type  | Use Case                  | Characteristics                                                                    |
| --------------- | ------------------------- | ---------------------------------------------------------------------------------- |
| **Stdio**       | Local Integration         | Standard input/output. Used when the MCP server runs as a local process.           |
| **SSE (HTTP)**  | Remote / Web              | Server-Sent Events. Used for connecting to servers over a network or the internet. |
| **WebSocket**   | Real-time bidirectional   | Full-duplex communication for streaming events or logs.                            |
| **HTTP / REST** | Remote / Web              | JSON over HTTP, simple request/response.                                           |
| **GRPC**        | Remote / High-performance | Typed, binary protocol for efficient communication.                                |

**Best Practices:**

* Always use **secure channels (TLS/HTTPS)** for remote transports
* Prefer **stateless requests** for scaling
* Authenticate clients before allowing execution

---

## 6. JSON Schema Fundamentals

### 6.1 What is JSON Schema?

JSON Schema is a **standard way to define structure, types, and validation rules** for tool inputs and outputs.

**Input Example:**

```json
{
  "type": "object",
  "properties": {
    "path": { "type": "string" },
    "lines": { "type": "integer", "minimum": 1, "maximum": 1000 }
  },
  "required": ["path", "lines"]
}
```

**Output Example:**

```json
{
  "type": "object",
  "properties": {
    "status": { "type": "string" },
    "content": { "type": "string" }
  },
  "required": ["status", "content"]
}
```

---

## 7. Input and Output Validation

**Why Validation Matters:**

* Prevents malformed or unsafe inputs
* Ensures predictable execution
* Makes multi-step workflows reliable

**Example (Python):**

```python
from jsonschema import validate, ValidationError

input_data = {"path": "/var/log/syslog", "lines": 50}

try:
    validate(instance=input_data, schema=tool_input_schema)
except ValidationError as e:
    print(f"Invalid input: {e}")
```

---

## 8. Key Takeaways

* **Tools**: Actions exposed by MCP server; must be safe and well-defined
* **Resources**: Data/assets tools operate on; access strictly controlled
* **Prompts**: Structured LLM instructions; schema adherence mandatory
* **Transports**: Secure communication channels; choose appropriate transport per use case
* **JSON Schema**: Standardized input/output validation
* **Validation**: Ensures safety, determinism, and reliability

These concepts form the **core of any MCP server**.

---

## 9. What Comes Next

After mastering this module, learners will be ready to implement [Module 4- MCP Transports](https://github.com/bhuvan-raj/mcp-from-scratch/tree/main/MCP%20Transports)

