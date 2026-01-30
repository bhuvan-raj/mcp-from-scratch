

# 📘 Module 4: MCP Transports

---

## 1. Overview

Module 4 focuses on **MCP transports**, which define **how the MCP client (LLM) communicates with the MCP server**.

A well-designed transport layer ensures:

* Reliable communication
* Security between client and server
* Efficient data transfer
* Compatibility between local and remote deployments

This module covers:

* STDIO transport
* HTTP transport
* Local vs remote MCP servers
* Tool discovery mechanism

---

## 2. What is a Transport in MCP?

A **transport** is a **communication channel** between the LLM (MCP client) and the MCP server.

Key responsibilities:

* Deliver **structured prompts** from the client
* Return **validated tool outputs** to the client
* Maintain **security boundaries**
* Enable **stateless or stateful interactions**

Without a transport, MCP cannot bridge the AI reasoning layer and real systems safely.

---

## 3. STDIO Transport

### 3.1 Overview

STDIO transport uses **standard input/output streams** to communicate between the LLM and MCP server.

* Typically used when **both client and server run locally** on the same machine
* Lightweight and fast
* Does not require network configuration

---

### 3.2 Characteristics

| Aspect        | STDIO                                |
| ------------- | ------------------------------------ |
| Communication | Standard input/output streams        |
| Use Case      | Local integration                    |
| Security      | Runs within local machine boundaries |
| Performance   | Low latency, high throughput         |
| Setup         | Minimal, no networking needed        |

---

### 3.3 Example Usage

```bash
# Start MCP server locally
python mcp_server.py

# LLM client communicates via stdin/stdout
cat prompt.json | python mcp_client.py
```

---

## 4. HTTP Transport

### 4.1 Overview

HTTP transport uses **HTTP/HTTPS requests** for communication.

* Designed for **remote or networked MCP servers**
* Supports REST or SSE (Server-Sent Events) patterns
* Enables **scalable client-server deployment**

---

### 4.2 Characteristics

| Aspect        | HTTP / SSE                                               |
| ------------- | -------------------------------------------------------- |
| Communication | JSON over HTTP or SSE                                    |
| Use Case      | Remote servers, cloud deployment                         |
| Security      | TLS/HTTPS, API keys, token authentication                |
| Performance   | Slightly higher latency than STDIO, but network-friendly |
| Setup         | Requires server endpoint and authentication              |

---

### 4.3 Example Usage

```python
import requests

payload = {
    "tool": "read_file",
    "input": {"path": "/var/log/syslog", "lines": 50}
}

response = requests.post("https://mcp.example.com/execute", json=payload)
print(response.json())
```

---

## 5. When to Use Each Transport

| Transport | Use Case          | Pros                                   | Cons                                    |
| --------- | ----------------- | -------------------------------------- | --------------------------------------- |
| STDIO     | Local MCP server  | Low latency, simple setup              | Only works locally                      |
| HTTP/SSE  | Remote MCP server | Networked, scalable, language-agnostic | Slightly higher latency, setup required |

**Guidelines:**

* Use **STDIO** for **local development or single-machine setups**
* Use **HTTP/SSE** for **production deployments, cloud, or remote servers**

---

## 6. Local vs Remote MCP Servers

### 6.1 Local MCP Server

* Runs on the **same machine as the LLM client**
* Uses **STDIO transport**
* Easy for **development and testing**
* Minimal security concerns (local boundaries)

### 6.2 Remote MCP Server

* Runs on a **different machine or cloud environment**
* Uses **HTTP/SSE transport**
* Suitable for **enterprise-scale deployments**
* Requires **authentication, encryption, and access control**

---

## 7. Tool Discovery Mechanism

### 7.1 What is Tool Discovery?

Tool discovery is how the **MCP client learns what tools are available** on the MCP server.

* Ensures the LLM knows **tool names, input/output schemas, and descriptions**
* Enables **dynamic, safe invocation of tools**

---

### 7.2 How It Works

1. MCP server exposes a **tool registry endpoint** (HTTP) or **tool list via STDIO**
2. Client fetches available tools and schemas
3. LLM selects the appropriate tool
4. Input/output validation occurs before execution

**Example HTTP endpoint:**

```http
GET /tools
Response:
[
  {
    "name": "read_file",
    "description": "Reads a file and returns its content",
    "input_schema": { ... },
    "output_schema": { ... }
  },
  ...
]
```

---

### 7.3 Best Practices

* Keep tool registry **up-to-date**
* Include **full schema and description**
* Limit tool exposure based on **client permissions**
* Version your tools to avoid breaking changes

---

## 8. Key Takeaways

* MCP **transports** define how clients communicate with servers
* **STDIO** is best for **local integrations**, **HTTP/SSE** for **remote deployments**
* Choice of transport affects **security, scalability, and latency**
* MCP supports **tool discovery**, enabling the LLM to know available tools dynamically
* Proper transport and discovery mechanisms are critical for **enterprise-ready MCP systems**

---

## 9. Next Steps

After mastering Module 4, learners can move on to:

* **Module 5: MCP Tool Implementation and Security**
