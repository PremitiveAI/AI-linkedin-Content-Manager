# Deploying `site/` to Netlify

The site is a single static file (`site/index.html`) — no build step, no
dependencies, no environment variables required at build time.

## Option A — connect the GitHub repo (recommended)

1. Push this repo to GitHub.
2. In Netlify: **Add new site → Import an existing project → GitHub** →
   select this repo.
3. Netlify reads `netlify.toml` at the repo root automatically:
   - Publish directory: `site`
   - Build command: none
4. Click **Deploy site**.

Every push to your default branch redeploys automatically.

## Option B — drag and drop

1. Zip or select just the `site/` folder contents (must contain `index.html`
   at the top level of what you drop).
2. Netlify → **Add new site → Deploy manually** → drag the folder in.

## After deploying

1. Open the live URL.
2. Paste your n8n Webhook's **Production URL** into the "Wire endpoint"
   field on the page — it's saved in the browser's local storage, so you
   only set it once per device.
3. **Enable CORS on the n8n side**: open the `LinkedIn Content Webhook`
   node → Options → **Allowed Origins (CORS)** → set to your Netlify
   domain (e.g. `https://your-site.netlify.app`) or `*` while testing.
   Without this, the browser will block the request with a CORS error.

## Custom domain (optional)

Netlify → **Domain settings → Add a domain** — follow their DNS
instructions. No changes needed on the site itself.
