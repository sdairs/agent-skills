---
title: Create a ClickHouse Cloud Service
tags: [cloud, service, create]
---

## Create a ClickHouse Cloud Service

Create a new ClickHouse Cloud service with desired configuration for provider, region, scaling, and security.

### Prerequisites

- chv CLI installed (see `setup-install`)
- Cloud API credentials configured (see `setup-auth`)

### Steps

1. **Create a minimal service (defaults: AWS, us-east-1)**

```bash
chv cloud service create --name my-service
```

2. **Create a service with specific provider and region**

```bash
chv cloud service create --name my-service \
  --provider aws \
  --region us-east-1
```

3. **Create a service with scaling options**

```bash
chv cloud service create --name my-service \
  --provider aws \
  --region us-east-1 \
  --min-replica-memory-gb 8 \
  --max-replica-memory-gb 32 \
  --num-replicas 2
```

4. **Create a service with IP allowlist**

```bash
chv cloud service create --name my-service \
  --ip-allow 10.0.0.0/8 \
  --ip-allow 192.168.1.0/24
```

5. **Create a service from a backup**

```bash
chv cloud service create --name restored-service --backup-id <backup-uuid>
```

6. **Create a service with a specific release channel**

```bash
chv cloud service create --name my-service --release-channel fast
```

### Options Reference

| Option | Description |
|--------|-------------|
| `--name` | Service name (required) |
| `--provider` | Cloud provider: `aws`, `gcp`, `azure` (default: `aws`) |
| `--region` | Region (default: `us-east-1`) |
| `--min-replica-memory-gb` | Min memory per replica in GB (8–356, multiple of 4) |
| `--max-replica-memory-gb` | Max memory per replica in GB (8–356, multiple of 4) |
| `--num-replicas` | Number of replicas (1–20) |
| `--idle-scaling` | Allow scale to zero (default: `true`) |
| `--idle-timeout-minutes` | Min idle timeout in minutes (>= 5) |
| `--ip-allow` | IP CIDR to allow (repeatable, default: `0.0.0.0/0`) |
| `--backup-id` | Backup ID to restore from |
| `--release-channel` | Release channel: `slow`, `default`, `fast` |
| `--encryption-key` | Customer disk encryption key |
| `--encryption-role` | Role ARN for disk encryption |
| `--enable-tde` | Enable Transparent Data Encryption |
| `--readonly` | Make service read-only |
| `--byoc-id` | BYOC region ID |
| `--compliance-type` | Compliance: `hipaa`, `pci` |
| `--profile` | Instance profile (enterprise) |

### Example

```bash
# Production-ready service with scaling and security
chv cloud service create --name prod-analytics \
  --provider aws \
  --region us-east-1 \
  --min-replica-memory-gb 16 \
  --max-replica-memory-gb 64 \
  --num-replicas 3 \
  --idle-scaling false \
  --ip-allow 10.0.0.0/8
```

Reference: [ClickHouse Cloud API](https://clickhouse.com/docs/en/cloud/manage/api)
