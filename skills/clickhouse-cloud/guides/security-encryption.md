---
title: Disk Encryption Options
tags: [cloud, security, encryption, tde]
---

## Disk Encryption Options

Configure customer-managed encryption keys or Transparent Data Encryption (TDE) for ClickHouse Cloud services to meet compliance and security requirements.

### Prerequisites

- chv CLI installed (see the `clickhouse-local-development` skill)
- Cloud API credentials configured (see `setup-auth`)
- For customer-managed keys: an encryption key and IAM role in your cloud provider

### Steps

1. **Create a service with customer-managed encryption key (AWS)**

```bash
chv cloud service create --name my-service \
  --provider aws \
  --encryption-key arn:aws:kms:us-east-1:123456789:key/abc-def \
  --encryption-role arn:aws:iam::123456789:role/clickhouse-encryption
```

2. **Create a service with Transparent Data Encryption**

```bash
chv cloud service create --name my-service \
  --enable-tde
```

3. **Create a service with compliance requirements**

```bash
chv cloud service create --name my-service \
  --compliance-type hipaa \
  --encryption-key arn:aws:kms:us-east-1:123456789:key/abc-def \
  --encryption-role arn:aws:iam::123456789:role/clickhouse-encryption
```

### Options Reference

| Option | Description |
|--------|-------------|
| `--encryption-key` | Customer disk encryption key (e.g., AWS KMS ARN) |
| `--encryption-role` | Role ARN for disk encryption (e.g., AWS IAM role) |
| `--enable-tde` | Enable Transparent Data Encryption |
| `--compliance-type` | Compliance standard: `hipaa`, `pci` |

### When to Use Each Option

| Option | Use Case |
|--------|----------|
| Default (no flags) | Standard encryption managed by ClickHouse Cloud |
| `--encryption-key` + `--encryption-role` | Customer-managed keys for regulatory requirements |
| `--enable-tde` | Additional encryption layer for data at rest |
| `--compliance-type` | HIPAA or PCI compliance requirements |

### Example

```bash
# HIPAA-compliant service with customer-managed encryption
chv cloud service create --name healthcare-analytics \
  --provider aws \
  --region us-east-1 \
  --compliance-type hipaa \
  --encryption-key arn:aws:kms:us-east-1:123456789:key/abc-def \
  --encryption-role arn:aws:iam::123456789:role/clickhouse-encryption \
  --ip-allow 10.0.0.0/8 \
  --idle-scaling false \
  --num-replicas 3
```

Reference: [ClickHouse Cloud API](https://clickhouse.com/docs/en/cloud/manage/api)
