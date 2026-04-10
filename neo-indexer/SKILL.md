---
name: neo-indexer
description: Index codebase knowledge into Neo — a persistent knowledge graph. Analyze patterns, conventions, architecture, modules, and decisions from any project, then send them to your Neo knowledge graph via MCP. Use when the user asks to index a project, save knowledge, or query project-specific context.
metadata:
  author: nura-labs
  version: "1.0"
---

# Neo Knowledge Indexer

You have access to a Neo MCP server that stores and serves a persistent knowledge graph. Use it to index codebase knowledge and answer questions with full project context.

## When to use

- When the user asks to "index this project", "add this to my knowledge base", or "save this pattern"
- When you want to check what's already known about the project before answering questions
- When the user asks "how do we do X in this project" — check Neo first

## MCP Tools Available

### Reading
- `get_overview` — See what's in the knowledge graph
- `search(query, source?, type?, tags?)` — Full-text search
- `get_node(id)` — Get full content of a knowledge node
- `get_related(id)` — Get connected nodes
- `how_to(topic, source?)` — Find patterns/conventions for a task

### Writing
- `add_knowledge(type, title, content, tags?, source?, source_meta?, related_to?)` — Add a knowledge node
- `update_knowledge(id, title?, content?, tags?)` — Update existing node
- `delete_knowledge(id)` — Delete a node
- `add_url(url, title?, tags?)` — Fetch and index a URL

## How to Index a Repository

When the user asks to index a project, follow this process:

### Step 1: Understand the codebase
Read the project structure, package.json/build files, and key source files to understand:
- Tech stack (languages, frameworks, databases)
- Architecture (monolith, microservices, MVC, etc.)
- Directory structure and its purpose

### Step 2: Extract and send knowledge

For each finding, call `add_knowledge` with the appropriate type. Always include:
- **Real code examples** from the actual codebase (not generic examples)
- **The repo name** in the `source` field (e.g., `github:org/repo-name`)
- **Stack details** in `source_meta` (e.g., `{"language": "typescript", "framework": "nextjs", "year": 2024}`)

#### Types of knowledge to extract:

**architecture** — One node for the overall architecture:
- Tech stack and frameworks
- High-level structure
- Key entry points
- Data flow

**module** — One node per major module/directory:
- Purpose of the module
- Key files
- Public API / exports
- Dependencies (what it uses and who uses it)

**pattern** — One node per coding pattern found:
- Name of the pattern
- Where it's used (which files/modules)
- A REAL code example from the repo (copy actual code, not paraphrased)
- When to use this pattern vs alternatives

**convention** — One node for coding conventions:
- Naming conventions (files, functions, classes, variables)
- Import ordering
- Error handling approach
- Test file naming and structure
- Git commit message format (if visible)

**decision** — One node per architectural decision visible in the code:
- What was decided
- Evidence from the code (comments, structure, patterns)
- Trade-offs visible

### Step 3: Create relationships

When calling `add_knowledge`, use the `related_to` parameter to create edges:
- A module `depends_on` another module
- A pattern `implements` an architectural concept
- A convention `follows` a pattern
- Two modules in different repos doing the same thing: `same_concept`
- Old vs new approach: `evolved_from`

### Step 4: Confirm

After indexing, call `get_overview` and show the user a summary:
- How many nodes were created
- How many edges
- What modules/patterns were found

## Knowledge Node Types

| Type | When to use |
|---|---|
| `pattern` | A recurring code pattern with examples |
| `convention` | Naming, structure, or style conventions |
| `module` | A logical module, directory, or component |
| `architecture` | High-level system design |
| `decision` | An architectural or design decision |
| `concept` | A domain concept or idea |
| `note` | General notes or observations |
| `reference` | External links, articles, docs |

## Edge Relationships

| Relationship | Meaning |
|---|---|
| `uses` | A uses B |
| `follows` | A follows pattern/convention B |
| `contains` | A contains B (parent-child) |
| `depends_on` | A depends on B |
| `related_to` | General relationship |
| `extends` | A extends/builds on B |
| `contradicts` | A contradicts B |
| `alternative_to` | A is an alternative to B |
| `same_concept` | Same concept in different contexts |
| `evolved_from` | A evolved from B |
| `implements` | A implements B |

## Tips

- Always use `source` to identify where knowledge came from — this prevents confusion when multiple repos are indexed
- Include the actual language/framework in `source_meta` so queries can be context-aware
- When the user asks "how do I..." — call `how_to()` first, then supplement with your own knowledge if needed
- After answering a complex question, consider saving the answer as a new knowledge node so it's available next time
