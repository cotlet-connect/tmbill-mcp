# TMBill MCP server

Connect TMBill to ChatGPT, Claude and any AI agent through Cotlet.dev. Read outlet reports, inventory and staff with 3 hosted read tools in your own TMBill backoffice. These workflows change nothing.

## What you can ask

> Show yesterday’s sales for every outlet.

> List stock on hand before my supplier order.

> Show the staff and roles at each outlet.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps & Connectors → Create, with connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev. No API key needed.

### Claude

Settings → Connectors → Add custom connector, with URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its TMBill tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run the included Python module with Python 3.10 or later:

```bash
python3 tmbill_agent.py serve
```

Or with Docker:

```bash
docker build -t tmbill-mcp .
docker run -i --rm -e COTLET_API_KEY tmbill-mcp
```

Tools: `tmbill_guide`, `tmbill_status`, `tmbill_open`, `tmbill_read`, `tmbill_click`, `tmbill_type`, `tmbill_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, with no third-party runtime dependencies. Clone this repository and run:

```bash
export COTLET_API_KEY="your-key"
python3 tmbill_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 tmbill_agent.py status    # is TMBill ready in your Cotlet.dev account
python3 tmbill_agent.py guide     # TMBill screens and sign-in link
python3 tmbill_agent.py read      # open TMBill, read the page, close the browser
python3 tmbill_agent.py tools     # list Cotlet.dev tools
python3 tmbill_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from TMBill users

**Which TMBill login should I use?**

Sign in at https://backoffice.tmbill.com/ in your Cotlet Cloud Browser. Pick Company or Management and use your password or emailed OTP. The agent sees the outlets your login can open.

**Can the agent change orders or stock?**

No. The three hosted TMBill tools only read outlet reports, inventory and staff. Billing, menus, prices, purchases and user rights stay with you.

**Is Cotlet affiliated with TMBill?**

No. Cotlet.dev is independent and not affiliated with TMBill or Techmainstay Software Private Limited.

## Learn more

- [TMBill integration and hosted tools](https://cotlet.dev/integrations/tmbill-api-mcp-server)

---

More: https://cotlet.dev/integrations/tmbill-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=tmbill

Request access: https://cotlet.dev/signup?app=tmbill&utm_source=github&utm_medium=readme&utm_campaign=tmbill

## Workflows on the hosted integration

The live integration page documents these 3 operations:

- `view_outlet_reports`
- `track_inventory`
- `view_staff`

The local stdio adapter exposes seven browser helpers listed above. These helpers forward to Cotlet.dev; the workflow operations are documented on the hosted integration page.
