<img src="assets/logo.png" alt="Flowsimp" width="72" height="72">

# Flowsimp MCP Server

Create flowcharts from chat. Flowsimp's hosted [Model Context Protocol](https://modelcontextprotocol.io) server turns Mermaid flowchart syntax or a plain indented outline into a shareable diagram link and an editable board in the [Flowsimp](https://flowsimp.com) editor.

- **Endpoint:** `https://flowsimp.com/mcp`
- **Transport:** Streamable HTTP (remote)
- **Auth:** none, no API key or account needed
- **Price:** completely free to use
- **Registry name:** `com.flowsimp/flowsimp` in the [official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=com.flowsimp)
- **Docs:** https://flowsimp.com/mcp

The server is stateless. It does not store your diagrams, because each diagram is encoded in the link it returns. Open the editable link to rearrange shapes, edit text, and export PNG or SVG.

This repository holds the public documentation and the registry metadata (`server.json`, `.mcp.json`) for the hosted server.

## Tools

| Tool | What it does |
| --- | --- |
| `create_flowchart` | Builds a flowchart from Mermaid flowchart syntax or an indented outline (up to 16,000 characters). Optional `title`, and optional `mode`: `flowchart` (default) or `network`. Returns a read-only `shareUrl`, an editable `editUrl`, and the node and connection counts. |
| `list_templates` | Lists the built-in templates (flowchart, process, swimlane, decision tree, user flow, org chart, network, mind map) with a link that opens each one in the editor. |

Example `create_flowchart` input:

```json
{
  "text": "flowchart TD\n  A[Start] --> B{Approved?}\n  B -->|Yes| C[Ship it]\n  B -->|No| D[Revise]",
  "title": "Release check"
}
```

Example result (links shortened):

```json
{
  "shareUrl": "https://flowsimp.com/view#…",
  "editUrl": "https://flowsimp.com/?board=…",
  "nodeCount": 4,
  "connectionCount": 3,
  "title": "Release check"
}
```

An indented outline works too:

```text
Customer submits order
  Payment approved?
    Yes: Pack and ship
    No: Email customer
```

## Connect a client

### Claude (claude.ai and Claude Desktop)

Open **Settings → Connectors → Add custom connector**, name it `Flowsimp`, and paste:

```
https://flowsimp.com/mcp
```

No authentication settings are needed.

### Claude Code

```bash
claude mcp add --transport http flowsimp https://flowsimp.com/mcp
```

### Cursor

Add to `~/.cursor/mcp.json` (global) or `.cursor/mcp.json` (project):

```json
{
  "mcpServers": {
    "flowsimp": {
      "url": "https://flowsimp.com/mcp"
    }
  }
}
```

### VS Code (GitHub Copilot agent mode)

Add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "flowsimp": {
      "type": "http",
      "url": "https://flowsimp.com/mcp"
    }
  }
}
```

### ChatGPT

In workspaces or plans that support custom MCP connectors (developer mode), create a connector with the URL `https://flowsimp.com/mcp` and no authentication. You can also add a GPT Action from the OpenAPI description at `https://flowsimp.com/openapi.json`.

### Clients that only support local (stdio) servers

Use the [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) bridge:

```json
{
  "mcpServers": {
    "flowsimp": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://flowsimp.com/mcp"]
    }
  }
}
```

## Example prompts

- "Make a flowchart of our customer refund process and give me the Flowsimp link."
- "Turn this Mermaid code into an editable diagram: …"
- "Draw a decision tree for whether to approve a vacation request."
- "Map the user sign-up flow for a mobile app, from landing page to first login."
- "Show me the Flowsimp templates, then start me on a swimlane diagram."
- "Create a simple network diagram: router, firewall, two web servers, and a database."

## HTTP API

The same create action is available without MCP:

```bash
curl -X POST https://flowsimp.com/api/flowchart \
  -H 'Content-Type: application/json' \
  -d '{"text": "flowchart TD\n  A[Start] --> B[Next]"}'
```

The OpenAPI description is at https://flowsimp.com/openapi.json.

## Limits

- Text input is capped at 16,000 characters.
- Calls are rate limited per IP.

## Links

- Flowsimp editor: https://flowsimp.com
- MCP docs: https://flowsimp.com/mcp
- Text to flowchart: https://flowsimp.com/text-to-flowchart
- About: https://flowsimp.com/about

## License

The contents of this repository are released under the [MIT License](LICENSE). © 2026 Dreamer Studio.
