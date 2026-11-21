# PUCIT GPA Assistant · Assignment 3

A Python GPA and CGPA assistant for PUCIT BS Computer Science students. The FastMCP server exposes tools for grade conversion, GPA and CGPA calculations, course lookup, credit totals, and report saving.

## Hosted MCP server

- Endpoint: https://cgpa-mcp-server.onrender.com/mcp
- Authentication: No MCP API key required
- Source: https://github.com/ridaamir85/cgpa_mcp_server

```powershell
fastmcp list https://cgpa-mcp-server.onrender.com/mcp
fastmcp call https://cgpa-mcp-server.onrender.com/mcp get_semester_courses semester=6
```

These commands test the server directly without Groq. The first request may take longer after the free Render instance has been idle.

## Run the MCP server locally

In the GitHub server repository, run the following from the repository root. In the assignment project, install dependencies from the project root, then enter `server/` before running `python server.py`.

```powershell
python -m pip install -r requirements.txt
python server.py
```

The local endpoint is `http://127.0.0.1:8000/mcp`. The server uses the hosting platform's `PORT` when provided and defaults to port 8000.

## Run the assistant

The assignment project includes the `agent/` directory. Install the project dependencies from its root, then copy `agent/.env.example` to `agent/.env` if it does not already exist. Add your Groq API key and configure the MCP connection.

```dotenv
GROQ_API_KEY=your_groq_key
MCP_URL=https://cgpa-mcp-server.onrender.com/mcp
FASTMCP_KEY=
```

Use the local endpoint in `MCP_URL` when running against a local server. Keep the real `.env` file private.

```powershell
cd agent
python agent.py
```

The terminal client streams model text and tool updates. It stores conversations in SQLite by chat name. Use `/switch` to change chats, `/history` to view a saved transcript, and `/forget` to delete the selected chat. When an interrupted run leaves a saved tool request without a result, recovery can record a matching error result so the conversation can continue.

For the optional browser interface, run `python -m streamlit run app.py` from `agent/`. Use the terminal client to demonstrate streaming and chat commands.

## Report storage

`save_report` writes `.txt` files to the `reports/` directory beside the running server's `tools.py`. With a local server, that directory is inside `server/` in the assignment project. With the hosted endpoint, reports stay on Render's temporary filesystem and may be lost on restart or redeploy.
