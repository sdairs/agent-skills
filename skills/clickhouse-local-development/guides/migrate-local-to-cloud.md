---
title: Migrate from Local Development to Cloud
tags: [local, migrate, cloud, deployment]
---

## Migrate from Local Development to Cloud

Deploy your local ClickHouse schema and data to a ClickHouse Cloud service. Since your SQL files are version-controlled (see `local-run-client`), migration is replaying the same files against the cloud endpoint.

### Prerequisites

- chv CLI installed (see `setup-install`)
- SQL files developed locally (see `local-run-client`)

### Steps

1. **Set up Cloud API credentials**

Create a ClickHouse Cloud account if you don't have one, then create an API key in the Cloud console. Run the interactive auth command in a separate terminal (see `setup-auth` in the `clickhouse-cloud` skill):

```bash
chv cloud auth
```

This stores credentials in `.clickhouse/` so all `chv cloud` commands authenticate automatically.

2. **Create a Cloud service**

See `service-create` in the `clickhouse-cloud` skill for full options:

```bash
chv cloud --json service create \
  --name my-project \
  --provider aws \
  --region us-east-1 \
  --tier development
```

Note the service ID, hostname, and password from the output.

3. **Get connection details** (if you need them later)

```bash
chv cloud --json service get <service-id>
```

4. **Apply table schemas to cloud**

```bash
chv run client -- \
  --host <cloud-hostname> \
  --port 9440 \
  --secure \
  --user default \
  --password <password> \
  --queries-file clickhouse/tables/events.sql
```

The same file you used with `chv run client` locally — just add the cloud connection flags.

5. **Apply materialized views**

```bash
chv run client -- \
  --host <cloud-hostname> \
  --port 9440 \
  --secure \
  --user default \
  --password <password> \
  --queries-file clickhouse/materialized_views/events_daily.sql
```

6. **Load seed data**

```bash
chv run client -- \
  --host <cloud-hostname> \
  --port 9440 \
  --secure \
  --user default \
  --password <password> \
  --query "INSERT INTO events FORMAT CSVWithNames" < data/seed.csv
```

7. **Validate**

```bash
chv run client -- \
  --host <cloud-hostname> \
  --port 9440 \
  --secure \
  --user default \
  --password <password> \
  --query "SELECT count() FROM events"
```

### Tips

- Run files in dependency order: tables first, then materialized views, then any seed data
- For large datasets, use Native format instead of CSV for better performance
- Your SQL files are the source of truth — no need to export from the local server

### Example

Given this project structure (from `local-run-client`):

```
clickhouse/
├── tables/
│   └── events.sql
├── materialized_views/
│   └── events_daily.sql
└── queries/
    └── top_events.sql
data/
└── seed.csv
```

```bash
# Set up Cloud credentials (interactive — run in a separate terminal)
chv cloud auth

# Create a service
chv cloud --json service create \
  --name my-project \
  --provider aws \
  --region us-east-1 \
  --tier development

# Set connection variables from the service create output
CH_HOST=xyz.clickhouse.cloud
CH_ARGS="--host $CH_HOST --port 9440 --secure --user default --password mypassword"

# Apply schemas
chv run client -- $CH_ARGS --queries-file clickhouse/tables/events.sql
chv run client -- $CH_ARGS --queries-file clickhouse/materialized_views/events_daily.sql

# Load data
chv run client -- $CH_ARGS --query "INSERT INTO events FORMAT CSVWithNames" < data/seed.csv

# Validate
chv run client -- $CH_ARGS --query "SELECT count() FROM events"

# Run a query
chv run client -- $CH_ARGS --queries-file clickhouse/queries/top_events.sql
```

Reference: [ClickHouse Cloud API](https://clickhouse.com/docs/en/cloud/manage/api)
