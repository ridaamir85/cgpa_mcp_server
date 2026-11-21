# PUCIT GPA Assistant - Assignment 3

The GPA/CGPA assistant uses MCP tools, persistent SQLite memory, asynchronous execution, streaming, and interrupted-tool-call recovery. This GitHub repository contains the server files at its root; the full local assignment has separate `agent/` and `server/` folders.

## Deployment details

- Hosted MCP server: https://cgpa-mcp-server.onrender.com/mcp
- MCP authentication: Not required; the current endpoint is public and does not check an API key.
- Server GitHub repository: https://github.com/ridaamir85/cgpa_mcp_server

The Groq API key belongs to the separate agent application. This tool server does not require it.

## Available tools

- `marks_to_grade_points`
- `calculate_semester_gpa`
- `calculate_new_cgpa`
- `required_gpa_for_target`
- `get_semester_courses`
- `get_remaining_credit_hours`
- `save_report`

## Test the hosted server

```powershell
fastmcp list https://cgpa-mcp-server.onrender.com/mcp
fastmcp call https://cgpa-mcp-server.onrender.com/mcp get_semester_courses semester=6
```

Render's free instance may take time to wake after inactivity. The `/mcp` endpoint is intended for MCP clients; opening it directly in a browser may return a missing-session error.

## Connect the agent

Set these values in the separate agent's `.env` file:

```text
MCP_URL=https://cgpa-mcp-server.onrender.com/mcp
FASTMCP_KEY=
```

Keep the agent's existing `GROQ_API_KEY` configured locally. Never commit its `.env` file.

## Run locally

The commands below run the server from this GitHub repository. In the full assignment folder, enter `server/` first.

```powershell
python -m pip install -r requirements.txt
python server.py
```

Local MCP URL: `http://127.0.0.1:8000/mcp`. The server reads `PORT` when supplied by the hosting platform and otherwise uses port 8000.

## Run the full assignment agent

From the full assignment folder, install its dependencies with `py -m pip install -r requirements.txt`. Copy `agent/.env.example` to `agent/.env` only if `.env` does not already exist. Add your Groq key and the hosted MCP settings shown above.

```powershell
cd agent
py agent.py
```

Enter a name to select a `thread_id`. Commands are `/switch`, `/history`, `/forget`, and `exit`. The SQLite checkpointer preserves messages in `chats.db` in the working directory, so consistently run the client from `agent/`. SQLite may automatically create `chats.db-wal` and `chats.db-shm`.

Streaming displays model text through `messages` and model/tool progress through `updates`. Recovery adds matching error ToolMessages for the supported case of an interrupted run ending with unanswered model tool calls.

No local MCP server is needed when using Render. To test locally, run `server.py` in another terminal, change `MCP_URL` to the local endpoint, and restart the agent.

## Render configuration

- Runtime: Python
- Build command: `pip install -r requirements.txt`
- Start command: `python server.py`
- Root directory: blank for this GitHub repository
- Compute: Free

## Report storage

`save_report` writes `.txt` reports into the server's `reports` directory and refuses to overwrite an existing filename. Reports are stored on the server, not downloaded to the client computer. Render's free filesystem is temporary, so reports may be lost after restarts or redeployments.

## Submission

Submit `BCSF24M052_Assignment3.zip` containing the assignment's `agent/`, `server/`, dependency file, and README. Include `agent/.env.example`.

Exclude real `.env` files, chat databases and their companion files, `__pycache__/`, virtual environments, and generated reports. The Streamlit interface and development test are optional; the terminal agent does not require them.
