# ClickHouse Cloud with chv — Complete Guide

Step-by-step guides for managing ClickHouse Cloud services using the chv CLI. 11 guides across 5 sections covering setup, service management, sizing, security, and backups.

> **Official docs:** [ClickHouse Cloud](https://clickhouse.com/docs/en/cloud)
>
> **For local development** (installing chv, running ClickHouse locally, migrating to cloud): See the `clickhouse-local-development` skill.

---

## Table of Contents

1. [Setup](#1-setup)
   - [Configure Cloud API Credentials](#configure-cloud-api-credentials)
2. [Service Management](#2-service-management)
   - [Create a ClickHouse Cloud Service](#create-a-clickhouse-cloud-service)
   - [Start, Stop, and Delete Cloud Services](#start-stop-and-delete-cloud-services)
   - [List and Inspect Cloud Services](#list-and-inspect-cloud-services)
3. [Sizing & Scaling](#3-sizing--scaling)
   - [Configure Replica Count and Memory](#configure-replica-count-and-memory)
   - [Idle Scaling and Timeouts](#idle-scaling-and-timeouts)
   - [Choose Cloud Provider and Region](#choose-cloud-provider-and-region)
4. [Security](#4-security)
   - [Configure IP Access Rules](#configure-ip-access-rules)
   - [Disk Encryption Options](#disk-encryption-options)
5. [Backups](#5-backups)
   - [List and Inspect Backups](#list-and-inspect-backups)
   - [Restore a Service from Backup](#restore-a-service-from-backup)

---

## 1. Setup

### Configure Cloud API Credentials

Set up authentication for ClickHouse Cloud API commands. Required before using any `chv cloud` command.

**Prerequisites:** chv CLI installed (see the `clickhouse-local-development` skill), a ClickHouse Cloud account with an API key.

**Steps:**

1. **Create an API key** in the ClickHouse Cloud console under your organization settings.

2. **Ask the user to run `chv cloud auth` in a separate terminal** (interactive — the agent cannot run this directly).

```bash
chv cloud auth
```

This stores credentials in a file at `.clickhouse/`. Once done, all `chv cloud` commands authenticate automatically.

3. **Verify access**

```bash
chv cloud org list
```

Alternatively, pass credentials inline:

```bash
chv cloud --api-key KEY --api-secret SECRET org list
```

Reference: [ClickHouse Cloud API Keys](https://clickhouse.com/docs/en/cloud/manage/api)

---

## 2. Service Management

### Create a ClickHouse Cloud Service

Create a new ClickHouse Cloud service with desired configuration.

**Prerequisites:** chv CLI installed, Cloud API credentials configured.

**Steps:**

1. **Create a minimal service (defaults: AWS, us-east-1)**

```bash
chv cloud service create --name my-service
```

2. **Create with provider, region, and scaling**

```bash
chv cloud service create --name my-service \
  --provider aws \
  --region us-east-1 \
  --min-replica-memory-gb 8 \
  --max-replica-memory-gb 32 \
  --num-replicas 2
```

3. **Create with IP allowlist**

```bash
chv cloud service create --name my-service \
  --ip-allow 10.0.0.0/8 \
  --ip-allow 192.168.1.0/24
```

4. **Create from a backup**

```bash
chv cloud service create --name restored-service --backup-id <backup-uuid>
```

5. **Create with a release channel**

```bash
chv cloud service create --name my-service --release-channel fast
```

**Full options reference:**

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

Reference: [ClickHouse Cloud API](https://clickhouse.com/docs/en/cloud/manage/api)

---

### Start, Stop, and Delete Cloud Services

Manage the lifecycle of ClickHouse Cloud services.

**Prerequisites:** chv CLI installed, Cloud API credentials configured, an existing service.

**Steps:**

1. **List services to get the service ID**

```bash
chv cloud service list
```

2. **Stop a running service**

```bash
chv cloud service stop <service-id>
```

Stopping pauses compute but retains data. No compute billing while stopped.

3. **Start a stopped service**

```bash
chv cloud service start <service-id>
```

4. **Delete a service**

```bash
chv cloud service delete <service-id>
```

> **Warning:** Deleting a service is permanent. All data is lost unless you have a backup.

---

### List and Inspect Cloud Services

View all services and get detailed information about a specific service.

**Prerequisites:** chv CLI installed, Cloud API credentials configured.

**Steps:**

1. **List all services**

```bash
chv cloud service list
```

2. **Get details for a specific service**

```bash
chv cloud service get <service-id>
```

3. **Get JSON output for scripting**

```bash
chv cloud --json service list
chv cloud --json service get <service-id>
```

4. **List and inspect organizations**

```bash
chv cloud org list
chv cloud org get <org-id>
```

---

## 3. Sizing & Scaling

### Configure Replica Count and Memory

Set the number of replicas and memory allocation per replica. Memory values must be multiples of 4 GB.

**Steps:**

```bash
chv cloud service create --name my-service \
  --num-replicas 3 \
  --min-replica-memory-gb 16 \
  --max-replica-memory-gb 64
```

**Sizing guidelines:**

| Workload | Replicas | Min Memory | Max Memory |
|----------|----------|------------|------------|
| Development/testing | 1 | 8 GB | 8 GB |
| Small production | 2 | 8 GB | 32 GB |
| Medium production | 3 | 16 GB | 64 GB |
| Large production | 3+ | 32 GB | 128 GB+ |

**Options:**

| Option | Description |
|--------|-------------|
| `--num-replicas` | Number of replicas (1–20) |
| `--min-replica-memory-gb` | Min memory per replica in GB (8–356, multiple of 4) |
| `--max-replica-memory-gb` | Max memory per replica in GB (8–356, multiple of 4) |

---

### Idle Scaling and Timeouts

Configure whether a service can scale to zero when idle.

**Steps:**

1. **Enable idle scaling (default)**

```bash
chv cloud service create --name dev-service \
  --idle-scaling true \
  --idle-timeout-minutes 10
```

2. **Disable idle scaling for production**

```bash
chv cloud service create --name prod-service \
  --idle-scaling false
```

**When to use each setting:**

| Setting | Use Case |
|---------|----------|
| `--idle-scaling true` (default) | Development, staging, infrequent queries, cost optimization |
| `--idle-scaling false` | Production, real-time dashboards, low-latency requirements |
| `--idle-timeout-minutes 5` | Aggressive cost savings, tolerant of cold starts |
| `--idle-timeout-minutes 30` | Bursty workloads with gaps between query batches |

**Options:**

| Option | Description |
|--------|-------------|
| `--idle-scaling` | Allow scale to zero (default: `true`) |
| `--idle-timeout-minutes` | Minutes of idle time before scaling down (>= 5) |

---

### Choose Cloud Provider and Region

Select the cloud provider and region based on where your application runs.

**Steps:**

```bash
# AWS (default)
chv cloud service create --name my-service --provider aws --region us-east-1

# GCP
chv cloud service create --name my-service --provider gcp --region us-central1

# Azure
chv cloud service create --name my-service --provider azure --region eastus2
```

**Key principle:** Deploy ClickHouse in the **same provider and region** as your application to minimize latency and avoid cross-cloud data transfer costs.

**Options:**

| Option | Description |
|--------|-------------|
| `--provider` | Cloud provider: `aws`, `gcp`, `azure` (default: `aws`) |
| `--region` | Provider-specific region string (default: `us-east-1`) |

---

## 4. Security

### Configure IP Access Rules

Restrict network access by specifying allowed IP CIDR ranges. By default, services allow access from all IPs (`0.0.0.0/0`).

**Steps:**

```bash
chv cloud service create --name my-service \
  --ip-allow 10.0.0.0/8 \
  --ip-allow 192.168.1.0/24
```

The `--ip-allow` flag is repeatable. Specify it multiple times for multiple ranges.

**Common patterns:**

| Pattern | CIDR | Use Case |
|---------|------|----------|
| Single IP | `203.0.113.42/32` | Developer machine or bastion host |
| Private subnet | `10.0.0.0/8` | VPC/private network access |
| Office range | `198.51.100.0/24` | Office network |
| All traffic | `0.0.0.0/0` | Open access (default, not recommended for production) |

---

### Disk Encryption Options

Configure customer-managed encryption keys or Transparent Data Encryption (TDE).

**Steps:**

1. **Customer-managed encryption key (AWS)**

```bash
chv cloud service create --name my-service \
  --provider aws \
  --encryption-key arn:aws:kms:us-east-1:123456789:key/abc-def \
  --encryption-role arn:aws:iam::123456789:role/clickhouse-encryption
```

2. **Transparent Data Encryption**

```bash
chv cloud service create --name my-service \
  --enable-tde
```

3. **With compliance requirements**

```bash
chv cloud service create --name my-service \
  --compliance-type hipaa \
  --encryption-key arn:aws:kms:us-east-1:123456789:key/abc-def \
  --encryption-role arn:aws:iam::123456789:role/clickhouse-encryption
```

**Options:**

| Option | Description |
|--------|-------------|
| `--encryption-key` | Customer disk encryption key (e.g., AWS KMS ARN) |
| `--encryption-role` | Role ARN for disk encryption (e.g., AWS IAM role) |
| `--enable-tde` | Enable Transparent Data Encryption |
| `--compliance-type` | Compliance standard: `hipaa`, `pci` |

---

## 5. Backups

### List and Inspect Backups

View available backups for a ClickHouse Cloud service.

**Steps:**

1. **List all backups for a service**

```bash
chv cloud backup list <service-id>
```

2. **Get details for a specific backup**

```bash
chv cloud backup get <service-id> <backup-id>
```

3. **Get JSON output**

```bash
chv cloud --json backup list <service-id>
chv cloud --json backup get <service-id> <backup-id>
```

---

### Restore a Service from Backup

Create a new service by restoring from a backup. This creates a new service — it does not overwrite the original.

**Steps:**

1. **List available backups**

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

Reference: [ClickHouse Cloud API](https://clickhouse.com/docs/en/cloud/manage/api)
