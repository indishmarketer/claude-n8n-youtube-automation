# Tracking Sheet Schema

Create a Google Sheet with these columns in row 1, in this exact order.

| Column | Header |
|---|---|
| A | `video_id` |
| B | `title` |
| C | `description` |
| D | `status` |
| E | `video_url` |
| F | `youtube_url` |
| G | `created_at` |

## Why the sheet exists

Claude does not wait for the video to finish rendering. It submits the production request, writes a row to the sheet, and exits.

Several minutes later, HeyGen finishes rendering and sends a webhook containing the `video_id`. Workflow 4 uses that `video_id` to find the correct row, update the render status, save the download URL, upload the video to YouTube, and finally write the YouTube URL back to the sheet.

Column A (`video_id`) is the join key. Do not rename it or reorder the columns unless you also update both workflows.

## Setup

1. Create a new Google Sheet.
2. Add the headers above to row 1 in the exact order shown.
3. Freeze row 1.
4. Copy the spreadsheet ID from the URL (the long string between `/d/` and `/edit`).
5. Paste that spreadsheet ID into both Workflow 3 and Workflow 4.

## Optional

Add conditional formatting to the `status` column so rows with `failed` are highlighted in red. This makes failed productions easy to spot.
