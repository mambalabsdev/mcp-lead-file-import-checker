# Lead File Import Checker MCP Server

[![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-lead-file-import-checker)](https://www.npmjs.com/package/@mambalabsdev/mcp-lead-file-import-checker) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-lead-file-import-checker)](https://github.com/mambalabsdev/mcp-lead-file-import-checker/blob/main/LICENSE)

An MCP server for the Mamba Labs Lead File Import Checker actor on Apify. Give it lead rows, a CSV link, or an Apify dataset. Every row comes back with an import, review, or reject verdict and the issues behind it, so only clean rows reach your CRM or sequencer.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Your keys](#your-keys)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Checks per row: email syntax, duplicates (the way inboxes see them, so `Jane+x@Stripe.com` matches `jane@stripe.com`), free mail, role inboxes, disposable domains, DNS and MX for the email domain, one homepage read per company domain for live, parked, and company name checks, missing required fields, and stale dates. It does not verify mailboxes and never contacts a mail server. All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-lead-file-import-checker": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-lead-file-import-checker"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `check_lead_file` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Check this lead CSV and give me only the rows marked import."
- "How many duplicates and role addresses are in this list?"
- "Which of these companies have a parked or dead website?"

## Tool

`check_lead_file`. One tool. Example arguments:

```json
{
  "rows": [
    { "Email": "jane@stripe.com", "Company": "Stripe" },
    { "Email": "info@gong.io", "Company": "Gong" }
  ]
}
```

## Inputs

Every buyer facing input of the actor, unchanged. Omitted inputs take the actor's own defaults. Numbers and yes or no options are sent as strings.

| Input | Type | Description |
|---|---|---|
| `rows` | array | Lead rows as objects, up to 50,000 |
| `csv_url` | string | A public CSV link, up to 50 MB |
| `dataset_id` | string | An Apify dataset ID |
| `column_map` | object | Which column holds each field |
| `required_fields` | array | Fields a row must have. Default email |
| `checks` | array | Which checks to run. Default all |
| `check_websites` | string | true (default) or false |
| `stale_days` | string | Days before a dated row is stale |
| `max_rows` | string | Default 5000, at most 50000 |
| `max_charge_usd` | string | Optional cost cap for the run |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `row-checked`, per row of your file checked | $0.002 | $0.0019 | $0.0018 | $0.0017 |
| `apify-actor-start`, per run start | $0.0005 | $0.0005 | $0.0005 | $0.0005 |

Blank rows, rows over `max_rows`, and the one row a refused input writes are free.

## Your keys

The actor needs no key and holds none. It reads DNS and the companies' own homepages. Mamba Labs pays the platform costs of the run.

## Full actor documentation

[apify.com/mambalabs/lead-file-import-checker](https://apify.com/mambalabs/lead-file-import-checker)

## License

MIT. See [LICENSE](LICENSE).
