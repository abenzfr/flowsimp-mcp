# Security

The Flowsimp MCP server is a hosted, stateless endpoint at `https://flowsimp.com/mcp`. It requires no credentials and does not write diagrams to a database; each diagram is encoded in the returned link. Requests are rate limited per IP and input is capped at 16,000 characters.

## Reporting a vulnerability

Please report security issues privately through GitHub's **Report a vulnerability** button on this repository's Security tab. Do not open a public issue for security reports.

Include the affected URL or tool, steps to reproduce, and the impact you observed. We aim to acknowledge reports within a few business days.
