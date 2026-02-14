---
title: Run a Local ClickHouse Server
tags: [local, server, run]
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

> **Next step:** To connect to the running server and execute SQL, see `local-run-client`.

### Custom arguments

Format: `chv run server -- <args>` 

Example: `chv run server -- --config-file=/path/to/config.xml`
