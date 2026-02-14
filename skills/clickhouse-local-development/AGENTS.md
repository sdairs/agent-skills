# ClickHouse Local Development with chv — Complete Guide

Step-by-step guides for local ClickHouse development using the chv CLI. 5 guides across 3 sections covering setup, local development, and migration to cloud.

> **Official docs:** [ClickHouse](https://clickhouse.com/docs)
>
> **For Cloud operations** (service creation, sizing, security, backups): See the `clickhouse-cloud` skill.

---

## Table of Contents

1. [Setup](#1-setup)
   - [Install the chv CLI](#install-the-chv-cli)
2. [Local Development](#2-local-development)
   - [Initialize a Local ClickHouse Project](#initialize-a-local-clickhouse-project)
   - [Run a Local ClickHouse Server](#run-a-local-clickhouse-server)
   - [Run the ClickHouse Client](#run-the-clickhouse-client)
3. [Migration](#3-migration)
   - [Migrate from Local Development to Cloud](#migrate-from-local-development-to-cloud)

---

## 1. Setup

### Install the chv CLI

Install chv, a fast ClickHouse version manager and cloud CLI. chv manages local ClickHouse versions and provides commands for interacting with ClickHouse Cloud.

**Prerequisites:** macOS (aarch64, x86_64) or Linux (aarch64, x86_64)

**Steps:**

1. **Install chv using the quick install script**

```bash
curl -fsSL https://raw.githubusercontent.com/sdairs/chv/main/install.sh | sh
```

This detects your OS and architecture, downloads the latest binary to `~/.local/bin/chv`, and makes it executable.

2. **Verify the installation**

```bash
chv --help
```

3. **Install a ClickHouse version**

```bash
# Install the latest stable release
chv install stable

# Or install the latest LTS release
chv install lts

# Or install a specific version
chv install 25.12
```

4. **Set the default version**

```bash
chv use stable
```

5. **Verify the active version**

```bash
chv which
```

**Version management commands:**

| Command | Description |
|---------|-------------|
| `chv install stable` | Install latest stable release |
| `chv install lts` | Install latest LTS release |
| `chv install 25.12` | Install latest 25.12.x.x |
| `chv install 25.12.5.44` | Install exact version |
| `chv list` | List installed versions |
| `chv list --remote` | List versions available for download |
| `chv use <version>` | Set default version (installs if needed) |
| `chv which` | Show current default version |
| `chv remove <version>` | Remove an installed version |

Versions are stored in `~/.clickhouse/versions/`.

---

## 2. Local Development

### Initialize a Local ClickHouse Project

Create a project-local ClickHouse environment with a suggested SQL file structure for organizing tables, views, and queries.

**Prerequisites:** chv CLI installed with at least one ClickHouse version.

**Steps:**

1. **Initialize the project**

```bash
chv init
```

This creates:
- `.clickhouse/` — project-local data directory (gitignored)
- `clickhouse/tables/` — table schema SQL files
- `clickhouse/materialized_views/` — materialized view SQL files
- `clickhouse/queries/` — reusable query SQL files

> **Note:** `chv run server` automatically runs `init` if needed, so you can skip this step if you just want to start a server.

**Directory structure:**

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

The `.clickhouse/` data directory is scoped by ClickHouse version, so switching versions with `chv use` won't cause compatibility issues. The `clickhouse/` directory is where you write your SQL files — see `local-run-client` for how to execute them.

---

### Run a Local ClickHouse Server

Start a ClickHouse server using the project-local data directory.

**Prerequisites:** chv CLI installed with at least one ClickHouse version.

**Steps:**

1. **Start the server**

```bash
chv run server
```

This auto-initializes `.clickhouse/` if needed and starts a ClickHouse server bound to the project-local data directory.

2. **Start the server with a custom config**

```bash
chv run server -- --config-file=/path/to/config.xml
```

> **Next step:** To connect to the running server and execute SQL, see the [Run the ClickHouse Client](#run-the-clickhouse-client) section below.

**Commands:**

| Command | Description |
|---------|-------------|
| `chv run server` | Start server with project-local data |
| `chv run server -- <args>` | Pass additional arguments to clickhouse-server |

---

### Run the ClickHouse Client

Connect to a running local server and execute SQL from files.

**Prerequisites:** chv CLI installed, local server running (see `setup-install`, `local-run-server`).

**Steps:**

1. **Connect interactively**

```bash
chv run client
```

2. **Run a SQL file**

```bash
chv run client -- --queries-file clickhouse/tables/events.sql
```

3. **Run via stdin**

```bash
chv run client -- < clickhouse/tables/events.sql
```

4. **Run multiple files in sequence**

```bash
chv run client -- --queries-file clickhouse/tables/events.sql
chv run client -- --queries-file clickhouse/materialized_views/events_daily.sql
chv run client -- --queries-file clickhouse/queries/top_events.sql
```

5. **Insert data from CSV**

```bash
chv run client -- --query "INSERT INTO events FORMAT CSVWithNames" < data/seed.csv
```

**Key options:**

| Option | Description |
|--------|-------------|
| `--queries-file <path>` | Execute SQL statements from a file |
| `--query <sql>` | Execute a single SQL statement inline |
| `--multiquery` | Allow multiple semicolon-separated statements |
| `--format <fmt>` | Output format (e.g. `TabSeparated`, `CSV`, `JSON`, `Pretty`) |

---

## 3. Migration

### Migrate from Local Development to Cloud

Deploy your local ClickHouse schema and data to a ClickHouse Cloud service. Since your SQL files are version-controlled (see `local-run-client`), migration is replaying the same files against the cloud endpoint.

**Prerequisites:** SQL files developed locally (see `local-run-client`).

**Steps:**

1. **Set up Cloud API credentials** — ask the user to run `chv cloud auth` in a separate terminal (interactive). See `setup-auth` in `clickhouse-cloud` skill.

```bash
chv cloud auth
```

2. **Create a Cloud service** (see `service-create` in `clickhouse-cloud` skill)

```bash
chv cloud --json service create \
  --name my-project \
  --provider aws \
  --region us-east-1 \
  --tier development
```

3. **Apply table schemas to cloud**

```bash
chv run client -- \
  --host <cloud-hostname> \
  --port 9440 \
  --secure \
  --user default \
  --password <password> \
  --queries-file clickhouse/tables/events.sql
```

4. **Apply materialized views**

```bash
chv run client -- \
  --host <cloud-hostname> \
  --port 9440 \
  --secure \
  --user default \
  --password <password> \
  --queries-file clickhouse/materialized_views/events_daily.sql
```

5. **Load seed data**

```bash
chv run client -- \
  --host <cloud-hostname> \
  --port 9440 \
  --secure \
  --user default \
  --password <password> \
  --query "INSERT INTO events FORMAT CSVWithNames" < data/seed.csv
```

6. **Validate**

```bash
chv run client -- \
  --host <cloud-hostname> \
  --port 9440 \
  --secure \
  --user default \
  --password <password> \
  --query "SELECT count() FROM events"
```

**Tips:**

- Run files in dependency order: tables first, then materialized views, then seed data
- For large datasets, use Native format instead of CSV for better performance
- Your SQL files are the source of truth — no need to export from the local server

Reference: [ClickHouse Cloud API](https://clickhouse.com/docs/en/cloud/manage/api)
