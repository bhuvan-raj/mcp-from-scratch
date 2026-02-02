
# 📘 Module 5: MCP Data Models

---

## 1. Overview

MCP data models define **how information is structured, validated, exchanged, and trusted** between the LLM (client) and the MCP server.

In MCP, **data models are not optional** — they are the primary mechanism that:

* Makes AI-tool interaction deterministic
* Prevents malformed or unsafe requests
* Enables reliable automation
* Reduces hallucinations
* Allows systems to scale safely

This module focuses on **tool definitions, schemas, structured responses, and error handling**.

---

## 2. Tool Definition Structure

### 2.1 What Is a Tool Definition?

A **tool definition** is a formal contract between the MCP server and the LLM client describing:

* What the tool does
* What inputs it accepts
* What outputs it returns
* What errors it can produce

The LLM relies entirely on this definition to reason about tool usage.

---

### 2.2 Core Fields of a Tool Definition

A standard MCP tool definition includes:

```json
{
  "name": "read_file",
  "description": "Reads a file and returns its contents",
  "input_schema": { ... },
  "output_schema": { ... }
}
```

| Field           | Purpose                          |
| --------------- | -------------------------------- |
| `name`          | Unique identifier                |
| `description`   | Human-readable explanation       |
| `input_schema`  | Defines required input structure |
| `output_schema` | Defines returned data format     |

---

### 2.3 Design Best Practices

* Use **clear, deterministic names**
* Keep tools **single-purpose**
* Avoid ambiguous descriptions
* Explicitly define **all required fields**

---

## 3. Input Schema

### 3.1 Purpose of Input Schema

Input schemas describe **exactly what the MCP server expects** from the client.

They enforce:

* Required fields
* Allowed data types
* Value constraints
* Input validation

---

### 3.2 Example Input Schema

```json
{
  "type": "object",
  "properties": {
    "path": {
      "type": "string",
      "description": "Absolute path to the file"
    },
    "lines": {
      "type": "integer",
      "minimum": 1,
      "maximum": 1000
    }
  },
  "required": ["path"]
}
```

---

### 3.3 Why Strict Input Schemas Matter

* Prevents arbitrary command execution
* Eliminates ambiguity
* Makes AI behavior predictable
* Simplifies debugging

---

## 4. Output Schema

### 4.1 Purpose of Output Schema

Output schemas ensure that **responses from tools are structured and predictable**, allowing the LLM to reason reliably over results.

---

### 4.2 Example Output Schema

```json
{
  "type": "object",
  "properties": {
    "status": {
      "type": "string",
      "enum": ["success", "error"]
    },
    "content": {
      "type": "string"
    }
  },
  "required": ["status"]
}
```

---

### 4.3 Benefits of Output Schemas

* Eliminates free-text responses
* Enables chaining of tool outputs
* Allows downstream automation
* Makes logs and audits consistent

---

## 5. Error Handling Model

### 5.1 Why Error Modeling Is Critical

Without structured errors, the LLM:

* Misinterprets failures
* Hallucinates success
* Retries incorrectly
* Breaks workflows

MCP requires **explicit error models**.

---

### 5.2 Structured Error Schema

```json
{
  "type": "object",
  "properties": {
    "error_code": {
      "type": "string"
    },
    "message": {
      "type": "string"
    },
    "details": {
      "type": "string"
    }
  },
  "required": ["error_code", "message"]
}
```

---

### 5.3 Common Error Categories

| Error Code           | Meaning                  |
| -------------------- | ------------------------ |
| `INVALID_INPUT`      | Schema validation failed |
| `PERMISSION_DENIED`  | Access not allowed       |
| `RESOURCE_NOT_FOUND` | Resource missing         |
| `EXECUTION_ERROR`    | Tool execution failed    |

---

## 6. Structured Responses

### 6.1 What Are Structured Responses?

Structured responses ensure that **all tool outputs follow a predictable format**, even during failures.

Example unified response:

```json
{
  "status": "success",
  "data": { ... },
  "error": null
}
```

Or on failure:

```json
{
  "status": "error",
  "data": null,
  "error": {
    "error_code": "INVALID_INPUT",
    "message": "Path must be absolute"
  }
}
```

---

### 6.2 Advantages

* Simplifies client logic
* Enables retries and fallbacks
* Supports multi-tool workflows
* Improves observability

---

## 7. Preventing Hallucinations Using Schema

### 7.1 Why Hallucinations Occur

LLMs hallucinate when:

* Output format is ambiguous
* Tool responses are free-text
* Errors are not structured
* Schema constraints are missing

---

### 7.2 Schema as a Guardrail

Schemas prevent hallucinations by:

* Forcing **allowed fields only**
* Enforcing **data types**
* Rejecting unknown properties
* Making outputs machine-verifiable

---

### 7.3 Key Techniques

* Always use **strict JSON schemas**
* Reject unknown keys (`additionalProperties: false`)
* Use enums for fixed values
* Validate outputs before returning them to the LLM

Example:

```json
{
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "status": { "type": "string", "enum": ["success", "error"] }
  }
}
```

---

## 8. Key Takeaways

* MCP data models define **trust boundaries**
* Tool definitions act as formal contracts
* Input and output schemas enforce correctness
* Structured error models prevent misinterpretation
* Schema-driven design drastically reduces hallucinations
* Strong data models are essential for production MCP systems

---

## 9. What Comes Next

After this module, learners will be ready for:

* **Module 6: [ MCP Development Environment](https://github.com/bhuvan-raj/mcp-from-scratch/tree/main/MCP%20Development%20Environment)**
