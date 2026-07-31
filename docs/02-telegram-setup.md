# Telegram Setup

## 1. Create the bot

Open Telegram, search for `@BotFather`, send `/newbot`.

Give it a name and a username ending in `bot`. BotFather returns a token that looks like `123456789:AAxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.

**That token is a credential. It never goes in a file you push to GitHub.**

## 2. Get your chat ID

Message your new bot anything. Then open:

```
https://api.telegram.org/bot<YOUR_TOKEN>/getUpdates
```

Look for `"chat":{"id":123456789`. That number is your chat ID.

## 3. Why the chat ID matters

Workflow 1 filters on your chat ID. Anyone who finds your bot username can message it, but only your chat ID gets through to Claude. This is the security model. It replaces any password or passphrase, and it is the reason the launcher web page is optional.

If you change devices or accounts, the chat ID changes. Update it in the workflow.

## 4. Add the credential in n8n

n8n, Credentials, New, Telegram. Paste the token. Name it something you will recognize.

Every Telegram node across all four workflows uses this same credential.

## 5. Test the two keywords

Send your bot `topic`. It should ask for your topic.
Send your bot `script`. It should ask for a ready-made script.

If nothing happens, check that Workflow 1 is active and that the chat ID filter matches yours.

## Message limits

Telegram caps a single message at 4096 characters. This is why full scripts are shown on the n8n form page and not sent through the bot. Do not try to work around it by chunking, it reads badly on a phone.
