# Neo Skills

Skills for [Neo](https://neo.nura.sh) — a persistent knowledge graph for developers, by [Nura Labs](https://nura.sh).

## Install

### 1. Add the Neo MCP server

**Claude Code:**
```bash
claude mcp add --transport http neo https://neo.nura.sh/api/mcp
```

**Codex:**
```toml
[mcp_servers.neo]
url = "https://neo.nura.sh/api/mcp"
```

**Cursor:**
```json
{
  "mcpServers": {
    "neo": {
      "url": "https://neo.nura.sh/api/mcp"
    }
  }
}
```

On first use, your browser opens to authenticate. No API keys needed.

### 2. Install skills

```bash
npx skills add nura-labs/neo-skills
```

This installs 3 skills:

| Skill | What it does |
|-------|-------------|
| `neo` | Search before coding, follow existing patterns |
| `neo-index` | Index a codebase into the knowledge graph |
| `neo-wikilinks` | Write content with [[wikilinks]] that auto-create connections |

### 3. Use it

```
Index this project in Neo
How do we create API endpoints in this project?
What patterns does this codebase use?
```

## How it works

1. Your AI agent analyzes the codebase **locally** (no source code leaves your machine)
2. It extracts patterns, conventions, architecture as structured knowledge
3. It sends summaries to Neo via MCP with [[wikilinks]] that auto-create graph connections
4. A Dream Cycle runs every 12h to discover missing connections and fix bad relationships
5. On future sessions, the AI queries Neo for project-specific context

## License

MIT
