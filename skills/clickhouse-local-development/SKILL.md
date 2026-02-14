---
name: clickhouse-local-development
description: MUST USE when installing chv, setting up local ClickHouse development, or running ClickHouse locally. Contains 5 guides covering chv CLI installation, local project initialization, running a local server, executing SQL from files, and migrating to cloud. Always read relevant guide files and cite them in responses.
license: Apache-2.0
metadata:
  author: ClickHouse Inc
  version: "0.1.0"
---

# ClickHouse Local Development with chv

Step-by-step guides for local ClickHouse development using the chv CLI. Covers installing and managing ClickHouse versions, initializing projects with a suggested SQL file structure, running a local server, executing SQL from files, and migrating to ClickHouse Cloud.

> **Official docs:** [ClickHouse](https://clickhouse.com/docs)

## IMPORTANT: How to Apply This Skill

**Before answering chv or local ClickHouse development questions, follow this priority order:**

1. **Check for applicable guides** in the `guides/` directory
2. **If a guide exists:** Follow it and cite it in your response using "Per `guide-name`..."
3. **If no guide exists:** Use the LLM's ClickHouse knowledge or search documentation
4. **If uncertain:** Use web search for current best practices
5. **Always cite your source:** guide name, "general ClickHouse guidance", or URL

**Why guides take priority:** The chv CLI is a prototype tool with specific command syntax. The guides encode the exact flags and workflows that have been verified against the CLI.

---

## Workflow Procedures

### For Local Development Setup

**Read these guide files in order:**

1. `guides/setup-install.md` — Install chv and a ClickHouse version
2. `guides/local-init.md` — Initialize a project-local data directory
3. `guides/local-run-server.md` — Start a local ClickHouse server
4. `guides/local-run-client.md` — Execute SQL files with the ClickHouse client

### For Migrating to Cloud

**Read these guide files in order:**

1. `setup-auth` (from `clickhouse-cloud` skill) — Create a Cloud account and configure API credentials
2. `service-create` (from `clickhouse-cloud` skill) — Create a Cloud service
3. `guides/migrate-local-to-cloud.md` — Deploy your SQL files and data to the Cloud service

---

## Guide Categories

| # | Section | Prefix | Guides | Description |
|---|---------|--------|--------|-------------|
| 1 | Setup | `setup-` | 1 | Install chv CLI and manage ClickHouse versions |
| 2 | Local Development | `local-` | 3 | Init projects, run server, run client |
| 3 | Migration | `migrate-` | 1 | Migrate data from local dev to cloud |

---

## Quick Reference

### Setup

- `setup-install` — Install chv CLI and manage ClickHouse versions

### Local Development

- `local-init` — Initialize project with data directory and SQL file structure
- `local-run-server` — Run a local ClickHouse server
- `local-run-client` — Execute SQL files with the ClickHouse client

### Migration

- `migrate-local-to-cloud` — Export local data and import to cloud

---

## When to Apply

This skill activates when you encounter:

- Installing or using `chv` CLI
- Managing ClickHouse versions locally
- Setting up a local ClickHouse development environment
- Initializing a ClickHouse project directory
- Running a local ClickHouse server
- Migrating from local ClickHouse to Cloud
- Executing SQL files with chv
- Questions about `chv run`, `chv install`, `chv init`, `chv use`, or `chv which`

---

## Related Skills

- **`clickhouse-cloud`** — For ClickHouse Cloud service management: API credentials, service creation, sizing, security, and backups
- **`clickhouse-best-practices`** — For ClickHouse schema design, query optimization, and data ingestion best practices

---

## Full Compiled Document

For the complete guide with all content expanded inline: `AGENTS.md`

Use `AGENTS.md` when you need to reference multiple guides quickly without reading individual files.
