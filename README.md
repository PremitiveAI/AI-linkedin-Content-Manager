# AI LinkedIn Content Manager

**Simple explanation:** A tool that helps you create LinkedIn posts automatically. You just tell it what topic you want to write about, who you want to reach, and the AI will write a complete post, find a picture for it, and send it to you for approval via email before posting.

**Who should use this?**
- LinkedIn content creators who want to post regularly but save time writing
- Business owners who need fresh content but don't have a dedicated writer
- Marketers managing multiple LinkedIn accounts
- Anyone who wants professional-looking posts without the writing effort

**How it works:**
1. You fill out a quick form with your topic and details
2. AI researches and writes a professional post
3. AI finds and adds a relevant image
4. You get an email asking for approval
5. Approve it and it publishes to LinkedIn automatically

**The workflow flow:**
```
You submit topic (site/)  -->  AI researches  -->  AI writes post  -->  AI finds image
                                                            |
                                                            v
                         Publishes <-- You approve (via Gmail) <-- Review
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
   and file a brief. Approve, regenerate, or edit it from the Gmail approval
   email when it lands.

Full logic walkthrough: [`docs/WORKFLOW.md`](docs/WORKFLOW.md).
# AI-linkedin-Content-Manager