

#  Module 6: MCP Development Environment

---

## 1. Overview

Before building MCP tools and servers, a **proper development environment** is critical.
This module ensures you can:

* Choose the right language (Node.js or Python)
* Understand the MCP SDK ecosystem
* Structure MCP projects cleanly
* Run MCP servers locally using STDIO and HTTP transports

This module bridges **theory (Level 1)** and **implementation (Level 2)**.

---

## 2. Required Tools

### 2.1 Core Requirements (Mandatory)

| Tool                      | Purpose                                |
| ------------------------- | -------------------------------------- |
| **Git**                   | Version control                        |
| **Node.js (LTS)**         | MCP server/client development (JS)     |
| **Python 3.9+**           | MCP server/client development (Python) |
| **Package Manager**       | `npm` / `yarn` or `pip` / `poetry`     |
| **Code Editor**           | VS Code recommended                    |
| **JSON Schema Validator** | Input/output validation                |
| **curl / httpie**         | Testing HTTP MCP servers               |

---

### 2.2 Optional but Recommended

| Tool                   | Why it helps              |
| ---------------------- | ------------------------- |
| **Docker**             | Containerized MCP servers |
| **Postman / Insomnia** | Testing HTTP transports   |
| **pre-commit**         | Enforce code quality      |
| **pytest / jest**      | Automated testing         |

---

## 3. Node.js vs Python for MCP

Both ecosystems are valid. The choice depends on **use case, team skills, and deployment target**.

---

### 3.1 Node.js for MCP

**Strengths**

* Native async I/O
* Excellent STDIO handling
* Strong ecosystem for CLIs and agents
* Natural fit for HTTP/SSE transports

**Typical Use Cases**

* Local MCP servers
* CLI-based agents
* Real-time streaming tools

**Example:**

```bash
node mcp-server.js
```

---

### 3.2 Python for MCP

**Strengths**

* Simple syntax and readability
* Rich data processing libraries
* Strong validation and schema tooling
* Popular in ML and DevOps environments

**Typical Use Cases**

* Backend MCP servers
* Data-heavy tools
* Infrastructure automation

**Example:**

```bash
python mcp_server.py
```

---

### 3.3 Comparison Summary

| Aspect         | Node.js      | Python               |
| -------------- | ------------ | -------------------- |
| Async I/O      | Native       | Async via asyncio    |
| STDIO handling | Excellent    | Good                 |
| HTTP servers   | Very strong  | Strong               |
| Learning curve | Medium       | Easy                 |
| Best for       | Agents, CLIs | Backends, automation |

👉 **Recommendation:**
Start with **Python** for learning, explore **Node.js** for advanced or CLI-based MCP servers.

---

## 4. MCP SDK Overview

### 4.1 What Is the MCP SDK?

The MCP SDK provides **libraries and utilities** to:

* Register tools
* Define schemas
* Handle transports
* Manage tool discovery
* Enforce validation and security

SDKs abstract away protocol complexity so you focus on **tool logic**.

---

### 4.2 Common SDK Capabilities

* Tool registration APIs
* JSON schema enforcement
* Transport adapters (STDIO, HTTP)
* Error handling utilities
* Logging and observability

---

### 4.3 SDK Usage Example (Conceptual)

```python
from mcp import Server, Tool

server = Server()

@server.tool(
    name="read_file",
    input_schema=ReadFileSchema,
    output_schema=ReadFileOutput
)
def read_file(path: str):
    ...
```

---

## 5. Project Folder Structure

### 5.1 Why Structure Matters

A clean structure ensures:

* Maintainability
* Scalability
* Security
* Easy onboarding

---

### 5.2 Recommended MCP Project Structure

```text
mcp-project/
├── tools/
│   ├── read_file.py
│   └── list_files.py
├── schemas/
│   ├── input.py
│   ├── output.py
│   └── errors.py
├── transports/
│   ├── stdio.py
│   └── http.py
├── server.py
├── client.py
├── config.yaml
├── requirements.txt
└── README.md
```

---

### 5.3 Folder Responsibilities

| Folder        | Responsibility        |
| ------------- | --------------------- |
| `tools/`      | Tool logic            |
| `schemas/`    | Input/output models   |
| `transports/` | Communication layer   |
| `server.py`   | MCP server bootstrap  |
| `client.py`   | MCP client            |
| `config.yaml` | Runtime configuration |

---

## 6. Running MCP Servers Locally

### 6.1 Running with STDIO Transport

STDIO is the **default choice for local development**.

```bash
python server.py
```

* LLM communicates via stdin/stdout
* No networking required
* Fast feedback loop

---

### 6.2 Running with HTTP Transport

Used when testing **remote-style deployment locally**.

```bash
python server.py --transport http --port 8080
```

Test using curl:

```bash
curl -X POST http://localhost:8080/execute \
  -H "Content-Type: application/json" \
  -d '{"tool":"read_file","input":{"path":"/etc/hosts"}}'
```

---

## 7. Best Practices

* Start with **STDIO**, move to **HTTP**
* Validate inputs and outputs strictly
* Use environment variables for secrets
* Keep tools stateless where possible
* Log all tool invocations

---

## 8. Key Takeaways

* A solid environment is mandatory for MCP development
* Both Node.js and Python are first-class MCP platforms
* SDKs simplify tool registration and transport handling
* Clean project structure enables scaling
* Local MCP servers allow fast iteration

---

## 9. What Comes Next

After this module, learners will:

* Build their first MCP server
* Register tools and schemas
* Run servers locally via STDIO and HTTP

Next module:
**Module 7: [ Building Your First MCP Server](https://github.com/bhuvan-raj/mcp-from-scratch/tree/main/Building%20Your%20First%20MCP%20Server)**

