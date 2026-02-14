---
title: Initialize a Local ClickHouse Project
tags: [local, init, project]
---

## Initialize a Local ClickHouse Project

Create a project-local ClickHouse environment for isolated development. `chv init` creates the data directory and scaffolds a suggested SQL file structure for organizing your tables, materialized views, and queries.

### Prerequisites

- chv CLI installed with at least one ClickHouse version (see `setup-install`)

### Steps

1. **Navigate to your project directory**

```bash
cd /path/to/your/project
```

2. **Initialize the project**

```bash
chv init
```

This creates:
- `.clickhouse/` — project-local data directory (gitignored)
- `clickhouse/tables/` — table schema SQL files
- `clickhouse/materialized_views/` — materialized view SQL files
- `clickhouse/queries/` — reusable query SQL files

> **Note:** `chv run server` automatically runs `init` if needed, so you can skip this step if you just want to start a server. Running `chv init` explicitly is useful when you want to set up the project structure before writing SQL.

3. **Verify the directory structure**

```bash
ls -R clickhouse/
```

### Directory Structure

```
your-project/
├── .clickhouse/           # Data directory (gitignored)
│   ├── .gitignore
│   └── 25.12.5.44/
│       ├── data/
│       └── ...
└── clickhouse/            # SQL files (version-controlled)
    ├── tables/
    ├── materialized_views/
    └── queries/
```

The `.clickhouse/` data directory is scoped by ClickHouse version, so switching versions with `chv use` won't cause compatibility issues.

The `clickhouse/` directory is where you write your SQL files. This structure is a suggested convention — see `local-run-client` for how to execute these files.

### Example

```bash
mkdir my-analytics-project && cd my-analytics-project
chv init
chv run server
```
