# Bolster MCP server

Connect Bolster to ChatGPT, Claude, Codex and Claude Code through Cotlet.dev. Work with estimates, proposals, schedules, job budgets, bid requests and customer payment requests in your own signed-in contractor account. Review write operations before approving them.

## What you can ask

> Show which clients opened their estimate proposals.

> Draft an estimate for a composite deck and show me the lines before saving.

> List payment requests for the Maple Street job.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev. No API key needed.

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

This repo is a real MCP server. It speaks MCP over stdio, lists its Bolster tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run the included Python module with Python 3.10 or later:

```bash
python3 bolster_agent.py serve
```

Or with Docker:

```bash
docker build -t bolster-mcp .
docker run -i --rm -e COTLET_API_KEY bolster-mcp
```

Tools: `bolster_guide`, `bolster_status`, `bolster_open`, `bolster_read`, `bolster_click`, `bolster_type`, `bolster_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, with no third-party runtime dependencies. Clone this repository and run:

```bash
export COTLET_API_KEY="your-key"
python3 bolster_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 bolster_agent.py status    # is Bolster ready in your Cotlet.dev account
python3 bolster_agent.py guide     # Bolster screens and sign-in link
python3 bolster_agent.py read      # open Bolster, read the page, close the browser
python3 bolster_agent.py tools     # list Cotlet.dev tools
python3 bolster_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Bolster users

**How do I connect my account?**

Request access at https://cotlet.dev/signup?app=bolster, sign in to Bolster in your Cotlet.dev Cloud Browser and enable it for agents.

**Can I review changes?**

Write calls ask for confirmation by default; you can change that per tool. Sending proposals and bid requests reaches your clients or vendors. Approving a finished schedule task can start a payment request.

**Which plan do I need?**

The live connection guide documents takeoff and scheduling from Pro, and RFQ management from Premium. Your Bolster plan and user permissions determine what the agent can access.

**Is Cotlet.dev affiliated with Bolster?**

No. Cotlet.dev is independent and not affiliated with Bolster. No tool pays vendor bills or moves money out of Bolster Wallet.

## Learn more

- [bolster integration and hosted tools](https://cotlet.dev/integrations/bolster-api-mcp-server)

---

More: https://cotlet.dev/integrations/bolster-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=bolster

Request access: https://cotlet.dev/signup?app=bolster&utm_source=github&utm_medium=readme&utm_campaign=bolster

## Workflows on the hosted integration

Examples documented on the live integration page:

- `manage_crm_pipeline`
- `generate_ai_estimates`
- `measure_takeoffs`
- `create_estimates`
- `manage_selections`
- `create_presentations`
- `schedule_jobs`
- `manage_job_budget`
- `send_rfq_requests`
- `collect_customer_payments`

The local stdio adapter exposes seven browser helpers listed above. These helpers forward to Cotlet.dev; the workflow operations are documented on the hosted integration page.
