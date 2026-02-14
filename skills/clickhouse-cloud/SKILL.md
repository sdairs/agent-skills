---
name: clickhouse-cloud
description: MUST USE when working with ClickHouse Cloud services, API credentials, service management, sizing, security, or backups. Contains 11 guides covering cloud setup, service lifecycle, scaling, security, and backup operations. Always read relevant guide files and cite them in responses.
license: Apache-2.0
metadata:
  author: ClickHouse Inc
  version: "0.2.0"
---

# ClickHouse Cloud with chv

Step-by-step guides for managing ClickHouse Cloud services using the chv CLI. Covers Cloud API credentials, service creation and lifecycle management, sizing and scaling, security configuration, and backup management.

> **Official docs:** [ClickHouse Cloud](https://clickhouse.com/docs/en/cloud)
>
> **For local development** (installing chv, running ClickHouse locally, migrating to cloud): See the `clickhouse-local-development` skill.

## IMPORTANT: How to Apply This Skill

**Before answering ClickHouse Cloud questions, follow this priority order:**

1. **Check for applicable guides** in the `guides/` directory
2. **If a guide exists:** Follow it and cite it in your response using "Per `guide-name`..."
3. **If no guide exists:** Use the LLM's ClickHouse Cloud knowledge or search documentation
4. **If uncertain:** Use web search for current best practices
5. **Always cite your source:** guide name, "general ClickHouse Cloud guidance", or URL

**Why guides take priority:** The chv CLI is a prototype tool with specific command syntax. The guides encode the exact flags and workflows that have been verified against the CLI.

---

## Workflow Procedures

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

### For Backup and Restore

**Read these guide files in order:**

1. `guides/backup-manage.md` — List and inspect backups
2. `guides/backup-restore.md` — Restore from a backup

### For Local Development or Migration

See the `clickhouse-local-development` skill for installing chv, running ClickHouse locally, and migrating data to cloud.

---

## Guide Categories

| # | Section | Prefix | Guides | Description |
|---|---------|--------|--------|-------------|
| 1 | Setup | `setup-` | 1 | Configure Cloud API credentials |
| 2 | Service Management | `service-` | 3 | Create, manage lifecycle, inspect services |
| 3 | Sizing & Scaling | `sizing-` | 3 | Replicas, idle scaling, provider/region |
| 4 | Security | `security-` | 2 | IP allowlists, encryption |
| 5 | Backups | `backup-` | 2 | List backups, restore from backup |

---

## Quick Reference

### Setup

- `setup-auth` — Configure ClickHouse Cloud API credentials

### Service Management

- `service-create` — Create a Cloud service with all options
- `service-manage` — Start, stop, and delete services
- `service-inspect` — List and inspect services

### Sizing & Scaling

- `sizing-replicas` — Configure replica count and memory
- `sizing-idle` — Idle scaling and timeout configuration
- `sizing-provider-region` — Choose cloud provider and region

### Security

- `security-ip-allowlist` — Configure IP access rules
- `security-encryption` — Disk encryption and compliance options

### Backups

- `backup-manage` — List and inspect backups
- `backup-restore` — Restore a service from backup

---

## When to Apply

This skill activates when you encounter:

- Configuring ClickHouse Cloud API credentials
- Creating ClickHouse Cloud services
- Choosing cloud provider, region, or sizing
- Starting, stopping, or deleting Cloud services
- Listing or inspecting Cloud services
- Configuring IP allowlists or encryption
- Managing or restoring from backups
- Questions about `chv cloud` commands

---

## Related Skills

- **`clickhouse-local-development`** — For installing chv, local ClickHouse development, and migrating to cloud
- **`clickhouse-best-practices`** — For ClickHouse schema design, query optimization, and data ingestion best practices

---

## Full Compiled Document

For the complete guide with all content expanded inline: `AGENTS.md`

Use `AGENTS.md` when you need to reference multiple guides quickly without reading individual files.
