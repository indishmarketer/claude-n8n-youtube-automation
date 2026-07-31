# Google Sheets and YouTube Setup

This is the longest part of the setup. Budget an hour. It is all one-time.

## 1. Create a Google Cloud project

console.cloud.google.com, new project, name it something you will recognize.

## 2. Enable two APIs

APIs and Services, Library. Enable:

- Google Sheets API
- YouTube Data API v3

## 3. Configure the OAuth consent screen

External. Fill in the app name, your email, and a support email.

Under Scopes, add:

```
https://www.googleapis.com/auth/spreadsheets
https://www.googleapis.com/auth/youtube.upload
https://www.googleapis.com/auth/youtube
```

Under Test users, add your own Google account. Without this, OAuth fails with an access blocked error.

You do not need to publish the app or go through verification. Test mode works for personal use.

## 4. Create OAuth credentials

Credentials, Create Credentials, OAuth client ID, Web application.

Under Authorized redirect URIs, paste the callback URL that n8n shows you when you create a Google credential. n8n Cloud and self-hosted have different callback URLs, so copy it from your own instance.

Copy the client ID and client secret into n8n.

## 5. Quota warning

YouTube Data API gives you 10,000 quota units per day by default. A video upload costs **1600 units**.

That is 6 uploads per day. Plenty for a normal channel, but worth knowing before you wonder why the seventh one failed.

Sheets API quota is generous and you will not hit it.

## 6. Create the tracking sheet

Make a new Google Sheet. Use the column schema in `../templates/tracking-sheet.md`.

Copy the spreadsheet ID from the URL. It is the long string between `/d/` and `/edit`.

Put it in Workflow 3 and Workflow 4. Both need it.

## 7. Test the upload separately

Before running the full pipeline, run Workflow 4 manually with a test video URL. YouTube OAuth is the most fragile piece of this setup and you want to find problems there in isolation, not at the end of a chain.
