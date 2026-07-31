# Tracking Sheet Schema

Create a Google Sheet with these columns in row 1, in this order. Workflow 3 appends rows. Workflow 4 reads and updates them.

| Column | Header | Written by | Notes |
|---|---|---|---|
| A | `timestamp` | Workflow 3 | When production started |
| B | `topic` | Workflow 3 | Original topic from Telegram |
| C | `title` | Workflow 3 | Final YouTube title |
| D | `description` | Workflow 3 | Final YouTube description |
| E | `heygen_video_id` | Workflow 3 | **The match key.** Workflow 4 looks up this column. |
| F | `status` | Both | `producing`, `rendered`, `uploaded`, `failed` |
| G | `video_url` | Workflow 4 | HeyGen download URL from the webhook |
| H | `youtube_url` | Workflow 4 | Final YouTube link |
| I | `mode` | Workflow 3 | `topic` or `script` |
| J | `notes` | You | Anything you want to track |

## Why the sheet exists

Claude does not wait for a render. It submits the video, writes a row, and stops.

Fifteen minutes later HeyGen finishes and fires a webhook with a video ID. That webhook has no idea what the title or description was. The sheet is what connects them.

Column E is the join. Do not rename it, do not reorder columns without updating both workflows.

## Setup

1. New Google Sheet, name it whatever
2. Paste the headers into row 1
3. Freeze row 1
4. Copy the spreadsheet ID from the URL, the long string between `/d/` and `/edit`
5. Put that ID in Workflow 3 and Workflow 4

## Optional

Add a conditional format on column F so failed rows turn red. Makes it obvious at a glance when something broke.
