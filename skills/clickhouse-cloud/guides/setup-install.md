---
title: Install the chv CLI
tags: [cloud, setup, install]
---

## Install the chv CLI

Install chv, a fast ClickHouse version manager and cloud CLI. chv manages local ClickHouse versions and provides commands for interacting with ClickHouse Cloud.

### Prerequisites

- macOS (aarch64, x86_64) or Linux (aarch64, x86_64)

### Steps

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

### Options Reference

| Command | Description |
|---------|-------------|
| `chv install stable` | Install latest stable release |
| `chv install lts` | Install latest LTS release |
| `chv install 25.12` | Install latest 25.12.x.x |
| `chv install 25.12.5.44` | Install exact version |
| `chv list` | List installed versions |
| `chv list --available` | List versions available for download |
| `chv use <version>` | Set default version (installs if needed) |
| `chv which` | Show current default version |
| `chv remove <version>` | Remove an installed version |

### Example

```bash
# Full setup from scratch
curl -fsSL https://raw.githubusercontent.com/sdairs/chv/main/install.sh | sh
chv install stable
chv use stable
chv which
```

Versions are stored in `~/.clickhouse/versions/`.

Reference: [ClickHouse Releases](https://github.com/ClickHouse/ClickHouse/releases)
