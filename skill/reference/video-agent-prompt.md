# HeyGen Video Agent Prompt Template

One shot. Send this as a single prompt. Do not follow up with refinement messages.

Fill in every placeholder. Do not leave instructions vague, the agent fills gaps with stock cliches if you do.

---

```
Create a YouTube video from the script below.

SCRIPT (use this word for word, do not paraphrase, do not trim, do not add lines):
"""
<PASTE THE EXACT APPROVED SCRIPT HERE>
"""

AVATAR: <avatarId>
VOICE: <voiceId>

VOICEOVER:
Use the specified HeyGen voice for the entire script. The avatar delivers the
full script.

CAPTIONS:
Burn in captions for the whole video.
Caption color: <Secondary hex>
Caption font: bold, clean sans serif, large enough to read on mobile.

VISUAL STYLE:
High quality modern graphic design and motion graphics.
Vox-explainer style visuals that illustrate the specific point being spoken.
When the script mentions a phone screen, app, dashboard or interface, generate a
clean UI mockup of it.
AI-generated footage and images are allowed when they look clean, realistic and
high quality, the way large YouTube channels use them.
Use visuals throughout the video, not only at a few moments.

STRICTLY AVOID:
No cyberpunk palettes. No neon. No glowing grids or circuit board imagery.
No sci-fi or "tech world" visuals. No robot hands, no floating holograms.
No heavy gradients. No generic gradient stock imagery.
Nothing cringe or clip-art like.

BACKGROUND:
Do not change, recolor or replace the avatar's real studio background.
Keep it exactly as it is. Brand colors apply only to captions, text overlays
and motion graphics.

TEXT OVERLAYS:
Use <Primary hex> and <Accent hex> for emphasis text and motion graphics.
Keep text on screen minimal and readable.

OUTPUT:
16:9 landscape.
```

---

## Why one shot

Every extra message to the agent is another render pass and another credit charge. It also drifts away from the script. Put everything in the first prompt and accept what comes back.

## Why no attached assets

The `files` parameter exists, but the agent does not reliably place uploaded images or clips where you want them. Letting the agent generate its own visuals produces more consistent output. This was tested and abandoned.
