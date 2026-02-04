# @talhaorak/tldraw-mcp

[![npm version](https://img.shields.io/npm/v/@talhaorak/tldraw-mcp.svg)](https://www.npmjs.com/package/@talhaorak/tldraw-mcp)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

MCP (Model Context Protocol) server for managing local [tldraw](https://tldraw.com) canvas files (`.tldr`).

## What Makes This Different?

Existing tldraw MCP servers let AI draw on an in-memory canvas. **This project is different** — it reads, writes, and searches **local `.tldr` files** on disk, making tldraw a persistent visual scratchpad that AI agents can programmatically update.

## Features

- 📖 **Read** — Load and parse `.tldr` files
- ✍️ **Write** — Create and update canvas files with validation
- 📋 **List** — Enumerate all `.tldr` files with metadata
- 🔍 **Search** — Full-text search across all canvases
- 🔷 **Shape CRUD** — Add, update, delete shapes programmatically

## Installation

### npm (recommended)

```bash
npx @talhaorak/tldraw-mcp
```

### From source

```bash
git clone https://github.com/talhaorak/tldraw-mcp.git
cd tldraw-mcp
bun install
bun run start
```

## Configuration

### Claude Desktop

Add to `~/Library/Application Support/Claude/claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "tldraw": {
      "command": "npx",
      "args": ["@talhaorak/tldraw-mcp"],
      "env": {
        "TLDRAW_DIR": "/path/to/your/tldraw/files"
      }
    }
  }
}
```

### OpenClaw

Add to your OpenClaw config:

```yaml
mcp:
  servers:
    tldraw:
      command: npx
      args: ["@talhaorak/tldraw-mcp"]
      env:
        TLDRAW_DIR: /path/to/your/tldraw/files
```

Or install as a skill:

```bash
curl -fsS https://raw.githubusercontent.com/talhaorak/tldraw-mcp/main/SKILLS.md | bash
```

### Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `TLDRAW_DIR` | `~/.tldraw` | Base directory for `.tldr` files |

## Tools

### `tldraw_read`
Read a `.tldr` file and return its parsed content.

```
tldraw_read({ path: "notes.tldr" })
```

### `tldraw_write`
Write or update a `.tldr` file (validates format).

```
tldraw_write({ 
  path: "notes.tldr",
  content: { /* tldraw file object */ }
})
```

### `tldraw_create`
Create a new empty tldraw canvas.

```
tldraw_create({ path: "new-canvas.tldr", name: "My Canvas" })
```

### `tldraw_list`
List all `.tldr` files with page/shape counts.

```
tldraw_list({ recursive: true })
```

### `tldraw_search`
Search text content across all canvases.

```
tldraw_search({ query: "TODO", searchIn: "text" })
```

### `tldraw_get_shapes`
Get all shapes from a file, optionally filtered by page.

```
tldraw_get_shapes({ path: "notes.tldr", pageId: "page:abc123" })
```

### `tldraw_add_shape`
Add a new shape to a canvas.

```
tldraw_add_shape({
  path: "notes.tldr",
  shape: {
    type: "geo",
    x: 100,
    y: 100,
    props: {
      w: 200,
      h: 100,
      geo: "rectangle",
      color: "blue",
      fill: "semi"
    }
  }
})
```

### `tldraw_update_shape`
Update properties of an existing shape.

```
tldraw_update_shape({
  path: "notes.tldr",
  shapeId: "shape:abc123",
  updates: { x: 200, props: { color: "red" } }
})
```

### `tldraw_delete_shape`
Delete a shape from a canvas.

```
tldraw_delete_shape({
  path: "notes.tldr",
  shapeId: "shape:abc123"
})
```

## Use Cases

- **Visual scratchpad for AI agents** — AI updates a canvas you can view in tldraw
- **Diagram generation** — Create flowcharts, architecture diagrams programmatically
- **Note organization** — Search and organize visual notes across multiple canvases
- **Integration with tldraw desktop/VS Code** — Files sync automatically

## tldraw File Format

This server works with tldraw v2 format:

```json
{
  "tldrawFileFormatVersion": 1,
  "schema": {
    "schemaVersion": 2,
    "sequences": { ... }
  },
  "records": [
    { "id": "document:document", "typeName": "document", ... },
    { "id": "page:xxx", "typeName": "page", ... },
    { "id": "shape:xxx", "typeName": "shape", "type": "geo", ... }
  ]
}
```

## Development

```bash
# Install dependencies
bun install

# Run in development mode
bun run dev

# Type check
bun run typecheck

# Build for distribution
bun run build

# Run tests
bun test
```

## Publishing

```bash
./scripts/publish.sh          # Auto-increment patch (0.1.0 → 0.1.1)
./scripts/publish.sh 0.2.0    # Use specific version
./scripts/publish.sh minor    # Bump minor (0.1.1 → 0.2.0)
./scripts/publish.sh major    # Bump major (0.2.0 → 1.0.0)
```

The script updates `package.json`, commits, tags, and pushes. GitHub Actions handles npm publish automatically via Trusted Publishers.

## Security

- **Path traversal prevention** — Relative paths can't escape `TLDRAW_DIR`
- **Format validation** — All writes are validated against tldraw schema
- **No network access** — Purely local file operations

## License

MIT © [Talha Orak](https://github.com/talhaorak)

## Related

- [tldraw](https://tldraw.com) — The infinite canvas
- [MCP](https://modelcontextprotocol.io) — Model Context Protocol
- [OpenClaw](https://openclaw.ai) — AI agent framework
