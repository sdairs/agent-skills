---
title: Initialize a Local ClickHouse Project
tags: [cloud, local, init, project]
---

## Initialize a Local ClickHouse Project

Create a project-local ClickHouse data directory for isolated development. Each project gets its own data, and data is scoped by ClickHouse version so switching versions won't cause compatibility issues.

### Prerequisites

- chv CLI installed with at least one ClickHouse version (see `setup-install`)

### Steps

1. **Navigate to your project directory**

```bash
cd /path/to/your/project
```

2. **Initialize the local data directory**

```bash
chv init
```

This creates a `.clickhouse/` directory with a `.gitignore` so it won't be committed.

> **Note:** `chv run server` automatically runs `init` if needed, so you can skip this step.

3. **Verify the directory structure**

```bash
ls -la .clickhouse/
```

### Directory Structure

```
.clickhouse/
├── .gitignore
├── 25.12.5.44/
│   ├── data/
│   └── ...
└── 26.1.2.11/
    ├── data/
    └── ...
```

Each ClickHouse version gets its own subdirectory, so switching versions with `chv use` won't cause compatibility issues.

### Example

```bash
mkdir my-analytics-project && cd my-analytics-project
chv init
chv run server
```
