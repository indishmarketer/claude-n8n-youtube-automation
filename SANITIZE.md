# Sanitize Before You Push

An n8n workflow export is not automatically safe to publish. It strips the credential **values** but keeps plenty of things you do not want in a public repo.

Run this checklist before your first commit, and again any time you re-export a workflow.

## What leaks in an n8n export

| Item | Where it shows up | Risk |
|---|---|---|
| Telegram chat ID | Workflow 1 filter node, Workflow 4 notify node | Anyone can spam your bot past the filter |
| Telegram bot token | Sometimes pasted into HTTP nodes by mistake | Full control of your bot |
| Google Sheets spreadsheet ID | Workflows 3 and 4 | Readable if the sheet is not locked down |
| Webhook path and instance URL | Workflow 3 and 4 webhook nodes | Anyone can trigger your workflows |
| n8n Form URL | Workflow 2 | Anyone can approve your scripts |
| Claude routine trigger URL and token | Workflows 1 and 2 HTTP nodes | Anyone can burn your Claude usage |
| Credential IDs and names | Every workflow | Low risk, but noisy |
| Your email address | Any leftover SMTP nodes | Spam |
| HeyGen API key | HTTP nodes, if you used one | Billing |
| YouTube channel ID | Workflow 4 | Low risk |

## The placeholder convention

Replace real values with obvious placeholders. Use this format so people know what to fill in:

```
__YOUR_TELEGRAM_CHAT_ID__
__YOUR_SPREADSHEET_ID__
__YOUR_N8N_INSTANCE_URL__
__YOUR_CLAUDE_ROUTINE_URL__
__YOUR_HEYGEN_AVATAR_ID__
__YOUR_HEYGEN_VOICE_ID__
```

Not `xxxxx`, not `12345`. A reader should be able to search the repo for `__YOUR_` and find everything they need to change.

## Manual checklist

- [ ] Open each JSON in a text editor
- [ ] Search for your chat ID number
- [ ] Search for your email address
- [ ] Search for your n8n domain
- [ ] Search for `token`, `key`, `secret`, `Bearer`, `apiKey`
- [ ] Search for `https://` and check every URL that is not a public docs link
- [ ] Check the `webhookId` and `path` fields on every webhook and form node
- [ ] Check `skill/reference/brand-kit.md` for anything that is not meant to be public
- [ ] Confirm `.env` is not staged

## Let Claude scan it for you

Paste this into Claude with the files attached:

```
Scan these n8n workflow JSON exports and this skill folder for anything that
should not be in a public GitHub repository.

Look for:
- API keys, bearer tokens, bot tokens, client secrets
- Telegram chat IDs
- Google spreadsheet IDs and document IDs
- Private instance URLs, webhook paths, form URLs
- Email addresses and personal identifiers
- Anything that looks like a credential even if it is not labelled as one

For each finding, give me the file, the JSON path or line, what it is, and the
placeholder I should replace it with using the __YOUR_THING__ convention.

Do not print the secret values back to me in full. Truncate them.
```

## Rotate anything that already leaked

If you pushed before sanitizing, deleting the file is not enough. Git history keeps it.

1. Regenerate the Telegram bot token through BotFather
2. Rotate the Claude routine trigger token
3. Rotate any HeyGen or Google credentials that appeared
4. Change the n8n webhook paths
5. Then clean history, or just delete the repo and start a fresh one

Rotating is faster and safer than rewriting history. Do that first.

## Keep it clean going forward

The `.gitignore` in this repo already excludes `.env`, `*.local.json` and `credentials/`. Keep your real exports outside the repo folder, sanitize a copy, and commit the copy.
