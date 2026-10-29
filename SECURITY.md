# Security Policy

## Reporting a Vulnerability

If you discover a security issue affecting **Arckép** (`arckep.ru`), its public REST API, or its remote MCP endpoint (`https://arckep.ru/api/v1/mcp`), please report it privately by email:

- **Email:** `support@arckep.ru`

Please **do not** open public GitHub issues containing vulnerability details, live API keys (`ark_live_...`), OAuth tokens, or personal account data.

## API Key Hygiene for MCP Users

- Store `ark_live_...` API keys in your MCP client's secret store or environment variables — never commit keys to repositories or paste them into public prompts.
- Scope keys to only the capabilities your workflow needs in **[Settings → API](https://arckep.ru/settings?tab=api)**.
- Revoke compromised or unused keys immediately from **[Settings → API](https://arckep.ru/settings?tab=api)**.
