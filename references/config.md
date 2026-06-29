# config.md — Topic Wiki Configuration

## Location

**`config.md` is a per-topic file.** It lives at the root of a specific topic sub-wiki:

```
HUB/topics/<slug>/config.md
```

For example:
- `HUB/topics/ai-research/config.md`
- `HUB/topics/dementia/config.md`
- `HUB/topics/quantum-computing/config.md`

For local wikis (`--local` mode), it lives at `.wiki/config.md`.

**It does NOT belong at the hub level.** If `config.md` is found at `HUB/config.md` during a structure check or lint, that is a legacy layout artifact — warn the user, do not delete, and suggest running `lint --fix` to migrate it into the appropriate topic wiki. See `linting.md` § C17.

## Purpose

`config.md` defines the identity, scope, and conventions for **one topic wiki**. It is the primary source for:

- The wiki's display title (used in boot/resume briefings, status reports, and audit headers)
- A human-readable description of what this topic covers
- Creation date for provenance
- Freshness threshold for lint/article quality checks
- Any topic-specific conventions beyond the skill defaults

## Format

```markdown
---
title: "Wiki Title"
description: "What this topic wiki is about"
created: YYYY-MM-DD
freshness_threshold: 70
---

# Wiki Configuration

## Scope

[What topics this wiki covers — one or two paragraphs]

## Conventions

[Any topic-specific conventions beyond skill defaults. For example:
- Preferred tag taxonomy
- Custom article categories
- Source attribution rules
]
```

## Frontmatter Fields

| Field | Required | Type | Description |
|-------|----------|------|-------------|
| `title` | Yes | string | Display name for this topic wiki. Used in boot/resume identity lines (`<title> booted from <path>`). |
| `description` | Yes | string | One-sentence summary of what this topic covers. |
| `created` | Yes | date | When this topic wiki was initialized (`YYYY-MM-DD`). |
| `freshness_threshold` | No | number (0–100) | Articles scoring below this threshold are flagged by lint. Default: 70. |

## Usage

### Boot / Resume Identity

When giving a boot, resume, or "where you left off" briefing, resolve the wiki name in this order:

1. **`config.md` title frontmatter** — if the file exists and has a `title` field, use it
2. **Parent directory name** — for local `.wiki/` projects, fall back to the parent directory name
3. **Topic slug** — for `HUB/topics/<slug>/`, fall back to the slug

Example identity line: `AI Research booted from ~/wiki/topics/ai-research`

### Lint & Audit

- `freshness_threshold` from `config.md` overrides the default 70 for article freshness scoring
- The `Conventions` section is consulted during lint to validate topic-specific rules
- Audit reads `config.md` to verify wiki identity matches registry entries

### Compilation

- Compilation agents may read `config.md` to understand the topic scope and avoid synthesizing off-topic content
- The `Conventions` section guides article formatting, tag normalization, and linking style

## Missing config.md

If a topic wiki lacks `config.md`:

- **Boot/resume**: Fall back to the topic slug as the wiki name
- **Lint**: Report as a suggestion (not a warning) — recommend creating one via `lint --fix`
- **Compile**: Proceed normally; scope is inferred from existing content

The `lint --fix` workflow can auto-generate a minimal `config.md` with `title` derived from the slug, `description` inferred from existing articles, and `created` set to the oldest source/article date in the wiki.

## Multiple Topics

Each topic wiki has its own `config.md`. They are independent — there is no shared or inherited configuration. If two topics need the same conventions, each must declare them in its own `config.md`.
