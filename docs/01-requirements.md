# Requirements

## Accounts you need

| Service | Why | Plan | Cost |
|---|---|---|---|
| Claude | Runs the routine that writes scripts and drives the pipeline | Pro or Max, routines with API trigger enabled | $20 to $100 / mo |
| HeyGen | Renders the video | Creator or higher | $29+ / mo |
| n8n | Runs the four workflows | Cloud Starter, or self-hosted | $0 to $24 / mo |
| vidIQ | Keyword validation, optional | Any tier with MCP access | $10+ / mo |
| Telegram | Intake and notifications | Free Bot API | Free |
| Google Cloud | Sheets API and YouTube Data API | Free tier | Free |
| GitHub | Holds your skill file and reference scripts | Free | Free |

## The HeyGen plan trap

Read this before you buy anything.

HeyGen web plans (Creator, Team) and the HeyGen API bill from **two separate credit wallets**. A web plan cannot buy one-time API credit packs. So if you connect a raw API key, your calls hit an empty API wallet and fail, or you end up paying for credits twice.

**Solution:** use the HeyGen MCP connector in Claude, not an API key. MCP calls bill your web plan credits.

## Credit budgeting

HeyGen Video Agent costs more than a plain avatar render, because it generates captions, motion graphics and B-roll. Budget accordingly. On a 600 credit plan you are not making a video a day.

vidIQ credits also run out. Use the `(no vidiq)` flag on personal stories and opinion videos where there is no keyword angle to research.

## Skills you need

None, really. You will:

- Import JSON into n8n and click through OAuth screens
- Create a Telegram bot by messaging BotFather
- Edit markdown files
- Copy and paste IDs between tabs

If you can set up a Zapier zap, you can do this.

## Time to first video

Realistically 2 to 4 hours for the first setup, most of it Google Cloud OAuth. After that, about 5 minutes of your attention per video.
