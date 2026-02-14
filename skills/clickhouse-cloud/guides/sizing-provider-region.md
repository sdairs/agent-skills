---
title: Choose Cloud Provider and Region
tags: [cloud, sizing, provider, region, aws, gcp, azure]
---

## Choose Cloud Provider and Region

Select the cloud provider and region when creating a ClickHouse Cloud service. Choose based on where your application runs and your compliance requirements.

### Prerequisites

- chv CLI installed (see the `clickhouse-local-development` skill)
- Cloud API credentials configured (see `setup-auth`)

### Steps

1. **Create a service on AWS (default)**

```bash
chv cloud service create --name my-service \
  --provider aws \
  --region us-east-1
```

2. **Create a service on GCP**

```bash
chv cloud service create --name my-service \
  --provider gcp \
  --region us-central1
```

3. **Create a service on Azure**

```bash
chv cloud service create --name my-service \
  --provider azure \
  --region eastus2
```

### Provider Selection Guidelines

| Factor | AWS | GCP | Azure |
|--------|-----|-----|-------|
| Default | Yes | No | No |
| Region coverage | Widest | Wide | Wide |
| Choose when | App runs on AWS, or no strong preference | App runs on GCP | App runs on Azure |

### Key Principle

Deploy ClickHouse in the **same provider and region** as your application to minimize latency and avoid cross-cloud data transfer costs.

### Options Reference

| Option | Description |
|--------|-------------|
| `--provider` | Cloud provider: `aws`, `gcp`, `azure` (default: `aws`) |
| `--region` | Provider-specific region string (default: `us-east-1`) |

### Example

```bash
# GCP service in Europe
chv cloud service create --name eu-analytics \
  --provider gcp \
  --region europe-west1 \
  --num-replicas 2 \
  --min-replica-memory-gb 16 \
  --max-replica-memory-gb 64
```

Reference: [ClickHouse Cloud API](https://clickhouse.com/docs/en/cloud/manage/api)
