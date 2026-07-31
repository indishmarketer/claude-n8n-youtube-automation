# Troubleshooting

## The bot does not reply

- Workflow 1 is not active
- Chat ID in the filter node does not match yours. Re-check with `getUpdates`.
- Telegram credential is not connected after import

## The bot replies but nothing else happens

The Claude routine trigger URL in Workflow 1 is wrong, or the API trigger is not enabled on the routine. Check the n8n execution log for the HTTP node response.

## The script does not sound like me

The routine is not reading `skill/reference/example-scripts.md`, or that file still has the placeholder in it. Check the repo connection on the routine, and confirm the file actually has your scripts in it.

## Em dashes keep appearing

Same cause. The skill file is not being read. If it is being read and they still appear, add the rule again more explicitly near the top of `SKILL.md`.

## The approval form shows an empty script

Workflow 2 is receiving the payload but the form field is not mapped to the right key. Open the last execution and look at the actual JSON coming in.

## Telegram message fails with a length error

Something is trying to send the full script through Telegram. Only the short notification goes through the bot. The script belongs on the form page. Check the Telegram node in Workflow 2.

## HeyGen call fails with insufficient credits

You are using an API key instead of the MCP connector. Web plan credits and API credits are separate wallets. Switch to MCP. See `04-heygen-setup.md`.

## The video renders but never reaches YouTube

Almost always one of three things:

1. The HeyGen webhook is registered with the **test** URL instead of the production URL
2. Workflow 4 is not active
3. The `heygen_video_id` in the sheet does not match what the webhook sends, so the row lookup returns nothing

Check Workflow 4's execution list. If there are no executions at all, it is the webhook. If there are executions that fail at the lookup step, it is the ID matching.

## YouTube upload fails with quota exceeded

Each upload costs 1600 of your 10,000 daily quota units. That is 6 per day. Wait for the reset at midnight Pacific, or request a quota increase.

## YouTube upload fails with access blocked

Your Google account is not in the OAuth consent screen test users list. Add it.

## The routine runs forever and burns tokens

It is polling HeyGen for render status. It should not be. The handoff rule in `SKILL.md` says the routine takes the video ID, logs it, and stops. Restore that rule.

## Video Agent output looks generic

The prompt is too vague. Fill in every placeholder in `video-agent-prompt.md`, and keep the "strictly avoid" block. Vague visual direction is what produces gradient stock imagery.

## The agent changed my background

The background instruction is missing from the prompt. It has to be stated explicitly in every prompt, not just once in the skill file.

## vidIQ credits running out fast

You are researching topics that do not need it. Use `(no vidiq)` on personal stories, opinion videos and build logs.

## Still stuck

Open an issue with the workflow number, the node that failed, and the error text from the n8n execution log. Redact your tokens first.
