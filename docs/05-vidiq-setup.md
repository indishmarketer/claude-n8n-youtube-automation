# vidIQ Setup

## What vidIQ does here

It validates and sharpens keywords for a topic you have **already chosen**.

It does not pick your topics. That was the original design and it was wrong. Open-ended topic discovery burns credits, returns generic suggestions, and produces videos you do not actually want to make. Topics come from you.

## Connect the MCP

Connect the vidIQ MCP connector in Claude. That is the whole setup.

## The per-topic flag

Append a flag to your topic text when you send it to the Telegram bot:

| Flag | Behaviour |
|---|---|
| `(use vidiq)` | Runs research. This is the default when no flag is present. |
| `(no vidiq)` or `(skip vidiq)` | Skips research entirely. |

Example:

```
Why my first automation video flopped (no vidiq)
```

```
n8n vs Zapier for small business (use vidiq)
```

## When to skip

Skip vidIQ on:

- Personal stories
- Opinion videos
- Build logs and behind-the-scenes
- Anything where there is no keyword to rank for

Use vidIQ on:

- Tutorials
- Comparisons
- "How to" and tool reviews
- Anything a person would actually type into search

This flag exists purely to stop burning credits on videos where research adds nothing.

## What Claude pulls

Search volume for the topic phrase and a few close variants, competition score, and related terms. The strongest variant goes in the title, the rest go in the description. Then it stops. A handful of calls, not dozens.
