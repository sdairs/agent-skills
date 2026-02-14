---
title: Restore a Service from Backup
tags: [cloud, backup, restore]
---

## Restore a Service from Backup

Create a new ClickHouse Cloud service by restoring from an existing backup. This creates a new service with the data from the backup — it does not overwrite the original.

### Prerequisites

- chv CLI installed (see the `clickhouse-local-development` skill)
- Cloud API credentials configured (see `setup-auth`)
- A backup ID (see `backup-manage`)

### Steps

1. **List available backups to find the backup ID**

```bash
chv cloud backup list <service-id>
```

2. **Create a new service from the backup**

```bash
chv cloud service create --name restored-service --backup-id <backup-uuid>
```

3. **Verify the restored service**

```bash
chv cloud service list
chv cloud service get <new-service-id>
```

### Options Reference

| Option | Description |
|--------|-------------|
| `--backup-id` | Backup UUID to restore from |
| `--name` | Name for the new restored service (required) |

### Example

```bash
# Full restore workflow
chv cloud backup list abc-123-def
# Pick a backup ID from the output

chv cloud service create --name restored-analytics \
  --backup-id backup-789-xyz

# Verify the new service
chv cloud service list
```

Reference: [ClickHouse Cloud API](https://clickhouse.com/docs/en/cloud/manage/api)
