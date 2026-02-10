---
title: Run a Local ClickHouse Server
tags: [cloud, local, server, run]
---

## Run a Local ClickHouse Server

Start a ClickHouse server using the project-local data directory. The server uses the version set by `chv use` and stores data in `.clickhouse/` within the current directory.

### Prerequisites

- chv CLI installed with at least one ClickHouse version (see `setup-install`)

### Steps

1. **Start the server**

```bash
chv run server
```

This auto-initializes `.clickhouse/` if needed and starts a ClickHouse server bound to the project-local data directory.

2. **Start the server with a custom config**

```bash
chv run server -- --config-file=/path/to/config.xml
```

3. **Connect to the running server with the client**

```bash
chv run client
```

4. **Connect with custom options**

```bash
chv run client -- --host localhost --query "SHOW DATABASES"
```

### Options Reference

| Command | Description |
|---------|-------------|
| `chv run server` | Start server with project-local data |
| `chv run server -- <args>` | Pass additional arguments to clickhouse-server |
| `chv run client` | Connect to the local server |
| `chv run client -- <args>` | Pass additional arguments to clickhouse-client |

### Example

```bash
# Start a local server and create a table
chv run server &
chv run client -- --query "CREATE TABLE test (id UInt64, name String) ENGINE = MergeTree() ORDER BY id"
chv run client -- --query "INSERT INTO test VALUES (1, 'hello'), (2, 'world')"
chv run client -- --query "SELECT * FROM test"
```
