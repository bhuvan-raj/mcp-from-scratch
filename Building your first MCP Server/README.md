# Building Your First MCP local Server ( vs code  + python )

### Step 1: Install the Requirements

1. Install `uv`: Open your VS Code terminal and run:
* **Mac/Linux**: `curl -LsSf https://astral.sh/uv/install.sh | sh`
* **Windows**: `powershell -c "irm https://astral.sh/uv/install.ps1 | iex"`


2. **Install the "GitHub Copilot" Extension**: Ensure you are signed in to GitHub and have the Copilot Chat extension enabled.

### Step 2: Set Up the Project

```bash
# Create and enter the directory
mkdir my-mcp-lab && cd my-mcp-lab

# Initialize the project and add FastMCP
uv init --app
uv add "fastmcp"

```

### Step 3: Write the Server Code

Create `server.py` in your project folder:

```python
from fastmcp import FastMCP

mcp = FastMCP("VSCodeLab")

@mcp.tool()
def get_system_load() -> str:
    """Calculates current simulated system load. Use for performance checks."""
    return "Current system load is 15%. All services green."

@mcp.resource("config://settings")
def get_config() -> str:
    """Returns the simulated lab configuration settings."""
    return "Mode: Development; Version: 2026.1; Transport: Stdio"

if __name__ == "__main__":
    mcp.run()

```

### Step 4: Register the Server in VS Code

VS Code 2026 uses a specific command to register local servers.

1. Open the **Command Palette** (`Cmd+Shift+P` or `Ctrl+Shift+P`).
2. Type **"MCP: Add Server"** and select it.
3. Choose **"Local Executable (Stdio)"**.
4. Enter the following details when prompted:
* **Server Name**: `lab-server`
* **Command**: `uv`
* **Arguments**: `["--directory", "/ABSOLUTE/PATH/TO/my-mcp-lab", "run", "python", "server.py"]`
*(Note: Right-click your `server.py` and select "Copy Path" to get the absolute path)*.


5. VS Code will create a `.vscode/mcp.json` file automatically.

### Step 5: Start and Verify

1. Open `.vscode/mcp.json`. You will see a **"Start"** or **"Restart"** button (CodeLens) floating above the JSON text. Click **Start**.
2. Open **Copilot Chat** (`Ctrl+Alt+I`).
3. Switch the mode to **"Agent"** at the top of the chat window.
4. Click the **Tools icon (🛠️)**. You should see `lab-server` listed with the `get_system_load` tool. Ensure it is checked.

### Step 6: Test the Lab

Type the following into the chat:

> "Check the system load and read the lab config settings."

The Agent will call your Python functions, validate the input, and present the structured response inside the chat UI.

---

### 🧪 Debugging Your VS Code Server

* **The Logs**: If the server fails to start, open the **Output** tab in VS Code and select **"MCP Server: lab-server"** from the dropdown menu to see the real-time Python errors.
* **Path Errors**: 90% of issues are caused by relative paths. Always use `C:\Users\...` or `/Users/...`.

[Setup MCP Server In Python | Step-By-Step Tutorial](https://www.youtube.com/watch?v=8g0z3DNi_fU)

This video provides a practical, step-by-step walkthrough of setting up an MCP server in Python and configuring it for use with IDEs like VS Code.
