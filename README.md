# JustOnce — memory for your AI assistants

<p align="center"><img src="assets/icon.png" width="128" alt="JustOnce"></p>

**JustOnce** is a persistent memory layer for AI coding assistants, delivered over MCP. Tell it your stack, conventions, project decisions and preferences once and every tool you use — Cursor, Claude, ChatGPT, VS Code, Claude Code and any other MCP client — recalls them in every later session, with a full bitemporal history of what was true and when.

It does three things:

- **Memories** — facts, decisions and preferences the assistant stores and recalls across sessions and tools (`add_memory`, `search_memories`, `get_context`).
- **Documents** — specs, READMEs, config and PDFs stored alongside memories so the assistant can fetch them back into context when needed (`store_document`, `fetch_document`).
- **Shared spaces** — a team, client or family gets one memory their own assistants can read, with owner, editor and viewer roles (`open_shared_space`, `add_memories_to_space`).

It is one vault, not one per app: the same memory follows you across assistants, and through shared spaces it follows your team, clients, contractors and family too — each with their own assistant, each seeing only what was shared with them.

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
- **Shared spaces** — `list_shared_spaces`, `open_shared_space`, `close_shared_space`, `list_space_memories`, `add_memories_to_space`, `remove_memories_from_space`

Every tool carries `readOnlyHint` / `destructiveHint` annotations so clients can ask before anything is changed or erased. `forget_memory` is permanent.

## Shared spaces — a memory layer you can share

Your personal vault is private and is never shared. A **shared space** is a separate container you choose to put specific memories and documents into, and invite people to:

- **One space per relationship.** A household space, a team space, a space per client or engagement. Separate spaces mean separate memberships — no cross-client bleed, no over-sharing.
- **Three roles, nothing to configure.** Owner, Editor and Viewer.
- **Works across assistants.** Members connect whichever MCP client they like. When someone asks their assistant a question, `search_memories` also returns hits from the shared spaces they belong to, each carrying a `shared` block that says which space it came from and who shared it — so the assistant attributes it correctly instead of presenting it as the user's own.
- **Owner stays in control.** Shared memories are read-only to everyone but the person who created them; only they can change or erase them. Remove a memory from a space, or close the space, and it stops being visible to the other members.

Typical uses: a family keeping the household's dates, sizes and contacts in one place every assistant can reach; a team giving every member's AI the same project context; a contractor or agency holding a client's preferences in a space that is closed when the engagement ends.

## Data & privacy

Memories are encrypted at rest and belong to the signed-in user only. JustOnce never trains on your data. Full details: <https://justonce.ai/privacy>.

## About this repo

This repo follows the [Agent Plugins](https://agent-plugins.org) standard (`plugin.json` + `mcp.json`) and also ships a Cursor-native manifest in `.cursor-plugin/`. It contains no server code — the JustOnce service is proprietary and hosted by [Bluesoul Technology](https://justonce.ai).

Found a problem with the manifest or docs? Open an issue here. Product support: <https://justonce.ai/contact>.
