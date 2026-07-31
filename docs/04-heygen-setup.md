# HeyGen Setup

## Use MCP, not the API key

This is the most important thing in this document.

HeyGen web plans and the HeyGen API bill from two separate credit wallets. A Creator plan gives you monthly web plan credits. An API key draws from an API wallet, which starts empty, and web plans cannot buy one-time API credit packs to fill it.

So: connect the **HeyGen MCP connector** inside Claude. MCP calls bill your web plan credits. That is the whole trick.

The only place an API key is still needed is nowhere in this pipeline. Workflow 4 downloads the finished video from the URL HeyGen provides in its webhook payload.

## Set up your avatar

Create your avatar in HeyGen. Video avatar or photo avatar, either works.

Record or upload against a clean real background. This matters, because the pipeline explicitly forbids the Video Agent from replacing that background with brand colors. A good real studio background is what makes the output look like a real video.

Copy the avatar ID into `skill/reference/brand-kit.md`.

## Pick a voice

Browse HeyGen voices and pick one. Copy the voice ID into `brand-kit.md`.

### Why you cannot use ElevenLabs or Fish Audio

You will be tempted. I was. I cloned my voice in ElevenLabs, then later bought a Fish Audio subscription and connected the Fish MCP, and built an entire n8n workflow to upload the generated audio to HeyGen as an asset.

It does not work with the Video Agent.

The Video Agent endpoint accepts exactly four things: `prompt`, `avatarId`, `voiceId` and `files`. `voiceId` must be a HeyGen TTS voice. There is no audio asset input. You cannot hand it an external voiceover file.

The external voice path only works with the plain avatar render endpoint, and plain avatar renders are why the output looked cheap in the first place.

Use a HeyGen voice. Pick a good one.

## Video Agent, not plain avatar

Plain talking-head renders look low quality. Video Agent generates captions, motion graphics and B-roll around the avatar, so it looks like something a person made deliberately.

It costs more credits per video. Accept that trade.

## Prompting rules

One shot. Send the entire script and every visual instruction in a single prompt. Do not open a conversation with the agent and refine over several turns. Each turn costs credits and drifts from the script.

The template is in `skill/reference/video-agent-prompt.md`.

## Do not attach assets

The `files` parameter exists. I built an entire Telegram asset upload bot around it: send an image with a caption, n8n downloads it from Telegram and uploads it to HeyGen assets, then Claude maps each asset to a specific script line in the prompt.

The agent did not place them reliably. The whole asset path was removed. Let the agent generate its own visuals.

## Register the webhook

HeyGen, Settings, Webhooks. Add the **production** webhook URL from Workflow 4. Subscribe to the video completion event.

Without this, videos render and then sit there. Nothing reaches YouTube.
