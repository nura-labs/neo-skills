---
name: neo-wikilinks
description: How to write Neo knowledge node content with [[wikilinks]] that auto-create graph connections. Use when creating or editing knowledge nodes in Neo, or when the user asks about linking nodes.
metadata:
  author: nura-labs
  version: "3.0"
---

# Neo — Wikilinks

Write `[[wikilinks]]` inside node content to auto-create edges in the knowledge graph. No manual edge creation needed.

## Syntax

```markdown
[[Node Title]]                    → creates "related_to" edge
[[Node Title|uses]]               → creates "uses" edge
[[Node Title|implements]]         → creates "implements" edge
[[Node Title#Section]]            → links to specific section
[[Node Title#Section|depends_on]] → section link with typed edge
```

## Example

```markdown
## REST Endpoints

All endpoints follow [[Clean Architecture|follows]].
Auth uses [[JWT Middleware|uses]] for token validation.
This replaces [[XML API|evolved_from]].

See [[Error Handling#HTTP Codes]] for status code conventions.
```

This auto-creates 3 edges:
```
REST Endpoints → follows → Clean Architecture
REST Endpoints → uses → JWT Middleware
REST Endpoints → evolved_from → XML API
```

## Valid Relationship Types

Use after the `|` pipe:

```
uses          → A uses B
follows       → A follows B's pattern
contains      → A contains B
depends_on    → A depends on B
related_to    → general (default if no | pipe)
extends       → A builds on B
contradicts   → A contradicts B
alternative_to → A is alternative to B
same_concept  → same thing, different context
evolved_from  → A replaced B
implements    → A implements B
```

## Rules

- The target must match an existing node title (resolved by slug)
- If the target doesn't exist, the wikilink is ignored (no phantom nodes)
- Wikilinks are re-parsed on every save — remove a wikilink to remove the edge
- You can also use `link_knowledge` tool for explicit connections without wikilinks

## Good Content Example

```markdown
## Auth Middleware

Validates JWT tokens on every request. Built on [[JWT Library|uses]].

Follows the [[Middleware Pattern|follows]] used across all services.
Part of [[Auth Module|contains]].

### Implementation
- Extracts token from Authorization header
- Validates signature and expiry
- Attaches user to request context

### When to use
Apply to all protected routes. See [[Route Guards]] for frontend equivalent.
```
