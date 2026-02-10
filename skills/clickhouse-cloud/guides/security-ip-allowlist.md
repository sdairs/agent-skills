---
title: Configure IP Access Rules
tags: [cloud, security, ip, allowlist, firewall]
---

## Configure IP Access Rules

Restrict network access to a ClickHouse Cloud service by specifying allowed IP CIDR ranges. By default, services allow access from all IPs (`0.0.0.0/0`).

### Prerequisites

- chv CLI installed (see `setup-install`)
- Cloud API credentials configured (see `setup-auth`)

### Steps

1. **Create a service with specific IP allowlist**

```bash
chv cloud service create --name my-service \
  --ip-allow 10.0.0.0/8 \
  --ip-allow 192.168.1.0/24
```

The `--ip-allow` flag is repeatable — specify it multiple times to allow multiple CIDR ranges.

2. **Allow only a single IP**

```bash
chv cloud service create --name my-service \
  --ip-allow 203.0.113.42/32
```

3. **Allow all traffic (default behavior)**

If `--ip-allow` is not specified, the default is `0.0.0.0/0` (all IPs allowed).

### Options Reference

| Option | Description |
|--------|-------------|
| `--ip-allow` | IP CIDR to allow (repeatable, default: `0.0.0.0/0`) |

### Common Patterns

| Pattern | CIDR | Use Case |
|---------|------|----------|
| Single IP | `203.0.113.42/32` | Developer machine or bastion host |
| Private subnet | `10.0.0.0/8` | VPC/private network access |
| Office range | `198.51.100.0/24` | Office network |
| All traffic | `0.0.0.0/0` | Open access (default, not recommended for production) |

### Example

```bash
# Production service locked to VPC and office network
chv cloud service create --name prod-analytics \
  --provider aws \
  --region us-east-1 \
  --ip-allow 10.0.0.0/8 \
  --ip-allow 198.51.100.0/24 \
  --num-replicas 3
```

Reference: [ClickHouse Cloud API](https://clickhouse.com/docs/en/cloud/manage/api)
