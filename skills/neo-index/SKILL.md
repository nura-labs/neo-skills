---
name: neo-index
description: Index a codebase into Neo's knowledge graph. Use when the user says "index this project", "add this repo to Neo", or "analyze this codebase". Extracts patterns, conventions, architecture, and modules with proper relationships.
metadata:
  author: nura-labs
  version: "3.0"
---

# Neo — Index a Project

Extract knowledge from a codebase and save it to Neo with proper relationships.

## Workflow

### Step 1: Check What Exists

Before analyzing anything:

```
get_overview()
search("project name")
```

Note existing nodes you can link to.

### Step 2: Analyze the Codebase

Read the project locally. Identify:
- Tech stack and architecture
- Directory structure and modules
- Recurring code patterns
- Naming and style conventions
- Key decisions visible in the code

### Step 3: Save Knowledge

For each finding, call `add_knowledge`. Use [[wikilinks]] in content to auto-create relationships.

```
add_knowledge(
  type: "pattern",
  title: "REST Endpoints",
  content: "All endpoints follow [[Clean Architecture]]...",
  source: "github:org/repo",
  tags: ["api", "rest"]
)
```

### Step 4: Link Across Projects

Use `link_knowledge` to connect across repos:

```
link_knowledge(
  source_title: "Auth Service",
  target_title: "JWT Pattern",
  relationship: "implements"
)
```

### Step 5: Confirm

Call `get_overview()` and show the user what was indexed.

## Naming Rules

Titles must be **short and clear**:

```
GOOD                          BAD
"Auth Pattern"                "Finny - Authentication & Authorization Pattern"
"REST Conventions"            "Neo — API & Code Conventions for REST Endpoints"
"User Service"                "apps/auth_login — Auth, usuario custom, OAuth y RBAC"
"Database Layer"              "Finny - Database & Query Pattern (PostgreSQL)"
```

Rules:
- Max 4 words
- No project prefix (use `source` field for the repo name)
- No descriptions in title (that goes in `content`)
- No special characters: —, &, /

## Node Types

### Knowledge (how things work)

| Type | Use for | Example |
|------|---------|---------|
| `pattern` | Recurring code solution | "Repository Pattern" |
| `convention` | Team standard | "API Naming Rules" |
| `architecture` | System design | "Event-Driven Architecture" |
| `decision` | Why X over Y | "PostgreSQL over MongoDB" |
| `concept` | Mental model | "Clean Architecture" |
| `workflow` | Step-by-step process | "Deploy to Production" |
| `snippet` | Reusable code | "Auth Middleware Setup" |

### Structure (what things are)

| Type | Use for | Example |
|------|---------|---------|
| `module` | Code module/package | "User Service" |
| `api` | API endpoint/contract | "REST Endpoints" |
| `service` | External service | "Stripe Integration" |
| `config` | Configuration | "Environment Setup" |

### Entities (who/what is involved)

| Type | Use for | Example |
|------|---------|---------|
| `person` | Team member | "Oscar Morales" |
| `project` | Active project | "Neo Platform" |
| `team` | Team/org unit | "Backend Team" |
| `tool` | Technology/library | "Drizzle ORM" |

### Reference (external knowledge)

| Type | Use for | Example |
|------|---------|---------|
| `reference` | Article/doc/link | "Vertex AI Docs" |
| `research` | Analysis/comparison | "ORM Comparison" |
| `note` | Anything else | "Meeting Notes" |

## Edge Relationships

| Relationship | Reads as |
|---|---|
| `uses` | A uses B |
| `follows` | A follows B's pattern |
| `contains` | A contains B |
| `depends_on` | A depends on B |
| `related_to` | General connection |
| `extends` | A builds on B |
| `contradicts` | A contradicts B |
| `alternative_to` | A is alternative to B |
| `same_concept` | Same thing, different context |
| `evolved_from` | A replaced B |
| `implements` | A implements B |

## What to Extract

For a typical project, create roughly:
- 1 `architecture` node (overview)
- 1 node per major `module`
- 1 node per recurring `pattern` (with real code examples)
- 1 node per important `convention`
- Key `decision` nodes (why the team chose X)

Every node MUST have at least one relationship. Use [[wikilinks]] in content — Neo auto-creates the edges.
