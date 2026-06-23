# Lead Qualification Bot — setup

Stop reading every raw form fill. Inbound leads come in via webhook, get enriched and scored, and only the qualified ones fire a Slack alert — the rest are logged quietly.

## Flow (6 nodes)

1. **Webhook Trigger** — `POST` from your form/landing page. Expects at least `email`, optionally `name` and `message`/`notes`.
2. **Normalize Input** — lowercases/trims the email and requires a valid-looking address (throws if missing).
3. **Enrich (Clearbit)** — HTTP Request to `person.clearbit.com/v2/combined/find` to pull role + company.
4. **Score & Format** — simple, editable rules: has a role → 80, known company → 60, otherwise → 30.
5. **Notify Slack** — posts the scored lead to a Slack incoming webhook.
6. **Respond OK** — acknowledges the form submission.

## What you need to fill in

| Node | Placeholder | Replace with |
|---|---|---|
| Enrich (Clearbit) | `Authorization: Bearer YOUR_CLEARBIT_API_KEY` | Your Clearbit API key |
| Notify Slack | `https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK` | Your Slack incoming-webhook URL |

No Clearbit account? Delete that node and have **Score & Format** read straight from the normalized fields — the scoring rules degrade gracefully (you'll just lean on the "known company" / fallback tiers).

## Test it

```bash
curl -X POST https://YOUR-N8N-HOST/webhook/<your-webhook-path> \
  -H "Content-Type: application/json" \
  -d '{"email":"jane@acme.com","name":"Jane Doe","message":"Interested in a demo"}'
```

A qualifying lead should land in your Slack channel within a second or two.

## Customize

- The scoring thresholds live in **Score & Format** — change the numbers or add signals (title keywords, company size, UTM source).
- Add an IF node after scoring to branch high-score leads to a CRM (Google Sheets, HubSpot) while low-score ones are only logged.
