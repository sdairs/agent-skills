---
title: Configure Replica Count and Memory
tags: [cloud, sizing, replicas, memory, scaling]
---

## Configure Replica Count and Memory

Set the number of replicas and memory allocation per replica when creating a ClickHouse Cloud service. Memory values must be multiples of 4 GB.

### Prerequisites

- chv CLI installed (see `setup-install`)
- Cloud API credentials configured (see `setup-auth`)

### Steps

1. **Create a service with specific replica and memory settings**

```bash
chv cloud service create --name my-service \
  --num-replicas 3 \
  --min-replica-memory-gb 16 \
  --max-replica-memory-gb 64
```

2. **Create a single-replica development service**

```bash
chv cloud service create --name dev-service \
  --num-replicas 1 \
  --min-replica-memory-gb 8 \
  --max-replica-memory-gb 8
```

### Sizing Guidelines

| Workload | Replicas | Min Memory | Max Memory |
|----------|----------|------------|------------|
| Development/testing | 1 | 8 GB | 8 GB |
| Small production | 2 | 8 GB | 32 GB |
| Medium production | 3 | 16 GB | 64 GB |
| Large production | 3+ | 32 GB | 128 GB+ |

### Options Reference

| Option | Description |
|--------|-------------|
| `--num-replicas` | Number of replicas (1–20) |
| `--min-replica-memory-gb` | Min memory per replica in GB (8–356, multiple of 4) |
| `--max-replica-memory-gb` | Max memory per replica in GB (8–356, multiple of 4) |

### Example

```bash
# High-availability production service
chv cloud service create --name prod-analytics \
  --provider aws \
  --region us-east-1 \
  --num-replicas 3 \
  --min-replica-memory-gb 32 \
  --max-replica-memory-gb 128
```

Reference: [ClickHouse Cloud API](https://clickhouse.com/docs/en/cloud/manage/api)
