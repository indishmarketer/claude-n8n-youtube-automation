# Customizing This For Your Niche

The four workflows stay the same. The plumbing does not care what your channel is about.

What changes is the skill file and the reference files. That is the whole point of keeping them in a repo instead of hardcoding them into n8n.

## Must change

### `skill/reference/example-scripts.md`

Replace the placeholder with two or three of your own published scripts.

This is the single biggest lever on output quality. Claude matches what it sees. Generic examples produce generic scripts. Your actual scripts produce your actual voice.

Pick scripts you were happy with. Keep the filler and the asides. Do not clean them up to sound impressive, because then Claude will write scripts that sound impressive instead of scripts that sound like you.

### `skill/reference/brand-kit.md`

Your avatar ID, your voice ID, your colors, your channel details.

## Should change

### `skill/reference/script-style.md`

The structure template in here is the one that works on my channel. Proof hook, context, numbered steps, "here's the part nobody shows you", current state, CTA.

If your niche needs a different structure, change it. A cooking channel does not need a proof hook. A finance channel might need a disclaimer section.

Keep the voice rules. Active voice, short sentences, no em dashes, no AI filler. Those are not niche-specific, they are just what good spoken writing sounds like.

### `skill/reference/video-agent-prompt.md`

The visual direction section is opinionated. Vox-explainer style, generated UI mockups, no cyberpunk, no neon, no heavy gradients.

If your niche wants a different look, rewrite that block. Just keep it **specific**. Vague visual instructions are how you end up with gradient stock imagery and floating holograms.

Keep the background rule. Never let the agent replace the avatar's real studio background.

## Can change

### `skill/SKILL.md`

You can adjust:

- Script length targets
- How much vidIQ research runs
- Approval rules
- Which visual rules go into the prompt

Keep the structure. The two entry modes, the approval loop, and above all the rule that the routine never waits for a render. That last one is not a style choice, it is what keeps the system cheap and reliable.

## Rarely need to change

### The workflows

Two things people do change:

**Upload privacy.** Workflow 4 uploads as unlisted so you can check before publishing. Change the privacy field to `public` if you want it fully hands-off.

**The avatar tool.** Only Workflow 4 is HeyGen-specific, and only in the download step. Swap that for any service that returns a video URL and the rest of the chain works.

## For a team or agency setup

If you are deploying this inside a company rather than for a personal channel:

- Use a professional stock avatar plus a brand kit instead of your own face
- Self-host n8n so the data stays internal
- Have whoever handles infrastructure own the Google Cloud and YouTube OAuth
- vidIQ becomes optional, since the content calendar usually already exists
- One Claude subscription covers the whole marketing team
