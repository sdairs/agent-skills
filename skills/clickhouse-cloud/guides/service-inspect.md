---
title: List and Inspect Cloud Services
tags: [cloud, service, inspect, list]
---

## List and Inspect Cloud Services

View all services in your organization and get detailed information about a specific service including its status, endpoints, and configuration.

### Prerequisites

- chv CLI installed (see `setup-install`)
- Cloud API credentials configured (see `setup-auth`)

### Steps

1. **List all services**

```bash
chv cloud service list
```

2. **Get details for a specific service**

```bash
chv cloud service get <service-id>
```

3. **Get JSON output for scripting or AI agent consumption**

```bash
chv cloud --json service list
chv cloud --json service get <service-id>
```

4. **List organizations**

```bash
chv cloud org list
```

5. **Get organization details**

```bash
chv cloud org get <org-id>
```

### Options Reference

| Command | Description |
|---------|-------------|
| `chv cloud service list` | List all services |
| `chv cloud service get <id>` | Get service details |
| `chv cloud --json service list` | JSON output for service list |
| `chv cloud --json service get <id>` | JSON output for service details |
| `chv cloud org list` | List organizations |
| `chv cloud org get <id>` | Get organization details |

### Example

```bash
# Find a service and inspect it
chv cloud service list
chv cloud service get abc-123-def

# Get machine-readable output
chv cloud --json service get abc-123-def
```

Reference: [ClickHouse Cloud API](https://clickhouse.com/docs/en/cloud/manage/api)
