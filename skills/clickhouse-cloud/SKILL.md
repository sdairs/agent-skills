---
name: clickhouse-cloud
description: MUST USE when working with ClickHouse Cloud services, the chv CLI, or local ClickHouse development. Contains 17 guides covering setup, local development, service management, sizing, migration, security, and backups. Always read relevant guide files and cite them in responses.
license: Apache-2.0
metadata:
  author: ClickHouse Inc
  version: "0.1.0"
---

# ClickHouse Cloud with chv

Step-by-step guides for working with ClickHouse Cloud using the chv CLI. Covers local development workflows, Cloud service creation and management, sizing and scaling, migrating local to cloud, security configuration, and backup management.

> **Official docs:** [ClickHouse Cloud](https://clickhouse.com/docs/en/cloud)

## IMPORTANT: How to Apply This Skill

**Before answering ClickHouse Cloud or chv questions, follow this priority order:**

1. **Check for applicable guides** in the `guides/` directory
2. **If a guide exists:** Follow it and cite it in your response using "Per `guide-name`..."
3. **If no guide exists:** Use the LLM's ClickHouse Cloud knowledge or search documentation
4. **If uncertain:** Use web search for current best practices
5. **Always cite your source:** guide name, "general ClickHouse Cloud guidance", or URL

**Why guides take priority:** The chv CLI is a prototype tool with specific command syntax. The guides encode the exact flags and workflows that have been verified against the CLI.

---

## Workflow Procedures

### For Local Development Setup

**Read these guide files in order:**

1. `guides/setup-install.md` — Install chv and a ClickHouse version
2. `guides/local-init.md` — Initialize a project-local data directory
3. `guides/local-run-server.md` — Start a local ClickHouse server
4. `guides/local-run-queries.md` — Run SQL queries with chv

### For Creating a Cloud Service

**Read these guide files in order:**

1. `guides/setup-auth.md` — Configure Cloud API credentials
2. `guides/sizing-provider-region.md` — Choose provider and region
3. `guides/sizing-replicas.md` — Choose replica count and memory
4. `guides/sizing-idle.md` — Configure idle scaling
5. `guides/service-create.md` — Create the service
6. `guides/security-ip-allowlist.md` — Configure IP access (if needed)
7. `guides/security-encryption.md` — Configure encryption (if needed)

### For Managing Existing Services

**Read the relevant guide:**

- `guides/service-inspect.md` — List and inspect services
- `guides/service-manage.md` — Start, stop, or delete services

### For Migrating Local to Cloud

**Read these guide files in order:**

1. `guides/service-create.md` — Create the cloud service
2. `guides/migrate-local-to-cloud.md` — Export and import data

### For Backup and Restore

**Read these guide files in order:**

1. `guides/backup-manage.md` — List and inspect backups
2. `guides/backup-restore.md` — Restore from a backup

---

## Guide Categories

| # | Section | Prefix | Guides | Description |
|---|---------|--------|--------|-------------|
| 1 | Setup | `setup-` | 2 | Install chv, configure API credentials |
| 2 | Local Development | `local-` | 3 | Init projects, run server, run queries |
| 3 | Service Management | `service-` | 3 | Create, manage lifecycle, inspect services |
| 4 | Sizing & Scaling | `sizing-` | 3 | Replicas, idle scaling, provider/region |
| 5 | Local to Cloud | `migrate-` | 1 | Migrate data from local dev to cloud |
| 6 | Security | `security-` | 2 | IP allowlists, encryption |
| 7 | Backups | `backup-` | 2 | List backups, restore from backup |

---

## Quick Reference

### Setup

- `setup-install` — Install chv CLI and manage ClickHouse versions
- `setup-auth` — Configure ClickHouse Cloud API credentials

### Local Development

- `local-init` — Initialize project-local ClickHouse data directory
- `local-run-server` — Run a local ClickHouse server
- `local-run-queries` — Run SQL queries with `chv run`

### Service Management

- `service-create` — Create a Cloud service with all options
- `service-manage` — Start, stop, and delete services
- `service-inspect` — List and inspect services

### Sizing & Scaling

- `sizing-replicas` — Configure replica count and memory
- `sizing-idle` — Idle scaling and timeout configuration
- `sizing-provider-region` — Choose cloud provider and region

### Local to Cloud

- `migrate-local-to-cloud` — Export local data and import to cloud

### Security

- `security-ip-allowlist` — Configure IP access rules
- `security-encryption` — Disk encryption and compliance options

### Backups

- `backup-manage` — List and inspect backups
- `backup-restore` — Restore a service from backup

---

## When to Apply

This skill activates when you encounter:

- Installing or using `chv` CLI
- Setting up a local ClickHouse development environment
- Creating ClickHouse Cloud services
- Choosing cloud provider, region, or sizing
- Starting, stopping, or deleting Cloud services
- Migrating from local ClickHouse to Cloud
- Configuring IP allowlists or encryption
- Managing or restoring from backups
- Questions about `chv run`, `chv cloud`, or `chv install`

---

## Full Compiled Document

For the complete guide with all content expanded inline: `AGENTS.md`

Use `AGENTS.md` when you need to reference multiple guides quickly without reading individual files.
