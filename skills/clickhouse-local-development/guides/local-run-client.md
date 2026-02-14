---
title: Run the ClickHouse Client
tags: [local, client, sql, files]
---

## Run the ClickHouse Client

Connect to a running local ClickHouse server and execute SQL from files. Queries can be sent as arguments (good for temporary queries for testing), or stored in files and piped in (good for creating tables, views, reusable production queries).

### Prerequisites

- chv CLI installed with at least one ClickHouse version (see `setup-install`)
- A local ClickHouse server running (see `local-run-server`)

### Steps

1. Connect interactively

```bash
chv run client
```

Opens an interactive ClickHouse client session connected to the local server.

2. Run a single SQL file with `--queries-file`

```bash
chv run client -- --queries-file clickhouse/tables/events.sql
```

Executes all statements in the file against the local server.

Alternative (equivalent to `--queries-file` but uses shell redirection.)

```bash
chv run client -- < clickhouse/tables/events.sql
```

3. Insert data from a CSV file

```bash
chv run client -- --query "INSERT INTO events FORMAT CSVWithNames" < data/seed.csv
```

4. Run test queries

```bash
chv run client -- --query "SELECT count() FROM events"
```

Run validation queries without using files.

### Options Reference

| Option | Description |
|--------|-------------|
| `--queries-file <path>` | Execute SQL statements from a file |
| `--query <sql>` | Execute a single SQL statement inline |
| `--multiquery` | Allow multiple semicolon-separated statements in `--query` |
| `--host <host>` | Server hostname (default: `localhost`) |
| `--port <port>` | Server TCP port (default: `9000`) |
| `--format <fmt>` | Output format (e.g. `TabSeparated`, `CSV`, `JSON`, `Pretty`) |

### Example

`chv init` scaffolds the following directory structure (see `local-init`). Add your SQL files to these directories:

```
clickhouse/
├── tables/
│   └── events.sql
├── materialized_views/
│   └── events_daily.sql
└── queries/
    └── top_events.sql
```

**`clickhouse/tables/events.sql`**

```sql
CREATE TABLE IF NOT EXISTS events (
    event_date Date,
    event_type String,
    user_id UInt64,
    properties String
) ENGINE = MergeTree()
ORDER BY (event_type, event_date);
```

**`clickhouse/materialized_views/events_daily.sql`**

```sql
CREATE MATERIALIZED VIEW IF NOT EXISTS events_daily
ENGINE = SummingMergeTree()
ORDER BY (event_date, event_type)
AS SELECT
    event_date,
    event_type,
    count() AS event_count
FROM events
GROUP BY event_date, event_type;
```

**`clickhouse/queries/top_events.sql`**

```sql
SELECT
    event_type,
    count() AS total
FROM events
GROUP BY event_type
ORDER BY total DESC
LIMIT 10;
```

**Run the full workflow:**

```bash
# Start the server
chv run server &

# Create the table
chv run client -- --queries-file clickhouse/tables/events.sql

# Create the materialized view
chv run client -- --queries-file clickhouse/materialized_views/events_daily.sql

# Seed data from CSV
chv run client -- --query "INSERT INTO events FORMAT CSVWithNames" < data/seed.csv

# Validate seed data was inserted
chv run client -- --query "SELECT count() FROM events"

# Run a query
chv run client -- --queries-file clickhouse/queries/top_events.sql
```
