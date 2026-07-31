# The Four Workflows

> **Before you push:** the four JSON files go in this folder. Export each workflow from n8n, then run through `../SANITIZE.md` before committing. An n8n export contains your bot token references, chat IDs, spreadsheet IDs and webhook URLs.

Import order matters. Do 1, 2, 3, then 4, because later workflows reference webhook URLs produced by earlier ones.

---

## Workflow 1: Telegram idea intake

**File:** `01-telegram-idea-intake.json`
**Trigger:** Telegram bot message
**Talks to:** Claude Code routine

### What it does

Listens to your Telegram bot. Runs a two-keyword conversation.

| You send | Bot asks for | Result |
|---|---|---|
| `topic` or `idea` | Your video topic | Full pipeline with script approval |
| `script` | A ready-made script | Treated as approved, straight to production |

A chat ID filter sits on the front. Only your chat ID gets through. This replaces any password or passphrase, and it is why the launcher web page is now optional.

The bot confirms receipt so you know it landed.

### Topic format

```
<topic> [angle: ...] [cta: ...] (use vidiq | no vidiq)
```

Only the topic is required.

### Nodes to reconnect after import

- Telegram Trigger credential
- Telegram Send Message credential
- Your chat ID in the filter node
- The Claude Code routine API trigger URL

---

## Workflow 2: Script review and approval

**File:** `02-script-review-approval.json`
**Trigger:** Called by Claude when a script is ready
**Talks to:** Telegram, n8n Form, Claude Code routine

### What it does

Claude sends the script, title and description here. The workflow then splits the notification in two:

1. **Telegram gets a short message.** Topic, title, and a link to the form.
2. **The n8n form page shows the full script**, the title and the description, with an Approve button and a Request Changes textarea.

This split exists because Telegram caps a message at 4096 characters and a full script is longer than that. Chunking across multiple Telegram messages was tried and it is worse to read on a phone.

### The loop

Approve, and the payload goes to production.
Request changes, and your notes go back to Claude. Claude rewrites and calls this workflow again with a fresh script. Repeat until you approve.

### Nodes to reconnect after import

- Telegram credential
- Form Trigger, note the new public form URL
- The Claude Code routine API trigger URL, used in both the approve and the revise branches

---

## Workflow 3: Log to tracking sheet

**File:** `03-log-to-spreadsheet.json`
**Trigger:** Webhook, called by Claude after the HeyGen video is submitted
**Talks to:** Google Sheets

### What it does

Writes one row per video the moment production starts.

Columns are defined in `../templates/tracking-sheet.md`. The important one is `heygen_video_id`, which is the key Workflow 4 uses to match the finished render back to its title and description.

This workflow is deliberately dumb. One webhook, one append row. It exists so that Claude can hand off and stop rather than sitting there waiting for a render.

### Nodes to reconnect after import

- Google Sheets OAuth credential
- Spreadsheet ID
- Sheet name

---

## Workflow 4: HeyGen webhook to YouTube upload

**File:** `04-heygen-webhook-youtube-upload.json`
**Trigger:** HeyGen webhook, fires when a render completes
**Talks to:** Google Sheets, YouTube Data API, Telegram

### What it does

This is the only workflow that does not involve Claude at all. Once it is set up, it runs on its own forever.

1. HeyGen fires the webhook with a video ID and a download URL
2. Look up the matching row in the tracking sheet by `heygen_video_id`
3. Download the video file
4. Upload to YouTube with the title and description from the sheet, set to **unlisted**
5. Write the YouTube URL back to the sheet
6. Send a Telegram message with the link

### Why unlisted

So you watch it before the world does. HeyGen is good, not perfect. Flip the privacy field to `public` in the YouTube node if you want to skip the check.

### Nodes to reconnect after import

- Webhook node, copy the production URL and register it in HeyGen
- Google Sheets OAuth credential and spreadsheet ID
- YouTube OAuth credential
- Telegram credential and chat ID

### Registering the webhook in HeyGen

HeyGen needs to know where to send the callback. Add the production webhook URL from this workflow in your HeyGen webhook settings, subscribed to the video completion event. See `../docs/04-heygen-setup.md`.
