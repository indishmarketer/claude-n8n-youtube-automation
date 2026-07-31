# Claude Code Routine Setup

This is the piece that makes the whole thing work without you. Read it carefully.

## What a routine is

A Claude Code routine runs on its own, outside a chat window. It has its own working directory, its own connected tools, and it can be triggered by an incoming API call rather than by you typing.

That last part is the key. Workflows 1, 2 and 3 call the routine. The routine wakes up, does the work, calls back into n8n, and stops.

## 1. Create the routine

In Claude Code, create a routine and **enable the API trigger**. This gives you a URL and a token.

That URL and token go into n8n as an HTTP Header Auth credential. They are secrets. They never go into a file you push.

## 2. Point it at your repo

Connect the routine to **your fork** of this repo.

The routine reads `skill/SKILL.md` and everything in `skill/reference/` on each run. This is why you fork instead of cloning. When you edit your script style or your brand kit, you commit the change and the routine picks it up on the next run. No redeploy.

## 3. Connect the MCP servers

The routine needs three connections:

| Connector | Used for |
|---|---|
| n8n MCP | Calling Workflows 2 and 3 |
| vidIQ MCP | Keyword research |
| HeyGen MCP | Video Agent production |

Connect HeyGen through MCP, not through an API key. See `04-heygen-setup.md` for why.

## 4. Understand what triggers what

```
Workflow 1  ──trigger──▶  Routine   (new topic or script arrived)
Routine     ──call────▶  Workflow 2 (here is a script, get approval)
Workflow 2  ──trigger──▶  Routine   (approved, or here are the changes)
Routine     ──MCP─────▶  HeyGen     (produce this video)
Routine     ──call────▶  Workflow 3 (log this video ID to the sheet)
Routine stops.

HeyGen      ──webhook─▶  Workflow 4 (render done)
Workflow 4 uploads to YouTube on its own. The routine is not involved.
```

The routine gets triggered multiple times per video. Once when the topic arrives, and once per approval round. Each trigger is a fresh run. The payload from n8n has to carry the state, because the routine does not remember the previous run.

## 5. Payload shape

Workflow 1 sends:

```json
{
  "mode": "topic",
  "topic": "...",
  "angle": "...",
  "cta": "...",
  "vidiq": "use"
}
```

or

```json
{
  "mode": "script",
  "script": "..."
}
```

Workflow 2 sends back:

```json
{
  "mode": "approved",
  "script": "...",
  "title": "...",
  "description": "..."
}
```

or

```json
{
  "mode": "revise",
  "script": "...",
  "notes": "...",
  "title": "...",
  "description": "..."
}
```

## 6. The rule that saves you money

**The routine must never wait for a HeyGen render.**

After the MCP call returns a video ID, the routine logs it to the sheet and stops. It does not poll. It does not check status in a loop.

A routine sitting in a polling loop for fifteen minutes burns tokens for nothing, and Workflow 4 already handles completion through the webhook. This rule is written into `SKILL.md`. Do not remove it.

## 7. Test the routine alone first

Trigger the routine directly with a test payload before connecting Telegram. Confirm it reads the skill file, writes a script that sounds like you, and calls Workflow 2. Then wire up the front end.

## Verify it is reading the skill

Trigger it with a topic and check the output for your style markers. If the script has em dashes, generic openers, or does not sound like your example scripts, the routine is not reading `skill/reference/` properly. Check the repo connection and the file paths.
