# CGPA MCP Server

FastMCP server for the PUCIT GPA/CGPA assistant, hosted on Render for AI Assignment 3.

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

```powershell
python -m pip install -r requirements.txt
python server.py
```

Local MCP URL: `http://127.0.0.1:8000/mcp`. The server reads `PORT` when supplied by the hosting platform and otherwise uses port 8000.

## Report storage

`save_report` writes `.txt` reports into the server's `reports` directory and refuses to overwrite an existing filename. Reports are stored on the server, not downloaded to the client computer. Render's free filesystem is temporary, so reports may be lost after restarts or redeployments.
