
# 1) GitHub Step-by-Step Doc (paste into your repo)

##  `lab-05-finops-reality-check-cost-explorer-mcp`

**Lab 05 — FinOps Reality Check with Cost Explorer MCP (Estimate vs Reality)**
*Goal:* Use **Cost Explorer MCP** to answer FinOps questions in plain English using real billing data.

### What you’ll build

You’ll enable Amazon Q CLI to call Cost Explorer via MCP, then run:

1. **Last 30 days spend by service + top 3 drivers**
2. **Last 7 days vs previous 7 days comparison + explanation**

### Prerequisites

* Ubuntu/WSL working
* `aws sts get-caller-identity` works (you already validated)
* `q chat` works
* AWS billing permissions for Cost Explorer:

  * `ce:GetCostAndUsage` (and related Cost Explorer read APIs)
  * In many orgs, these are restricted to finance/billing roles

---

## Step 1 — Verify AWS identity (must pass)

```bash
aws sts get-caller-identity
```

Expected:

* You see your **Account**, **UserId**, **Arn**
* No SignatureExpired / SSO token errors

---

## Step 2 — Ensure Q is logged in

```bash
q login
```

Then:

```bash
q chat
```

---

## Step 3 — Confirm MCP config file location

Amazon Q CLI reads MCP server config from:

```bash
cat ~/.aws/amazonq/mcp.json
```

You should see a `mcpServers` section.

---

## Step 4 — Add Cost Explorer MCP server to `mcp.json`

Edit:

```bash
nano ~/.aws/amazonq/mcp.json
```

Add (or merge) something like this under `mcpServers`:

```json
{
  "mcpServers": {
    "cost-explorer": {
      "command": "uvx",
      "args": ["awslabs.cost-explorer-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "mcp01",
        "AWS_REGION": "ap-south-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

**Notes**

* Keep `AWS_PROFILE` = the profile you use for AWS CLI (`mcp01`)
* Region can stay `ap-south-1` (Cost Explorer is global-ish in behavior, but keep consistent)
* `autoApprove` is intentionally empty to keep guardrails

---

## Step 5 — Restart Q chat (important)

Exit and reopen Q so it loads the MCP servers again.

```bash
q chat
```

Check MCP load status:

* You should see `cost-explorer` appear under `/tools`

In chat:

```
/tools
```

---

#  Use Case #2A — “Show my AWS spend for last 30 days grouped by service”

### Prompt (paste into Q chat)

```
Show my AWS spend for the last 30 days grouped by service.
Highlight the top 3 cost drivers.
```

### What you should expect

* A table (or summary) grouped by service
* “Top 3” ranked drivers
* Sometimes credits show as negative values (important insight)

### How to interpret results

* If total is near $0: you may be in Free Tier or credits offset costs
* Even $0 can still reveal **behavior patterns** (sporadic usage, service spikes)

---

#  Use Case #2B — “Compare last 7 days vs previous 7 days”

### Prompt (paste into Q chat)

```
Compare AWS costs for the last 7 days vs the previous 7 days.
Explain which services changed and why.
```

### What you should expect

* A week-over-week delta
* Services that increased/decreased
* A narrative “why” (e.g., fewer compute-hours, reduced testing, seasonal behavior)

### Your real output pattern (example from your run)

* Total down ~77% WoW
* Primary change: `EC2 - Other`
* Credits adjust proportionally

### Why this matters (FinOps “gold”)

This directly answers:

* “Why did our bill change?”
* “Which service drove it?”
* “Was it a usage shift or pricing shift?”

---

## Troubleshooting

### Problem: `Signature expired`

Your WSL clock is behind. Fix by restarting WSL:

From Windows PowerShell:

```powershell
wsl --shutdown
wsl -d Ubuntu
```

### Problem: MCP server fails to load

* Confirm `uvx` exists:

```bash
which uvx
uvx --version
```

* Confirm MCP JSON is valid JSON (no trailing commas)
* Reopen `q chat`

---

## Cleanup / Safety Notes

* Cost Explorer MCP is read-only, but still treat it as production-facing data.
* Don’t paste account-wide cost details into public posts; publish insights, not raw bills.

