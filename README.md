# @pipeworx/dwds

[DWDS](https://www.dwds.de) MCP — German Digital Dictionary (Digitales Wörterbuch der deutschen Sprache). Keyless.

Part of [Pipeworx](https://pipeworx.io) — an MCP gateway connecting AI agents to 1558+ live data sources.

## Tools

- `snippet(query)` — dictionary entry summary for a German word (part of speech, base lemma, link to full entry). Exact lemmas only.
- `dwds_frequency(query)` — how common a German word is: absolute hit count + per-million frequency in the DWDS reference corpus.

## Auth

None — keyless.

## Removed tools

- `lemma` — removed 2026-08-07. Called `/api/lemma/`, which 404s for every input; the endpoint was never documented and never worked. No keyless lemmatizer replacement exists (`/wb/snippet`, `dwdsmor`, `wb/lemma` all fail on inflected forms).
- `corpus_concordance` / `kwic` — removed 2026-08-31 (fleet #729). DWDS retired its public corpus-concordance / KWIC API; corpus search moved to the access-restricted "dstar" platform (`ddc.dwds.de`), which no longer serves JSON. No keyless replacement exists. For raw corpus example sentences, search manually at https://www.dwds.de.

## Data sources

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

### What this endpoint actually serves

`tools/list` at `https://gateway.pipeworx.io/dwds/mcp` returns the tools in the table
above **plus the shared Pipeworx meta-tools** — `ask_pipeworx`,
`discover_tools`, `search_within`, `remember`/`recall` and the rest of the
gateway-wide set. So the tool count you see is larger than this table: a
single-pack endpoint currently lists roughly 30 shared tools alongside the
pack's own. The connection's `initialize` response states its exact scope, and
is the authoritative answer for a given day.

This is deliberate, not multiplexing by accident. The meta-tools are what let a
scoped connection answer a question this pack does not cover — via
`ask_pipeworx`, which routes across the whole catalog — without you adding a
second MCP server. There is currently no way to mount a pack endpoint without
them; if the extra schemas cost you more context than the routing is worth,
connect to the full gateway once rather than to several pack endpoints.

Or connect to the full Pipeworx gateway to get every pack's tools listed
directly, instead of just this one's:

```json
{
  "mcpServers": {
    "pipeworx": {
      "url": "https://gateway.pipeworx.io/mcp"
    }
  }
}
```

Both URLs reach the same gateway and the same 1558+ data sources. The
only difference is which pack's tools are listed **directly**; `ask_pipeworx`
reaches all of them from either one.

## Standalone (no gateway account)

This package also runs as a local stdio MCP server — no Pipeworx account, no
gateway round-trip:

```json
{
  "mcpServers": {
    "dwds": {
      "command": "npx",
      "args": ["-y", "@pipeworx/mcp-dwds"]
    }
  }
}
```

Or run it directly to confirm it starts:

```bash
npx -y @pipeworx/mcp-dwds
```

It speaks MCP over stdin/stdout and answers `initialize`/`tools/list`/`tools/call`
for **only** this pack's tools — none of the shared meta-tools the gateway
connection above adds. Same source, same tools, no ask_pipeworx routing.

## Using with ask_pipeworx

Instead of calling tools directly, you can ask questions in plain English —
this works on the pack endpoint above as well as on the full gateway:

```
ask_pipeworx({ question: "your question about Dwds data" })
```

The gateway picks the right tool and fills the arguments automatically.

## More

- [Docs and guides](https://pipeworx.io/docs)
- [pipeworx.io](https://pipeworx.io)

## License

MIT
