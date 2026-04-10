# Neo Skill

Index codebase knowledge into [Neo](https://neo.nura.sh) — a persistent knowledge graph for developers, by [Nura Labs](https://nura.sh).

## Install

### 1. Add the Neo MCP server

**Claude Code:**
```bash
claude mcp add --transport http neo https://neo.nura.sh/api/mcp
```

**Codex:**
Add to `~/.codex/config.toml`:
```toml
[mcp_servers.neo]
url = "https://neo.nura.sh/api/mcp"
```

**Cursor:**
Add to MCP settings:
```json
{
  "mcpServers": {
    "neo": {
      "url": "https://neo.nura.sh/api/mcp"
    }
  }
}
```

On first use, your browser will open to authenticate. No API keys needed.

### 2. Install the skill

```bash
npx skills add nura-labs/neo-skill
```

### 3. Index your first project

Open your project and say:

```
Index this project in Neo
```

The AI analyzes your codebase locally, extracts patterns, conventions, and architecture, then sends the knowledge to your Neo graph.

### 4. Use it

In any session, your AI agent has full project context:

```
How do we create a new API endpoint in this project?
What patterns does this codebase use?
What's the architecture of the auth module?
```

## How it works

1. Your AI agent analyzes the codebase **locally** (no code leaves your machine)
2. It extracts structured knowledge: patterns, conventions, modules, architecture, decisions
3. It sends the extracted knowledge to Neo via MCP (only summaries, not source code)
4. Neo stores it in a knowledge graph with edges between related concepts
5. On future sessions, the AI queries Neo for project-specific context

## License

MIT
