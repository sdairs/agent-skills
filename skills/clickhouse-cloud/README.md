# ClickHouse Cloud

Agent skill providing step-by-step guides for working with ClickHouse Cloud using the chv CLI.

## Installation

```bash
npx skills add clickhouse/agent-skills
```

## What's Included

**17 guides** organized by section:

| Prefix | Count | Coverage |
|--------|-------|----------|
| `setup-*` | 2 | Install chv CLI, configure Cloud API credentials |
| `local-*` | 3 | Init projects, run server, run queries |
| `service-*` | 3 | Create, manage lifecycle, inspect services |
| `sizing-*` | 3 | Replicas, idle scaling, provider/region |
| `migrate-*` | 1 | Migrate local dev to cloud |
| `security-*` | 2 | IP allowlists, encryption |
| `backup-*` | 2 | List backups, restore from backup |

## Trigger Phrases

This skill activates when you:
- "Install chv" or "set up chv"
- "Create a ClickHouse Cloud service"
- "Run ClickHouse locally"
- "Migrate to ClickHouse Cloud"
- "Configure IP allowlist"
- "How do I size my ClickHouse service?"
- "Restore from backup"

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
tags: [cloud, category]
---

## Guide Title

Brief description.

### Prerequisites
### Steps
### Options Reference
### Example
```

Unlike the best-practices skill which uses incorrect/correct rule pairs, this skill uses procedural guides with bash examples.

## Related Documentation

- [ClickHouse Cloud](https://clickhouse.com/docs/en/cloud)
- [ClickHouse Cloud API](https://clickhouse.com/docs/en/cloud/manage/api)
