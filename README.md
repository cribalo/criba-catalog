# criba bot catalog

**Who is really calling your MCP server or A2A agent?** This is the open catalog behind
[criba](https://criba.cribalo.com/): known clients of the AI-agent ecosystem, with the User-Agent
patterns and MCP `clientInfo.name` values they send, and what they were seen doing.

The catalog comes from real traffic to a declared research decoy,
[mcp-senuelo](https://github.com/cribalo/mcp-senuelo) (an open MCP server and A2A agent with fake
tools, listed in the official MCP Registry). Most of what reaches an open MCP server turns out to be
directory crawlers, verifiers and scanners, not agents doing work. Telling them apart is what
this catalog is for.

## What is in it

[`catalog.json`](catalog.json): catalog version `2026-11-11`, 79 entries.

| Category | Entries | What they are |
|---|---|---|
| `mcp-indexer` | 25 | directory and catalog crawlers: discover or initialize, then `tools/list`, nothing else |
| `mcp-verifier` | 22 | health, conformance, security or "agent-friendliness" checks |
| `agent-runtime` | 7 | something that says it is an agent or agent runtime |
| `generic-client` | 6 | HTTP libraries and runtimes (`node`, `python-httpx`…): say nothing about who is behind them |
| `security-scanner` | 5 | internet-wide scanners and threat-intel crawlers |
| `commercial-crawler` | 5 | lead enrichment and generic SEO crawlers |
| `a2a-directory` | 3 | A2A directories: read the Agent Card, sometimes ping the endpoint |
| `agent-marketplace` | 2 | marketplaces that list, probe and message agents |
| `research` | 2 | academic or census scanners |
| `ai-crawler` | 2 | web crawlers of AI companies |

Each entry:

```json
{
  "id": "glama",
  "name": "Glama",
  "category": "mcp-indexer",
  "url": "https://glama.ai",
  "protocols": ["mcp"],
  "clientNames": ["glama"],
  "seen": "initialize (declares tasks capability), tools/list"
}
```

- `ua`: case-insensitive regular expression matched against the User-Agent (optional).
- `clientNames`: exact, lower-cased MCP `clientInfo.name` values (optional).
- `seen`: what it did on the decoy, in a few words.

A match is a **declaration, not proof**: anyone can send any User-Agent. criba keeps the declared
identity apart from the observed behaviour, and flags a mismatch when they disagree.

## Use it without downloading anything

- **MCP server:** `https://criba.cribalo.com/mcp` (Streamable HTTP, no authentication; official MCP
  Registry name `com.cribalo/criba`). Tools: `identify_client`, `classify_visitor`, `list_known_bots`.
- **HTTP API:** `GET https://criba.cribalo.com/v1/identify?ua=…&client=…`,
  `POST https://criba.cribalo.com/v1/classify`, `GET https://criba.cribalo.com/v1/bots`.
  Reference: [docs.txt](https://criba.cribalo.com/docs.txt).

Nothing sent to the API or the MCP server is stored.

Example MCP client configuration:

```json
{
  "mcpServers": {
    "criba": { "type": "http", "url": "https://criba.cribalo.com/mcp" }
  }
}
```

## Contributing

Seen a bot that is missing or wrong? Open an issue with the User-Agent, the `clientInfo.name` if it
is an MCP client, and what it did (methods, paths). Please do not include IP addresses or personal data.

## Licence

The catalog is licensed under
[Creative Commons Attribution-NonCommercial 4.0 International](https://creativecommons.org/licenses/by-nc/4.0/)
(`CC-BY-NC-4.0`, full text in [LICENSE](LICENSE)). Non-commercial use is free with attribution:
"criba agent-ecosystem bot catalog, cribalo.com". Commercial use needs a separate licence:
contact@cribalo.com.

`catalog.json` is generated from criba's source; changes land here with each catalog version.
