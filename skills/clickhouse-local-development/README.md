# ClickHouse Local Development

Agent skill providing step-by-step guides for local ClickHouse development using the chv CLI.

## Installation

```bash
npx skills add clickhouse/agent-skills
```

## What's Included

**5 guides** organized by section:

| Prefix | Count | Coverage |
|--------|-------|----------|
| `setup-*` | 1 | Install chv CLI and manage ClickHouse versions |
| `local-*` | 3 | Init projects, run server, run client |
| `migrate-*` | 1 | Migrate local dev to cloud |

## Trigger Phrases

This skill activates when you:
- "Install chv" or "set up chv"
- "Run ClickHouse locally"
- "Initialize a ClickHouse project"
- "Migrate to ClickHouse Cloud"

## Files

| File | Purpose |
|------|---------|
| `SKILL.md` | Quick reference and workflow procedures |
| `AGENTS.md` | Complete guide reference (hand-written) |
| `guides/*.md` | Individual guide files |

## Guide Format

Each guide uses YAML frontmatter with a step-by-step format:

```markdown
---
title: Guide Title
tags: [local, category]
---

## Guide Title

Brief description.

### Prerequisites
### Steps
### Options Reference
### Example
```

Unlike the best-practices skill which uses incorrect/correct rule pairs, this skill uses procedural guides with bash examples.

## Related Skills

- **[clickhouse-cloud](../clickhouse-cloud/)** — ClickHouse Cloud service management: API credentials, service creation, sizing, security, and backups
- **[clickhouse-best-practices](../clickhouse-best-practices/)** — ClickHouse schema design, query optimization, and data ingestion best practices

## Related Documentation

- [ClickHouse](https://clickhouse.com/docs)
- [chv CLI](https://github.com/sdairs/chv)
