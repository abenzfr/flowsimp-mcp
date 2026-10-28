# Installing the Flowsimp MCP server

Flowsimp is a hosted, remote MCP server. There is nothing to download, build, or run locally, and no API key is required.

- URL: `https://flowsimp.com/mcp`
- Transport: Streamable HTTP
- Authentication: none

## Cline

Add this entry to `cline_mcp_settings.json`:

```json
{
  "mcpServers": {
    "flowsimp": {
      "type": "streamableHttp",
      "url": "https://flowsimp.com/mcp",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

## Other clients

Any client that supports remote Streamable HTTP servers can use the URL above with no headers. For clients that only support local stdio servers:

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

## Verify

After connecting, the server exposes two tools:

- `create_flowchart`: pass `text` containing Mermaid flowchart syntax or an indented outline. It returns `shareUrl` and `editUrl`.
- `list_templates`: takes no arguments and returns the built-in templates with editor links.

A quick test: call `create_flowchart` with `{"text": "flowchart TD\n  A[Start] --> B[Done]"}` and open the returned `editUrl`.
