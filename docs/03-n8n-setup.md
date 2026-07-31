# n8n Setup

## Cloud or self-hosted

Either works. Self-hosted is what I recommend if you are deploying this for a team, because you control the data and there is no execution cap.

Self-hosted needs a public URL for two things: the n8n Form pages in Workflow 2, and the HeyGen webhook in Workflow 4. Use n8n's tunnel for testing and a real domain for production.

## Import the workflows

n8n, Workflows, Import from File. Do all four, in order 1 to 4.

Every imported workflow lands with broken credential references. That is expected. An export strips the actual secrets and keeps only the credential name.

## Credentials to create

| Credential | Used in |
|---|---|
| Telegram | Workflows 1, 2, 4 |
| Google Sheets OAuth2 | Workflows 3, 4 |
| YouTube OAuth2 | Workflow 4 |
| HTTP Header Auth for Claude routine | Workflows 1, 2 |

## After import, per workflow

Open each workflow and walk every node with a red triangle.

**Workflow 1:** Telegram trigger credential, Telegram send credential, your chat ID in the filter, the Claude routine trigger URL.

**Workflow 2:** Telegram credential, Form Trigger. The form gets a new public URL on your instance, copy it. Then the Claude routine URL in both the approve branch and the revise branch.

**Workflow 3:** Google Sheets credential, spreadsheet ID, sheet name.

**Workflow 4:** Webhook node (copy the production URL, you register it in HeyGen), Google Sheets credential and spreadsheet ID, YouTube credential, Telegram credential and chat ID.

## Activate

All four need to be active. Workflow 1 and Workflow 4 are the two that listen constantly. If Workflow 1 is inactive your bot goes silent. If Workflow 4 is inactive your videos render and then nothing happens.

## Webhook URL gotcha

n8n gives every webhook a test URL and a production URL. They are different. Test URLs only fire when you have the editor open with Listen active.

Register the **production** URL everywhere. This is the single most common reason a setup half works.
