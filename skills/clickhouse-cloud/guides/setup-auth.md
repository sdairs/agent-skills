---
title: Configure Cloud API Credentials
tags: [cloud, setup, auth, api]
---

## Configure Cloud API Credentials

Set up authentication for ClickHouse Cloud API commands. Required before using any `chv cloud` command.

### Prerequisites

- chv CLI installed (see the `clickhouse-local-development` skill)
- A ClickHouse Cloud account
- An API key and secret from the ClickHouse Cloud console

### Steps

1. **Create an API key in the ClickHouse Cloud console**

   Go to your ClickHouse Cloud console and create an API key under your organization settings.

2. **Run `chv cloud auth` in a separate terminal**

> **Note for agents:** This is an interactive command. Ask the user to open a new terminal tab and run it there. Once they confirm it's done, you can continue.

```bash
chv cloud auth
```

This prompts for your API key and secret, then stores them in a credentials file at `.clickhouse/`. Once configured, all `chv cloud` commands authenticate automatically — no flags or environment variables needed.

3. **Verify access**

```bash
chv cloud org list
```

This should return your organization(s) if credentials are valid.

### Auth Fallback Order

`chv cloud` commands resolve credentials in this order:

1. **Inline flags** — `--api-key` and `--api-secret` (overrides everything)
2. **Credentials file** — created by `chv cloud auth` (recommended)
3. **Environment variables** — `CLICKHOUSE_CLOUD_API_KEY` and `CLICKHOUSE_CLOUD_API_SECRET`

The credentials file is the recommended approach — it avoids leaking secrets in shell history or env vars.

### Example

```bash
# In a separate terminal, run the interactive auth flow
chv cloud auth

# Back in your main terminal, verify it works
chv cloud org list
```

Reference: [ClickHouse Cloud API Keys](https://clickhouse.com/docs/en/cloud/manage/api)
