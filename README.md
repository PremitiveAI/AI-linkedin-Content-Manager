# AI LinkedIn Content Manager

An n8n workflow that turns a short brief (topic, category, audience) into a
researched, written, and image-illustrated LinkedIn post — held for human
approval over Telegram before it publishes.

```
Wire Ticket (site/)  --POST-->  Webhook  -->  Validate  -->  Strategy
                                                                 |
                                                                 v
                              Publish <-- Approval (Telegram) <-- Research -> Draft -> Review -> Image
```

**Repo contents:**

| Path | What it is |
|---|---|
| `n8n-workflow/AI_LinkedIn_Content_Manager.json` | The importable n8n workflow |
| `site/` | Static intake form ("Assignment Desk") — deploys to Netlify, posts to the workflow's webhook |
| `docs/WORKFLOW.md` | Node-by-node explanation of the flow and its logic |
| `docs/CREDENTIALS.md` | How to plug in your own OpenAI, Telegram, and LinkedIn credentials |
| `docs/DEPLOY_NETLIFY.md` | How to put `site/` live on Netlify |

## Quickstart

1. **Import the workflow** into n8n: Workflows → Import from File →
   `n8n-workflow/AI_LinkedIn_Content_Manager.json`.
2. **Add your own credentials** — see [`docs/CREDENTIALS.md`](docs/CREDENTIALS.md).
   Nothing in this repo contains a real API key; you connect your own inside n8n.
3. **Activate the workflow** and copy the Webhook node's Production URL.
4. **Deploy `site/` to Netlify** — see [`docs/DEPLOY_NETLIFY.md`](docs/DEPLOY_NETLIFY.md).
5. Open the deployed site, paste the webhook URL into the "Wire endpoint" field,
   and file a brief. Approve or reject it from Telegram when it lands.

Full logic walkthrough: [`docs/WORKFLOW.md`](docs/WORKFLOW.md).
# AI-linkedin-Content-Manager
