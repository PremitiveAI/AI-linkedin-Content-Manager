# Adding your own credentials

**No API key ever lives in this repo.** The workflow JSON only contains
placeholder credential references (`REPLACE_WITH_YOUR_..._CREDENTIAL_ID`).
When you import the workflow, n8n will flag every node that needs a
credential — you connect your own from inside the n8n UI, where n8n encrypts
and stores it. This is standard n8n practice and the only safe way to do it;
raw keys should never be committed to Git.

After importing `n8n-workflow/AI_LinkedIn_Content_Manager.json`:

## 1. OpenAI (ChatGPT) key

Used by: `Strategy Model`, `Research Model`, `Post Model`, `Concept Model`,
`Image Generation`, `Review Model`, `Editor Model`, `Optimizer Model`.

1. In n8n: **Credentials → Add Credential → OpenAI**.
2. Paste your key from <https://platform.openai.com/api-keys>.
3. Save it, then open each of the 8 nodes listed above and select this
   credential from the dropdown (n8n will prompt you for most of these
   automatically on import since the placeholder ID won't resolve).

## 2. Gmail (for approvals)

1. In n8n: **Credentials → Add Credential → Gmail OAuth2 API** and complete
   Google's OAuth flow (the credential screen links to the Google Cloud
   Console and lists the exact scopes/redirect URL to use).
2. Open the **`Approval Gmail`** node (this is the node that used to be
   `Approval Telegram`) and select this credential.
3. In that same node, set **`sendTo`** to the email address that should
   receive and approve drafts.
4. That's it — the node emails the draft with **Approve / Regenerate / Edit**
   links built from `{{ $execution.resumeUrl }}`; clicking one resumes the
   paused workflow with that action, same as the old Telegram buttons did.

## 3. LinkedIn

1. In n8n: **Credentials → Add Credential → LinkedIn OAuth2 API**.
2. Follow n8n's OAuth flow to connect your LinkedIn profile (you'll need a
   LinkedIn app — n8n's credential screen links directly to the developer
   portal and lists the exact redirect URL/scopes to use).
3. Open **`Publish to LinkedIn`** and select this credential.

## 4. Activate

Once all nodes show a green credential (no more red warning triangles),
toggle the workflow **Active**, then open the Webhook node and copy its
**Production URL** — that's what goes into the site's "Wire endpoint" field.