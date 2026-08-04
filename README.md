# @pipeworx/dwds

[DWDS](https://www.dwds.de) MCP — German Digital Dictionary (Digitales Wörterbuch der deutschen Sprache). Keyless.

Part of [Pipeworx](https://pipeworx.io) — an MCP gateway connecting AI agents to 1394+ live data sources.

## Tools

- `snippet(query)` — JSON dictionary snippet for a German word
- `lemma(form)` — lemma resolution (form → base form)
- `corpus_concordance(query, corpus?, limit?)` — search the DWDS corpus
- `kwic(query, corpus?, limit?)` — keyword-in-context view

## Data source

`https://www.dwds.de/api/`

## Quick Start

Add to your MCP client (Claude Desktop, Cursor, Windsurf, etc.):

```json
{
  "mcpServers": {
    "dwds": {
      "url": "https://gateway.pipeworx.io/dwds/mcp"
    }
  }
}
```

Or connect to the full Pipeworx gateway for access to all 1394+ data sources:

```json
{
  "mcpServers": {
    "pipeworx": {
      "url": "https://gateway.pipeworx.io/mcp"
    }
  }
}
```

## Using with ask_pipeworx

Instead of calling tools directly, you can ask questions in plain English:

```
ask_pipeworx({ question: "your question about Dwds data" })
```

The gateway picks the right tool and fills the arguments automatically.

## More

- [Docs and guides](https://pipeworx.io/docs)
- [pipeworx.io](https://pipeworx.io)

## License

MIT
