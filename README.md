# fresh-feeds

**Continuously-maintained, machine-readable data + trust feeds for AI agents.**
No account, no API key — free preview/search + per-call paid access via [x402](https://x402.org) (USDC on Base).

🌐 Live: **https://fresh-feeds.foomworks.workers.dev** · 🔌 MCP: **`https://fresh-feeds.foomworks.workers.dev/mcp`** · 🧭 https://foomworks.app/fresh-feeds/

---

## What it gives an agent

- **MCP server registry** — the [Model Context Protocol](https://modelcontextprotocol.io) server registry, deduplicated and **quality-scored**, refreshed every 30 minutes. Find a good server for a capability, or poll what changed.
- **x402 services catalog** — the discovery catalog's active edge, **liveness-probed**: know a service is up and correctly demanding payment *before* you pay it.
- **Server verification** — a transparent 0–100 trust report for any registry MCP server (live endpoint probe + graded signals).

## Connect as an MCP server (recommended)

Streamable HTTP, JSON-RPC 2.0:

```
https://fresh-feeds.foomworks.workers.dev/mcp
```

| Tool | Cost | Returns |
|---|---|---|
| `search_mcp_registry` | free | matching MCP servers (name, score, status, remotes, packages) |
| `get_registry_highlights` | free | top 10 + trending + freshness |
| `verify_mcp_server` | free | preview trust grade for a server |
| `list_x402_services` | free | currently-live x402 services |
| `feeds_status` | free | feed freshness/health |
| `get_mcp_registry_snapshot` | x402 $0.05 | full registry snapshot |
| `get_registry_changes` | x402 $0.02 | added/removed/changed since a baseline |
| `get_x402_services_snapshot` | x402 $0.05 | full liveness-probed services feed |
| `verify_mcp_server_report` | x402 $0.03 | full verification report (live probe) |

Free tools return data directly; paid tools return x402 payment instructions for the full datasets (settled per call in USDC on Base — no account).

## Or call it over HTTP

```bash
BASE=https://fresh-feeds.foomworks.workers.dev
curl -s "$BASE/feeds/mcp-registry/preview"                 # free: top 10 by quality score
curl -s "$BASE/verify/mcp/preview?server=<NAME>"           # free: grade + headline liveness
curl -s "$BASE/feeds/mcp-registry"                         # paid: full snapshot (x402 handler settles the 402)
```

Discovery: [`/.well-known/agent-card.json`](https://fresh-feeds.foomworks.workers.dev/.well-known/agent-card.json) (A2A) · [`/openapi.json`](https://fresh-feeds.foomworks.workers.dev/openapi.json) · [`/.well-known/mcp.json`](https://fresh-feeds.foomworks.workers.dev/.well-known/mcp.json)

## Install the AgentSkill

[`SKILL.md`](./SKILL.md) is a portable AgentSkill (ClawHub / skills.sh / Claude Skills). Published on ClawHub as [`@foomworks/fresh-feeds`](https://clawhub.ai/foomworks/fresh-feeds).

## How it works & trust

- Quality score = active status + has-remotes + has-packages + real description + recency (the snapshot's `scoring` field documents it). Liveness reflects a probe, not an SLA.
- The service **holds no keys and never initiates payments** — all payment is buyer-side via x402; the `payTo` address only *receives*.
- Free `*/preview`, `/health`, and ETag/`If-None-Match` → `304` let you check for new data **before** paying.

## About

fresh-feeds is operated by **FOOM** — an AI-operated, human-supervised autonomous service.
License: [MIT](./LICENSE).
