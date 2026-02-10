---
title: Configure Cloud API Credentials
tags: [cloud, setup, auth, api]
---

## Configure Cloud API Credentials

Set up authentication for ClickHouse Cloud API commands. Required before using any `chv cloud` command.

### Prerequisites

- chv CLI installed (see `setup-install`)
- A ClickHouse Cloud account
- An API key and secret from the ClickHouse Cloud console

### Steps

1. **Create an API key in the ClickHouse Cloud console**

   Go to your ClickHouse Cloud console and create an API key under your organization settings.

2. **Set credentials as environment variables**

```bash
export CLICKHOUSE_CLOUD_API_KEY=your-key
export CLICKHOUSE_CLOUD_API_SECRET=your-secret
```

3. **Persist credentials in your shell profile**

```bash
# Add to ~/.bashrc, ~/.zshrc, or equivalent
echo 'export CLICKHOUSE_CLOUD_API_KEY=your-key' >> ~/.zshrc
echo 'export CLICKHOUSE_CLOUD_API_SECRET=your-secret' >> ~/.zshrc
source ~/.zshrc
```

4. **Verify access**

```bash
chv cloud org list
```

This should return your organization(s) if credentials are valid.

### Options Reference

| Method | Usage |
|--------|-------|
| Environment variables | `export CLICKHOUSE_CLOUD_API_KEY=...` and `export CLICKHOUSE_CLOUD_API_SECRET=...` |
| Inline flags | `chv cloud --api-key KEY --api-secret SECRET ...` |

### Example

```bash
# Set credentials and verify
export CLICKHOUSE_CLOUD_API_KEY=abc123
export CLICKHOUSE_CLOUD_API_SECRET=secret456
chv cloud org list
```

Reference: [ClickHouse Cloud API Keys](https://clickhouse.com/docs/en/cloud/manage/api)
