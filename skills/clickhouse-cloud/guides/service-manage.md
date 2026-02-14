---
title: Start, Stop, and Delete Cloud Services
tags: [cloud, service, manage, lifecycle]
---

## Start, Stop, and Delete Cloud Services

Manage the lifecycle of ClickHouse Cloud services — start stopped services, stop running services, and delete services that are no longer needed.

### Prerequisites

- chv CLI installed (see the `clickhouse-local-development` skill)
- Cloud API credentials configured (see `setup-auth`)
- At least one existing service (see `service-create`)

### Steps

1. **List services to get the service ID**

```bash
chv cloud service list
```

2. **Stop a running service**

```bash
chv cloud service stop <service-id>
```

Stopping a service pauses compute but retains data. You are not billed for compute while stopped.

3. **Start a stopped service**

```bash
chv cloud service start <service-id>
```

4. **Delete a service**

```bash
chv cloud service delete <service-id>
```

> **Warning:** Deleting a service is permanent. All data is lost unless you have a backup.

### Options Reference

| Command | Description |
|---------|-------------|
| `chv cloud service start <id>` | Resume a stopped service |
| `chv cloud service stop <id>` | Pause a running service |
| `chv cloud service delete <id>` | Permanently delete a service |

### Example

```bash
# Stop a service for maintenance
chv cloud service list
chv cloud service stop abc-123-def

# Restart it later
chv cloud service start abc-123-def
```

Reference: [ClickHouse Cloud API](https://clickhouse.com/docs/en/cloud/manage/api)
