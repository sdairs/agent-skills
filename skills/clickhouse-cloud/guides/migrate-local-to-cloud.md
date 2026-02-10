---
title: Migrate from Local Development to Cloud
tags: [cloud, migrate, local, deployment]
---

## Migrate from Local Development to Cloud

Move your schema and data from a local ClickHouse development environment (managed by chv) to a ClickHouse Cloud service.

### Prerequisites

- chv CLI installed (see `setup-install`)
- Cloud API credentials configured (see `setup-auth`)
- A local ClickHouse project with data (see `local-init`, `local-run-server`)
- A ClickHouse Cloud service created (see `service-create`)

### Steps

1. **Start your local server**

```bash
chv run server &
```

2. **Export your schema from the local server**

```bash
chv run client -- --query "SHOW CREATE TABLE my_table" > schema.sql
```

3. **Export data from the local server**

```bash
chv run client -- --query "SELECT * FROM my_table FORMAT CSVWithNames" > data.csv
```

4. **Get your cloud service connection details**

```bash
chv cloud --json service get <service-id>
```

Note the hostname, port, and credentials from the output.

5. **Apply the schema to the cloud service**

```bash
chv run client -- \
  --host <cloud-hostname> \
  --port 9440 \
  --secure \
  --user default \
  --password <password> \
  --query "$(cat schema.sql)"
```

6. **Load the data into the cloud service**

```bash
chv run client -- \
  --host <cloud-hostname> \
  --port 9440 \
  --secure \
  --user default \
  --password <password> \
  --query "INSERT INTO my_table FORMAT CSVWithNames" < data.csv
```

### Tips

- For large datasets, use Native format instead of CSV for better performance:
  ```bash
  chv run client -- --query "SELECT * FROM my_table FORMAT Native" > data.native
  ```
- Export and import one table at a time to manage memory usage
- Verify row counts after migration:
  ```bash
  chv run client -- --query "SELECT count() FROM my_table"  # local
  chv run client -- --host <cloud-hostname> --port 9440 --secure --user default --password <password> --query "SELECT count() FROM my_table"  # cloud
  ```

### Example

```bash
# Full migration workflow
chv run server &

# Export schema and data
chv run client -- --query "SHOW CREATE TABLE events" > events_schema.sql
chv run client -- --query "SELECT * FROM events FORMAT CSVWithNames" > events_data.csv

# Get cloud connection info
chv cloud --json service get abc-123-def

# Apply to cloud
chv run client -- \
  --host xyz.clickhouse.cloud \
  --port 9440 --secure \
  --user default --password mypassword \
  --query "$(cat events_schema.sql)"

chv run client -- \
  --host xyz.clickhouse.cloud \
  --port 9440 --secure \
  --user default --password mypassword \
  --query "INSERT INTO events FORMAT CSVWithNames" < events_data.csv
```

Reference: [ClickHouse Cloud API](https://clickhouse.com/docs/en/cloud/manage/api)
