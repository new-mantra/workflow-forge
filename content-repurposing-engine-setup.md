# Content Repurposing Engine — setup

Turn one piece of long-form writing into three channel-ready drafts in a single call: an X/Twitter thread, a LinkedIn post, and an email snippet. Good for solo creators who write once and want to distribute everywhere without re-drafting by hand.

## Flow (6 nodes)

1. **Webhook Trigger** — `POST` to path `repurpose-content`. Send a JSON body with the article in `text`, `article`, or `content`.
2. **Validate Input** — rejects payloads under 50 characters so you don't burn an API call on an empty request.
3. **Claude Repurpose** — HTTP Request to `api.anthropic.com/v1/messages`. One prompt asks for all three formats in a delimited block.
4. **Parse Outputs** — splits the model response on `===TWITTER_THREAD===` / `===LINKEDIN_POST===` / `===EMAIL_SNIPPET===` markers into clean fields.
5. **Format Markdown** — assembles a tidy Markdown document of the three outputs.
6. **Respond to Webhook** — returns the result to the caller.

## What you need to fill in

| Node | Placeholder | Replace with |
|---|---|---|
| Claude Repurpose | `x-api-key: YOUR_ANTHROPIC_API_KEY` | Your Anthropic API key |

The `anthropic-version` header is already set. No other credentials required.

## Test it

```bash
curl -X POST https://YOUR-N8N-HOST/webhook/repurpose-content \
  -H "Content-Type: application/json" \
  -d '{"text":"<paste at least a paragraph or two of an article here>"}'
```

You should get back a Markdown doc with the three repurposed formats.

## Customize

- Edit the prompt in **Claude Repurpose** to change tone, length, or add a fourth channel.
- Swap the model by changing the request body (e.g. a smaller/cheaper Claude model for volume).
- The delimiter parsing lives in **Parse Outputs** — keep the `===SECTION===` markers in the prompt and the parser in sync if you add formats.
