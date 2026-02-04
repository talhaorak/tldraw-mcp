# tldraw-mcp (local .tldr MCP server)

This skill helps you run and configure the `@talhaorak/tldraw-mcp` MCP server, which manages local **tldraw** canvas files (`.tldr`).

## What it does
- Read / write / list / search local `.tldr` files
- Add / update / delete shapes programmatically

## Install

```bash
curl -fsS https://raw.githubusercontent.com/talhaorak/tldraw-mcp/main/SKILLS.md | bash
```

## Configure OpenClaw

Add an MCP server entry (example):

```yaml
mcp:
  servers:
    tldraw:
      command: npx
      args: ["@talhaorak/tldraw-mcp"]
      env:
        TLDRAW_DIR: /path/to/your/tldraw/files
```

## Environment
- `TLDRAW_DIR` (default: `~/.tldraw`)
