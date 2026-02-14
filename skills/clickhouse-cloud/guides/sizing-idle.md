---
title: Idle Scaling and Timeouts
tags: [cloud, sizing, idle, scaling, timeout]
---

## Idle Scaling and Timeouts

Configure whether a ClickHouse Cloud service can scale to zero when idle and how long it waits before scaling down. Idle scaling reduces costs for non-production workloads.

### Prerequisites

- chv CLI installed (see the `clickhouse-local-development` skill)
- Cloud API credentials configured (see `setup-auth`)

### Steps

1. **Create a service with idle scaling enabled (default)**

```bash
chv cloud service create --name dev-service \
  --idle-scaling true \
  --idle-timeout-minutes 10
```

When no queries are running for the timeout period, the service scales to zero. It automatically resumes on the next connection (with a brief cold-start delay).

2. **Create a service with idle scaling disabled**

```bash
chv cloud service create --name prod-service \
  --idle-scaling false
```

The service stays running at all times. Use this for production workloads that need consistent low-latency responses.

### When to Use Each Setting

| Setting | Use Case |
|---------|----------|
| `--idle-scaling true` (default) | Development, staging, infrequent queries, cost optimization |
| `--idle-scaling false` | Production, real-time dashboards, low-latency requirements |
| `--idle-timeout-minutes 5` | Aggressive cost savings, tolerant of cold starts |
| `--idle-timeout-minutes 30` | Bursty workloads with gaps between query batches |

### Options Reference

| Option | Description |
|--------|-------------|
| `--idle-scaling` | Allow scale to zero (default: `true`) |
| `--idle-timeout-minutes` | Minutes of idle time before scaling down (>= 5) |

### Example

```bash
# Cost-optimized dev service that scales to zero after 5 minutes
chv cloud service create --name dev-analytics \
  --idle-scaling true \
  --idle-timeout-minutes 5 \
  --min-replica-memory-gb 8 \
  --max-replica-memory-gb 8

# Always-on production service
chv cloud service create --name prod-analytics \
  --idle-scaling false \
  --num-replicas 3 \
  --min-replica-memory-gb 16 \
  --max-replica-memory-gb 64
```

Reference: [ClickHouse Cloud API](https://clickhouse.com/docs/en/cloud/manage/api)
