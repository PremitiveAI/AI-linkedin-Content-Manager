# How the workflow works

## 1. Intake

**`LinkedIn Content Webhook`** — a `POST` endpoint. Accepts:

```json
{ "Topic": "...", "Category": "...", "Target Audience": "..." }
```

It responds immediately (`"status": "received"`) so the caller — the
`site/` form, curl, Zapier, whatever — doesn't sit waiting through the
whole pipeline below.

**`Validate Input`** reads the body, trims/cleans it, and checks:
- Topic is present and at least 3 characters
- Category is one of a fixed list (AI & Automation, Business, Marketing, etc.)
- Target Audience is present

**`Input Valid?`** branches: bad input → **`Validation Failed`** (records the
reason and stops). Good input continues.

## 2. Strategy → Research → Draft

- **`Content Strategist`** (OpenAI agent) turns the brief into an angle: hook
  direction, structure, tone, and whether the topic needs outside research.
- **`Research Needed?`** — if the strategist flagged it, **`Research`** (another
  agent) gathers supporting points; otherwise the flow logs **`No Research`**
  and skips ahead.
- **`Post Generator`** writes the actual LinkedIn post from the strategy (+
  research, if any).
- **`Image Concept`** decides on an image direction for the post, and
  **`Image Generation`** (OpenAI image model) renders it.
- **`Quality Review`** — an agent grades the draft against a rubric.
  **`Review Passed?`** either continues to `Finalize (Original)`, or on
  failure ends the run at **`Generation Failed`**.

Every agent node (`Content Strategist`, `Research`, `Post Generator`,
`Image Concept`, `Quality Review`, `Editor`, `Final Optimizer`) runs on its
own `lmChatOpenAi` model node and a matching structured-output schema, so
each step returns predictable JSON instead of free text.

## 3. Human approval — Gmail

This is the gate before anything goes live.

- **`Current Content`** snapshots the current draft, image, hashtags, etc.
  into one clean object.
- **`Approval Gmail`** (this node used to be `Approval Telegram`) emails
  that snapshot to the address set in its `sendTo` field — an HTML message
  with the post text and three buttons: **Approve**, **Regenerate**, **Edit**.
- **`Approval Wait`** pauses the execution right after the email is sent,
  and hands the Gmail node a unique resume URL
  (`{{ $execution.resumeUrl }}`) — each button links to that URL with a
  different `?action=` value. Clicking a button in the email resumes the
  paused workflow with that query param.
- **`Approval Action`** (a Switch) reads `$json.query.action` and routes:
  - `approve` → straight to publishing
  - `regenerate` → back to `Content Strategist` for a fresh attempt
  - `edit` → to the **`Editor`** agent, which rewrites the draft using the
    feedback text carried in the button's URL, then **`Final Optimizer`**
    gives it one more pass before publishing

> **Known limitation:** email buttons can't carry free-typed text. The
> "Edit" button ships with a fixed instruction
> (*"Improve the hook and tighten the language"*) baked into its URL. If you
> want to type custom edit notes from your inbox itself, that needs an IMAP
> or Gmail trigger node polling for replies — happy to extend it, just ask.

## 4. Publish

- **`Download Image`** and **`Validate Approved Payload`** prepare the
  final asset and check nothing's missing.
- **`Publish to LinkedIn`** posts it to your connected LinkedIn profile.
- **`Log Result`** records the outcome, ending at **`Published`** or
  **`Publish Failed`**.

## Why some "form" steps are now `Set` nodes

The original design used n8n's Form trigger, where "failure" screens are
literally web pages shown to whoever filled the form. Since intake is now a
webhook (see `README.md`), those steps (`Validation Failed`,
`Generation Failed`, `Published`, `Publish Failed`) just record a
`status` + `message` on the item instead of rendering a page — useful if you
pipe workflow executions into a log/dashboard later.