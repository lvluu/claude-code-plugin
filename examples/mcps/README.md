# MCP Server Catalog

Copy any of these into your `.mcp.json` under `"mcpServers"`.

## Available Servers

| File | Server | What it does | Requires |
|------|--------|-------------|----------|
| `serena.json` | Serena | Semantic code intelligence via LSP (30+ langs) | [uv](https://docs.astral.sh/uv/getting-started/installation/) |
| `chrome-devtools.json` | Chrome DevTools | Chrome DevTools Protocol (inspect, debug, profile) | Node 18+ |
| `context7.json` | Context7 | Up-to-date library/framework docs | Node 18+ |
| `shadcn.json` | Shadcn UI | Official shadcn CLI with MCP support | Node 18+ |

## Usage

Pick what you need and merge into `.mcp.json`:

```jsonc
{
  "mcpServers": {
    // paste server entries here
  }
}
```

## Optional API Keys

Some servers work better with API keys. Pass them via `args` or `env`:

```jsonc
// Context7 with API key (free at https://context7.com/dashboard)
"context7": {
  "type": "stdio",
  "command": "npx",
  "args": ["-y", "@upstash/context7-mcp", "--api-key", "YOUR_KEY"]
}

```

## Adding Your Own

Create a new `.json` file here with the same shape:

```json
{
  "server-name": {
    "type": "stdio",
    "command": "npx",
    "args": ["-y", "@scope/package"],
    "env": {
      "API_KEY": "value"
    }
  }
}
```
