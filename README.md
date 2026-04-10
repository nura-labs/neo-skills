# Neo Skill

A skill for indexing codebase knowledge into [Neo](https://neo.nura.sh) — a persistent knowledge graph for developers.

## What it does

This skill teaches your AI agent how to:
- Analyze your codebase and extract patterns, conventions, architecture, and decisions
- Send that knowledge to your Neo knowledge graph via MCP
- Query the graph to answer context-aware questions about your projects

## Setup

### 1. Create a Neo account

Go to [neo.nura.sh](https://neo.nura.sh) and create an account. Copy your API token from Settings.

### 2. Add the Neo MCP server

```bash
# Claude Code
claude mcp add neo https://neo.nura.sh/api/mcp --header "Authorization: Bearer YOUR_TOKEN"

# Cursor — add to your MCP settings:
# { "neo": { "url": "https://neo.nura.sh/api/mcp", "headers": { "Authorization": "Bearer YOUR_TOKEN" } } }
```

### 3. Install the skill

```bash
npx skills add nura-labs/neo-skill
```

### 4. Index your first project

Open your project in Claude Code and say:

```
Index this project in Neo
```

The AI will analyze your codebase, extract knowledge, and send it to your Neo graph.

### 5. Use it

Now in any session, your AI agent has full context:

```
How do we create a new API endpoint in this project?
What patterns does this codebase use?
What's the architecture of the auth module?
```

## How it works

1. Your AI agent analyzes the codebase **locally** (no code leaves your machine)
2. It extracts structured knowledge: patterns, conventions, modules, architecture, decisions
3. It sends that knowledge to Neo via MCP (only the extracted summaries, not your source code)
4. Neo stores it in a knowledge graph with edges between related concepts
5. On future sessions, the AI queries Neo for project-specific context

## License

MIT
