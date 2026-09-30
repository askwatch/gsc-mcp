# AskWatch Google Search Console MCP

Ask Google Search Console in plain language. Hosted, free, no Google Cloud project.

A hosted [MCP](https://modelcontextprotocol.io) server over Google Search Console. Connect your Google account once and AskWatch issues a personal link: no Cloud project, no `credentials.json`, nothing to run or host. Eleven tools answer whole questions rather than returning rows to interpret, and a raw Search Analytics query is there for everything else. Read-only at Google's level: the grant asks for `webmasters.readonly`, so it cannot submit, index or change anything.

**Get your link:** [askwatch.ai/free-tools/google-search-console-mcp](https://askwatch.ai/free-tools/google-search-console-mcp?ref=github)

> This repository holds the documentation, the registry entry and the issue tracker. The server itself is hosted by AskWatch at `https://mcp.askwatch.ai/gsc`; there is nothing here to install or run.

## Connect in about a minute

1. **Sign in** at [askwatch.ai](https://askwatch.ai/free-tools/google-search-console-mcp?ref=github) with your email. A six-digit code, no password, no card.
2. **Connect Google Search Console.** One consent screen, read-only.
3. **Paste your personal link** into your AI client. It looks like `https://mcp.askwatch.ai/gsc/your-personal-key`.

Then ask: *How did my site do last month?*

Treat the link like a password: whoever holds it can read the Search Console data it was issued for. You can regenerate it at any time, and the old one stops working immediately.

## Client setup

Replace `https://mcp.askwatch.ai/gsc/your-personal-key` with your own link.

<details>
<summary><b>Claude</b></summary>

1. Open Settings and go to Connectors.
2. Choose **Add custom connector**.
3. Paste your link as the server URL and save.

</details>

<details>
<summary><b>ChatGPT</b></summary>

1. Open Settings, go to Security and login and turn on **Developer mode**.
2. Go to Plugins, press **+**, then **Create app** and **Create MCP App**.
3. Paste your link as the MCP server URL and set Authentication to **No authentication**: the key is in the link.
4. Save, then pick the plugin with **+** in a new chat.

</details>

<details>
<summary><b>Claude Code</b></summary>

```bash
claude mcp add --transport http askwatch-gsc https://mcp.askwatch.ai/gsc/your-personal-key
```

</details>

<details>
<summary><b>Codex</b></summary>

```bash
codex mcp add askwatch-gsc --url https://mcp.askwatch.ai/gsc/your-personal-key
```

Or in `~/.codex/config.toml`, which the CLI, the IDE extension and the desktop app all read:

```toml
[mcp_servers.askwatch-gsc]
url = "https://mcp.askwatch.ai/gsc/your-personal-key"
```

</details>

<details>
<summary><b>Cursor</b></summary>

Settings → MCP → add a server:

```json
{
  "mcpServers": {
    "askwatch-gsc": {
      "url": "https://mcp.askwatch.ai/gsc/your-personal-key"
    }
  }
}
```

Also listed in the [Cursor Directory](https://cursor.directory/plugins/askwatch-google-search-console-mcp).

</details>

<details>
<summary><b>VS Code (Copilot Chat)</b></summary>

Run **MCP: Add Server** from the Command Palette, or write `.vscode/mcp.json` (one project) or your user configuration (all projects). The `type` field is required.

```json
{
  "servers": {
    "askwatch-gsc": {
      "type": "http",
      "url": "https://mcp.askwatch.ai/gsc/your-personal-key"
    }
  }
}
```

</details>

<details>
<summary><b>Windsurf</b></summary>

In `~/.codeium/windsurf/mcp_config.json` (or Settings → Cascade → MCP). Windsurf calls the field `serverUrl`, not `url`.

```json
{
  "mcpServers": {
    "askwatch-gsc": {
      "serverUrl": "https://mcp.askwatch.ai/gsc/your-personal-key"
    }
  }
}
```

</details>

<details>
<summary><b>Gemini CLI</b></summary>

In `~/.gemini/settings.json`, or `.gemini/settings.json` inside a project. The `httpUrl` key selects the streaming transport.

```json
{
  "mcpServers": {
    "askwatch-gsc": {
      "httpUrl": "https://mcp.askwatch.ai/gsc/your-personal-key"
    }
  }
}
```

Start the CLI and run `/mcp` to see the tools.

</details>

<details>
<summary><b>Any other client</b></summary>

Add it as a remote MCP server over **Streamable HTTP**. The address is the whole credential:

```
https://mcp.askwatch.ai/gsc/your-personal-key
```

If your client would rather send a header, the key works there too, on the bare address `https://mcp.askwatch.ai/gsc`:

```
Authorization: Bearer <your key>
```

</details>

## Tools

Seven answer a whole question, one passes anything through, three say what is there.

| Tool | What it returns |
| --- | --- |
| `site_overview` | Clicks, impressions, CTR and average position for a period, each against the period before it. |
| `top` | Top queries, pages, countries or devices, optionally narrowed to one country or device. |
| `changes` | What gained and what lost between two periods, with rows too small to mean anything filtered out. |
| `find_opportunities` | Queries ranking 5–20 whose click-through rate is below what that position usually earns. |
| `find_cannibalization` | Queries where more than one of your pages is being shown, and how each one does. |
| `page_queries` | What a specific page is actually shown for. |
| `brand_split` | Branded against non-branded search, split on the spellings you supply. |
| `query` | The Search Analytics query itself, with the dates, dimensions and filters Google takes. |
| `list_sites` | The properties this link can read, and your level of access to each. |
| `inspect_url` | Whether a URL is indexed, and what Google saw when it last crawled it. |
| `list_sitemaps` | The sitemaps a property has submitted, with their errors and last read. |

All eleven are read-only. Every answer states the period, the last day the data runs through, and the criteria it used.

## Things worth asking

- How did my site do last month?
- What dropped in the last 28 days, and on which pages?
- Where am I ranking 5 to 20 with a click-through rate below what that position usually gets?
- Which queries are pulling in two of my pages at once?
- What searches is /pricing actually shown for?
- How much of my traffic is branded?
- Is this URL indexed, and when was it last crawled?

## Why the numbers hold up

Search Console closes a day's data after a delay, and until it does the numbers for that day are incomplete. The server asks Google only for finalised data, and every answer says which day it runs through. Including the newest day once reported an 11.7% fall in clicks on a site that had fallen 1.1%.

## Access and privacy

- **Scope:** `webmasters.readonly`, plus `openid` and `email` so your AskWatch account can show which Google account is connected. The grant cannot submit a sitemap, request indexing, remove a URL or change a property.
- **Stored:** your Google tokens (encrypted), the list of properties your account can see, Google's answers cached for 30 minutes to 30 days depending on how old the data is, and a count of calls per tool per day. Not sold, not used to train models.
- **Revoking:** regenerate the link in AskWatch (the old one stops working at once), or remove AskWatch from your Google account's third-party access. Disconnecting deletes the grant and the cache read under it.
- **Limits:** a guard against runaway agents, not a ration. 20 calls a minute is the only hard refusal and comes back with a retry time. Past 300 calls a day, answers carry a notice and slow down slightly. URL inspection has its own budget of 100 a day, because Google meters that endpoint far more tightly.

[Privacy Policy](https://askwatch.ai/privacy) · [Terms](https://askwatch.ai/terms)

## Support

Found a bug or a client whose setup changed? [Open an issue](https://github.com/askwatch/gsc-mcp/issues). For anything else: hello@askwatch.ai.

## About AskWatch

[AskWatch](https://askwatch.ai/?ref=github) is an AI visibility tracker: it shows how often ChatGPT, Perplexity, Gemini, Copilot and Google AI name your brand, where you rank in the answer, and which pages they cite. This server is free, with nothing to upgrade to.

## License

The documentation in this repository is MIT-licensed. The hosted service is covered by the [AskWatch Terms](https://askwatch.ai/terms).
