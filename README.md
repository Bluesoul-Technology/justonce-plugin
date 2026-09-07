# JustOnce — memory for your AI assistants

<p align="center"><img src="assets/icon.png" width="96" alt="JustOnce"></p>

**JustOnce** is a persistent, bitemporal memory layer for AI assistants. Tell it something once and every tool you use — Cursor, Claude, ChatGPT, VS Code, Claude Code and any other MCP client — can recall it later, with a full history of what was true and when.

This repository is the public **MCP plugin manifest** for the hosted JustOnce server. The server itself runs at `https://mcp.justonce.ai/`. There is nothing to build or self-host: install the plugin, sign in, done.

- Website & setup guide: <https://justonce.ai/connect>
- Privacy policy: <https://justonce.ai/privacy>
- Terms: <https://justonce.ai/terms>
- Support: <https://justonce.ai/contact>

## Install

| Client | How |
| --- | --- |
| **Cursor** | [![Add to Cursor](https://img.shields.io/badge/Add_to-Cursor-black?logo=cursor)](cursor://anysphere.cursor-deeplink/mcp/install?name=justonce&config=eyJ1cmwiOiJodHRwczovL21jcC5qdXN0b25jZS5haS8ifQ==) — or paste the JSON below into `~/.cursor/mcp.json` |
| **Claude.ai / Claude Desktop** | Settings → Connectors → *Add custom connector* → URL `https://mcp.justonce.ai/` |
| **Claude Code** | `claude mcp add --transport http justonce https://mcp.justonce.ai/` |
| **ChatGPT** | Settings → Connectors → *Create* → URL `https://mcp.justonce.ai/` |
| **VS Code / GitHub Copilot** | Add the JSON below to `.vscode/mcp.json` (use `"servers"` instead of `"mcpServers"`) |
| **Any other MCP client** | Streamable HTTP endpoint `https://mcp.justonce.ai/` |

```json
{
  "mcpServers": {
    "justonce": {
      "type": "streamable-http",
      "url": "https://mcp.justonce.ai/"
    }
  }
}
```

## Authentication

JustOnce uses **OAuth 2.0** with PKCE and dynamic client registration. No API key or token goes in any config file. The first time a client connects it opens a browser window where you sign in to (or create) your JustOnce account and approve the scopes; the client stores the resulting token itself.

Discovery endpoints, for clients that need them:

- `https://mcp.justonce.ai/.well-known/oauth-protected-resource`
- `https://mcp.justonce.ai/.well-known/oauth-authorization-server`

Scopes: `memories:read`, `memories:write`, `context:read`, `categories:read`.

## What the assistant gets

Once connected, the assistant can:

- **Remember** — `add_memory`, `update_memory`, `forget_memory`, `import_memories`
- **Recall** — `search_memories`, `get_context`, `get_core_profile`, `get_memory`
- **Travel in time** — `get_as_of`, `get_memory_history` (bitemporal: what was true, and when you knew it)
- **Walk the graph** — `get_relationships`, `find_related_entities`, `explore_connections`, `get_entity_confidence`
- **Files & documents** — `store_document`, `fetch_document`, `request_upload_link`
- **Shared spaces** — `list_shared_spaces`, `open_shared_space`, `list_space_memories`, `add_memories_to_space`

Every tool carries `readOnlyHint` / `destructiveHint` annotations so clients can ask before anything is changed or erased. `forget_memory` is permanent.

## Data & privacy

Memories are encrypted at rest and belong to the signed-in user only. JustOnce never trains on your data. Full details: <https://justonce.ai/privacy>.

## About this repo

This repo follows the [Agent Plugins](https://agent-plugins.org) standard (`plugin.json` + `mcp.json`) and also ships a Cursor-native manifest in `.cursor-plugin/`. It contains no server code — the JustOnce service is proprietary and hosted by [Bluesoul Technology](https://justonce.ai).

Found a problem with the manifest or docs? Open an issue here. Product support: <https://justonce.ai/contact>.
