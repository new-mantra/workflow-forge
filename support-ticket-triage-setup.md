# Support Ticket Triage — setup

Take the first pass on a support inbox automatically. An inbound email is categorized, a reply is drafted, and the ticket is logged — so a human is editing a draft instead of starting cold.

## Flow (6 nodes)

1. **Email Trigger (IMAP)** — watches a support mailbox for new mail.
2. **Heuristic Pre-Label** — fast keyword pass tags obvious categories (billing / bug / general) before the model call, so simple tickets are cheap.
3. **Claude Triage + Draft** — HTTP Request to `api.anthropic.com/v1/messages`. Returns a `CATEGORY`, a `DRAFT` reply, and an `ASSIGNEE`.
4. **Parse Triage** — extracts those fields from the response, with safe fallbacks if the model output is unexpected.
5. **Log to Notion** — creates a page in your Notion tickets database.
6. **Notify Slack** — posts the category + draft to Slack for a human to approve/edit.

## What you need to fill in

| Node | Placeholder | Replace with |
|---|---|---|
| Email Trigger (IMAP) | IMAP credential | Your support mailbox (host, user, password/app-password) — set in the node's credential picker |
| Claude Triage + Draft | `x-api-key: YOUR_ANTHROPIC_API_KEY` | Your Anthropic API key |
| Log to Notion | `Authorization: Bearer YOUR_NOTION_API_KEY` | Your Notion integration token |
| Notify Slack | `https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK` | Your Slack incoming-webhook URL |

The `Notion-Version` and `anthropic-version` headers are already set.

> Tip: in your Notion database, make sure the property names the **Log to Notion** node writes to (e.g. title, category, assignee) match your columns, or adjust the request body to fit your schema.

## Test it

Send a test email to the monitored mailbox (e.g. subject "Refund request" with a sentence of body). Within a polling cycle you should see a categorized draft appear in Slack and a new row in Notion.

## Customize

- Tune the keyword rules in **Heuristic Pre-Label** to match your product's vocabulary.
- Edit the prompt in **Claude Triage + Draft** to set tone and your support policy (refund window, escalation rules).
- Drop the Notion node if you log elsewhere — the Slack draft is the core human-in-the-loop step.
