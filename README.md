# undocsend for AI agents

[undocsend](https://undocsend.com) is a dealroom for founders raising capital or running an M&A process. You put
documents in rooms, share gated links with investors, and see who read what.

This repository holds the skills, plugin manifest and MCP client configs that let an AI agent work in an undocsend
workspace. The server's source is private and not included.

## Connect

The product MCP server:

```
https://mcp.undocsend.com/mcp
```

- **Transport:** Streamable HTTP.
- **Auth:** OAuth 2.1 with PKCE (`S256`) and dynamic client registration. Your client opens undocsend in a browser and
  the workspace owner or admin approves the connection there. There are no API keys. The flow is written up for
  agents at [undocsend.com/auth.md](https://undocsend.com/auth.md).
- **Scopes:** `read`, `build`, `share`. `share` (links, invites, revoking access) stays off until the founder ticks it.
- **One connection, one workspace.** The founder picks the workspace on the approval screen.

A public docs MCP server at `https://mcp.undocsend.com/docs` is coming. It is not live yet. Until it is, read the docs
as markdown (see [Docs for agents](#docs-for-agents)).

To try undocsend without an account, open the demo sandbox at [demo.undocsend.com](https://demo.undocsend.com).

### Claude Code

```sh
claude mcp add --transport http undocsend https://mcp.undocsend.com/mcp
```

Then run `/mcp`, choose **undocsend** and sign in. Or copy [`examples/claude-code/.mcp.json`](./examples/claude-code/.mcp.json)
into your project.

### Claude (claude.ai, Claude Desktop and mobile)

**Customize → Connectors → Add → Add custom connector**. Name it `undocsend`, paste the server URL, then **Continue**,
**Add** and **Connect**, and approve in the window that opens. On Team and Enterprise plans, an owner adds it under
**Organization settings → Connectors**.

### Cursor

Put [`examples/cursor/mcp.json`](./examples/cursor/mcp.json) at `~/.cursor/mcp.json` (all projects) or
`.cursor/mcp.json` (one project):

```json
{
  "mcpServers": {
    "undocsend": {
      "url": "https://mcp.undocsend.com/mcp"
    }
  }
}
```

Cursor shows that undocsend needs sign-in. Start it and approve in your browser.

### VS Code

Run **MCP: Add Server** from the Command Palette, or put [`examples/vscode/.mcp.json`](./examples/vscode/.mcp.json) at
your workspace root:

```json
{
  "mcpServers": {
    "undocsend": {
      "type": "http",
      "url": "https://mcp.undocsend.com/mcp"
    }
  }
}
```

VS Code opens a browser window for OAuth on the first connection.

### Any other MCP client

| | |
|---|---|
| Endpoint | `https://mcp.undocsend.com/mcp` |
| Protected resource metadata | `https://mcp.undocsend.com/.well-known/oauth-protected-resource/mcp` |
| Authorization server metadata | `https://mcp.undocsend.com/.well-known/oauth-authorization-server` |
| Client registration | Dynamic (`/oauth/register`), public clients only. Client ID metadata documents are also accepted. |
| Token | `Authorization: Bearer <access token>`. Query-string tokens are refused. |

## What an agent can do

Read rooms, documents, analytics and visitor activity. Build rooms, folders and documents, and publish HTML pages
into a room. Draft answers to investor questions. With `share`, create links and invite investors.

Some actions wait for the founder. Deleting things, opening a room or file to more people, and removing teammates
are filed as requests that the founder approves in undocsend. An agent can never approve or send an answer, touch
billing or sign-in settings, or change its own permissions.

## Skills

| Skill | Use it for |
|---|---|
| [`undocsend`](./skills/undocsend/SKILL.md) | Connecting, scopes, every tool, and what needs a person. |
| [`undocsend-build-dealroom`](./skills/undocsend-build-dealroom/SKILL.md) | Creating a room, laying out folders, uploading, sharing with investors. |
| [`undocsend-investor-activity`](./skills/undocsend-investor-activity/SKILL.md) | Analytics, visitors, NDA acceptances, and drafting answers to investor questions. |

Install them with the [skills CLI](https://skills.sh):

```sh
npx skills add itseasyco/undocsend-agents
```

## In this repository

| File | What it is |
|---|---|
| `skills/` | The three skills above, in the [Agent Skills](https://agentskills.io) format. |
| [`plugin.json`](./plugin.json) + [`mcp.json`](./mcp.json) | [Agent Plugin](https://agent-plugins.org) manifest and its remote MCP server. |
| [`.mcp.json`](./.mcp.json) | Project MCP config for Claude Code, VS Code and other clients that read it. |
| [`examples/`](./examples) | MCP config for Claude Code, Cursor and VS Code. |
| [`server.json`](./server.json) | Entry for the official MCP Registry, `com.undocsend/mcp`. |
| [`AGENTS.md`](./AGENTS.md) | Instructions for AI coding agents that use undocsend or edit this repository. |

## Plans

Every plan has every feature. Plans differ only by how many rooms you get. E-signature is on every plan (a US beta,
with a fair-use limit). For AI features, bring your own provider key (we add no markup) or buy credits from $5. Try
it free for 14 days, no credit card. Prices: [undocsend.com/pricing](https://undocsend.com/pricing).

## Docs for agents

- Developers: https://undocsend.com/developers
- MCP server: https://undocsend.com/mcp
- Agent docs: https://undocsend.com/docs/agents (add `.md` to any docs URL for markdown)
- Auth for agents: https://undocsend.com/auth.md
- llms.txt: https://undocsend.com/llms.txt

## License

The contents of this repository are MIT licensed (see [LICENSE](./LICENSE)). undocsend itself is a hosted service by
Easy Labs Inc. and is not covered by this license.
