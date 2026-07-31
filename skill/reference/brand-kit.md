# Brand Kit

Fill this in with your own values before first run. Nothing here should be a secret. Avatar IDs and voice IDs are not credentials.

## HeyGen

```
avatarId: <your HeyGen avatar or photo avatar ID>
voiceId:  <your HeyGen TTS voice ID>
```

Find these in HeyGen under Avatars and Voices, or ask Claude to list them through the HeyGen MCP connector.

Important: `voiceId` must be a **HeyGen TTS voice**. The Video Agent endpoint does not accept an external audio file. Voices from ElevenLabs, Fish Audio or any other provider cannot be used here, no matter how the docs read.

## Colors

```
Primary:    #RRGGBB
Secondary:  #RRGGBB
Tertiary:   #FFFFFF
Accent:     #RRGGBB
```

### Where colors are allowed

1. Subtitles and captions
2. Fonts and text overlays
3. Motion graphics

### Where colors are forbidden

**The avatar's real studio background.** Never change it, recolor it, or replace it with brand colors. This instruction goes into every Video Agent prompt.

## Channel details

```
Channel name:  <name>
Niche:         <niche>
Audience:      <who watches>
Default CTA:   <your usual CTA>
Upload privacy: unlisted
```
