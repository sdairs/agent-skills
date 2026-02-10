---
title: Run SQL Queries with chv
tags: [cloud, local, query, sql]
---

## Run SQL Queries with chv

Execute SQL queries using `chv run` without starting a persistent server. Uses `clickhouse local` for quick queries against local data or for ad-hoc computation.

### Prerequisites

- chv CLI installed with at least one ClickHouse version (see `setup-install`)

### Steps

1. **Run a quick SQL query**

```bash
chv run --sql "SELECT 1"
```

Short form:

```bash
chv run -s "SELECT * FROM system.functions LIMIT 5"
```

2. **Run clickhouse local with full options**

```bash
chv run local --query "SELECT 1"
```

3. **Get help for clickhouse local options**

```bash
chv run local -- --help
```

### Options Reference

| Command | Description |
|---------|-------------|
| `chv run --sql "..."` | Quick SQL query using clickhouse local |
| `chv run -s "..."` | Short form of `--sql` |
| `chv run local --query "..."` | Run clickhouse local with full options |
| `chv run local -- <args>` | Pass additional arguments to clickhouse local |

### Example

```bash
# Quick computation
chv run -s "SELECT sum(number) FROM numbers(1000000)"

# Query system tables
chv run -s "SELECT name, type FROM system.functions WHERE name LIKE '%sum%'"

# Process a local file
chv run local --query "SELECT * FROM file('data.csv', CSVWithNames) LIMIT 10"
```
