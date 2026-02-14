---
title: List and Inspect Backups
tags: [cloud, backup, list, inspect]
---

## List and Inspect Backups

View available backups for a ClickHouse Cloud service and get details about a specific backup. ClickHouse Cloud automatically creates backups for your services.

### Prerequisites

- chv CLI installed (see the `clickhouse-local-development` skill)
- Cloud API credentials configured (see `setup-auth`)
- An existing Cloud service (see `service-create`)

### Steps

1. **List all backups for a service**

```bash
chv cloud backup list <service-id>
```

2. **Get details for a specific backup**

```bash
chv cloud backup get <service-id> <backup-id>
```

3. **Get JSON output for scripting**

```bash
chv cloud --json backup list <service-id>
chv cloud --json backup get <service-id> <backup-id>
```

### Options Reference

| Command | Description |
|---------|-------------|
| `chv cloud backup list <service-id>` | List all backups for a service |
| `chv cloud backup get <service-id> <backup-id>` | Get details for a specific backup |
| `chv cloud --json backup list <service-id>` | JSON output for backup list |
| `chv cloud --json backup get <service-id> <backup-id>` | JSON output for backup details |

### Example

```bash
# Check backups for a service
chv cloud service list
chv cloud backup list abc-123-def
chv cloud backup get abc-123-def backup-789-xyz
```

Reference: [ClickHouse Cloud API](https://clickhouse.com/docs/en/cloud/manage/api)
