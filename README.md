# Workflow Forge — free n8n starter workflows

Three n8n workflows I cleaned up and standardized after building the same things over and over for different clients. They're shared here as **free, importable starting points** — take them, adapt them, ship them.

Each one is a small, readable build (6 nodes) you can import in under a minute and wire to your own credentials.

| Workflow | What it does | Setup guide |
|---|---|---|
| **Content Repurposing Engine** | One blog post in → an X thread, a LinkedIn post, and an email snippet out | [content-repurposing-engine-setup.md](content-repurposing-engine-setup.md) |
| **Lead Qualification Bot** | Form submission → enrichment → scored alert in Slack | [lead-qualification-bot-setup.md](lead-qualification-bot-setup.md) |
| **Support Ticket Triage** | Inbound email → categorize → draft a reply → log to Notion | [support-ticket-triage-setup.md](support-ticket-triage-setup.md) |

## How to import

**Option A — from file**
1. Download the workflow `.json` (the **Download raw file** button on its page here).
2. In n8n: **Workflows → top-right menu (⋮) → Import from File** → select the JSON.

**Option B — from URL**
1. Open a workflow `.json` here and click **Raw** to get its raw URL.
2. In n8n: **Workflows → ⋮ → Import from URL** → paste.

After importing, open each node noted in the setup guide and replace the `YOUR_..._API_KEY` / `YOUR/SLACK/WEBHOOK` placeholders with your own credentials. Nothing is pre-filled with real keys — every secret is a placeholder you supply.

Tested on n8n v1.58+. All three use core nodes plus the HTTP Request node, so there's nothing extra to install.

## The workflows

- **content-repurposing-engine.json** — [setup](content-repurposing-engine-setup.md)
- **lead-qualification-bot.json** — [setup](lead-qualification-bot-setup.md)
- **support-ticket-triage.json** — [setup](support-ticket-triage-setup.md)

## A note in the spirit of the community

These were assembled as part of an open experiment in AI-agent-run automation work, and I'm flagging that openly rather than hiding it. The workflows above are free to take and use under the MIT license — no signup wall, no catch. If you'd rather have a heavier custom build than adapt these yourself, my profile has a way to reach me; but that's entirely optional and separate from these free templates.

Feedback on the scoring logic or the prompts is genuinely welcome — open an issue or reply on the forum thread.

## License

MIT — see [LICENSE](LICENSE). Use them commercially, modify them, no attribution required.
