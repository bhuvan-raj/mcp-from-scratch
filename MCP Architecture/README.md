# 📘 Module 2: MCP Architecture
<img src="https://github.com/bhuvan-raj/mcp-from-scratch/blob/main/assets/arch.webp" alt="Banner" />

---

## 1. Overview of MCP Architecture

MCP follows a **strict client–server architecture** designed specifically for AI systems.

Unlike traditional applications where both sides are trusted, MCP assumes:

> **The AI model must never directly access real systems.**

Instead, access is mediated through a controlled server.

---

### High-level architecture

```
User
  ↓
LLM (MCP Client)
  ↓
MCP Protocol
  ↓
MCP Server
  ↓
Tools / Data / Systems
```

This separation is the foundation of MCP’s **safety, scalability, and enterprise readiness**.

---

## 2. Client–Server Model

### Why client–server?

AI models are:

* Non-deterministic
* Probabilistic
* Not secure environments

Infrastructure systems are:

* Deterministic
* Sensitive
* Security-critical

MCP separates these responsibilities clearly.

---

### MCP Client

* Usually the **LLM runtime**
* Decides *when* to use tools
* Sends structured tool calls
* Receives structured responses
* Never executes real commands

Examples:

* ChatGPT
* Claude
* Local LLM agents

---

### MCP Server

* Runs locally or remotely
* Owns tool execution
* Accesses files, APIs, infrastructure
* Enforces permissions
* Returns validated output

This separation ensures:

* Least privilege
* Clear trust boundary
* Auditable execution

---

## 3. Role of LLM as MCP Client

The LLM plays a **reasoning role**, not an execution role.

---

### Responsibilities of LLM client

The LLM:

* Understands user intent
* Chooses the correct tool
* Formats input using schema
* Interprets structured output
* Explains results to the user

The LLM does **not**:

* Access filesystem
* Run shell commands
* Call cloud APIs directly
* Hold credentials

---

### Why this is critical

If LLMs directly executed tools:

* Prompt injection becomes dangerous
* Secrets could leak
* Commands could be abused
* Infrastructure could be destroyed

MCP ensures:

> **The model can think, but cannot act directly.**

---

## 4. MCP Server Responsibilities

The MCP server is the **trusted execution layer**.

---

### Core responsibilities

1. Expose tools
2. Define schemas
3. Validate inputs
4. Execute actions
5. Enforce security
6. Return structured results

---

### MCP server owns:

* File access rules
* Command whitelisting
* API authentication
* Environment variables
* Secrets
* Network access

---

### Example MCP server tasks

* Read log files
* Query Kubernetes API
* Parse Terraform plan
* Inspect AWS resources
* Analyze system metrics

All under strict control.

---

## 5. Communication Flow

Understanding the flow is essential.

---

### Step-by-step flow

```
1. User asks a question
2. LLM reasons about intent
3. LLM decides a tool is needed
4. LLM sends MCP tool request
5. MCP server validates schema
6. Tool executes safely
7. Server returns structured output
8. LLM explains result
```

---

### Example flow

User:

> “Why is my pod crashing?”

LLM:

* Needs logs
* Needs events

LLM calls:

* get_pod_logs
* get_pod_events

MCP server:

* Fetches real data
* Returns structured JSON

LLM:

* Correlates information
* Provides explanation

This is **grounded AI**, not hallucinated AI.

---

## 6. Stateless vs Stateful Interactions

This is a very important architectural concept.

---

### Stateless MCP interactions

Most MCP calls are **stateless**.

Each request:

* Is independent
* Contains all required input
* Has no memory of previous calls

Example:

```
read_file("/var/log/syslog")
```

Advantages:

* Simple
* Scalable
* Safe
* Easy to audit

This is the default and recommended approach.

---

### Stateful MCP interactions

Stateful interactions maintain context between calls.

Example:

* Session-based analysis
* Multi-step workflows
* Long-running investigations

State may include:

* Cached data
* Session IDs
* Intermediate results

---

### Comparison

| Aspect      | Stateless | Stateful       |
| ----------- | --------- | -------------- |
| Complexity  | Low       | High           |
| Scalability | High      | Medium         |
| Security    | Strong    | Riskier        |
| Debugging   | Easy      | Hard           |
| Recommended | ✅ Yes     | ⚠️ Limited use |

---

### Best practice

> MCP servers should be **stateless by default**, and **stateful only when unavoidable**.

---

## 7. Security Boundaries (Most Important Topic)

Security is the heart of MCP architecture.

---

### Primary security principle

> **The LLM is untrusted.
> The MCP server is trusted.**

---

### Security boundaries include:

#### 1️⃣ Execution boundary

* LLM cannot execute commands
* Server executes tools

#### 2️⃣ Permission boundary

* Tools expose only allowed actions
* No unrestricted access

#### 3️⃣ Data boundary

* Only approved files or APIs accessible
* No arbitrary paths

#### 4️⃣ Credential boundary

* Secrets stored on server
* Never exposed to LLM

---

### Example

❌ Dangerous:

```
run_command("rm -rf /")
```

✅ Safe:

```
get_disk_usage()
```

Whitelisted, controlled, safe.

---

## 8. Why This Architecture Is Enterprise-Ready

Traditional AI integrations break under enterprise requirements.

MCP architecture supports:

* Audit logging
* Access control
* Tool-level permissions
* Predictable behavior
* Compliance readiness

This is why MCP is suitable for:

* Banks
* Cloud providers
* Enterprises
* Internal platforms

---

## 9. Architecture Diagram (Conceptual)

```
+---------------------+
|        User         |
+----------+----------+
           |
           v
+---------------------+
|   LLM (MCP Client)  |
|  - reasoning        |
|  - tool selection   |
+----------+----------+
           |
           v
+---------------------+
|     MCP Server      |
|  - validation       |
|  - permissions      |
|  - execution        |
+----------+----------+
           |
           v
+---------------------+
| Systems / Tools     |
| Files | APIs | K8s  |
+---------------------+
```

---

## 10. Key Takeaways (Very Important)

* MCP uses strict client–server architecture
* LLM acts only as reasoning engine
* MCP server owns execution
* Communication is schema-driven
* Stateless design is preferred
* Security boundaries protect infrastructure
* MCP enables safe AI agents

---

## 11. What Comes Next

After this module, you must understand:

➡ Tools
➡ Resources
➡ Prompts
➡ Transports
➡ JSON schema design

These form **Module 3: Core MCP Concepts**.

