# Everything AI Voice Prototype

**The first code project of Everything AI — a historical voice-assistant prototype.**

An early Everything AI prototype combining a Svelte interface, streamed speech recognition, language-model responses, and audio playback.

## Our Beginnings

This was our first code project, and it cost us **$13,000 USD**. We preserve it as a record of Everything AI’s beginnings and our first investment in building the company.

## The Original Delivery

The repository now includes a sanitized copy of Everything AI’s original delivery supplied by the owner, previously preserved as `EverythingAI-wakeUp.zip`. The code calls the assistant **Sophia**; this historical name is kept rather than rewriting the original identity.

The prototype combines:

- A Svelte/SvelteKit interface with Everything AI visual assets.
- Browser microphone capture and streamed transcription.
- Groq/LangChain conversation and wake/sleep detection.
- Audio playback using a separately hosted text-to-speech endpoint.
- A standalone Deepgram streaming experiment.

## Original Prototype Artwork

<img src="prototype/eai-app/static/images/sophia-full.png" alt="Original Sophia character artwork from the Everything AI voice prototype" width="280" />

Original interface artwork, preserved from the historical delivery. This is not a screenshot of a currently operating service.

## Explore the Source

| File or Folder | Purpose |
|---|---|
| [Voice component](prototype/eai-app/src/components/Commands.svelte) | Conversation, wake/sleep logic, microphone and transcription integration |
| [Application page](prototype/eai-app/src/routes/+page.svelte) | Original interface and branding |
| [Streaming experiment](prototype/deepgram/streamdemo.html) | Standalone transcription experiment |
| [Dependency manifest](prototype/eai-app/package.json) | Historical dependencies and scripts |
| [Archive notes](ARCHIVE_NOTES.md) | Provenance, sanitization, and limitations |

## Archive Status

This is a **historical source archive**, not an active voice service. Embedded credentials were removed, the original text-to-speech endpoint was replaced, and automatic microphone/network startup is disabled. Deployment-specific AWS settings and the original ZIP remain private.

Historical dependency declarations and model names are retained. The source check currently reports 10 type diagnostics and 2 warnings; see [validation notes](ARCHIVE_NOTES.md#publication-validation). A modern working service would require a separate restoration effort, server-side secret handling, current provider integrations, and explicit microphone controls.

## Local Source Inspection

From `prototype/eai-app/`, the historical project declares `npm ci`, `npm run check`, and `npm run build`. Dependency compatibility and archived provider APIs must be reviewed before attempting restoration. Do not insert long-lived provider keys into browser code.

## About Everything AI

[Everything AI Co](https://www.everythingaico.com/) builds AI workflows, research software, and data engineering tools. This repository documents where that work began.

See [NOTICE.md](NOTICE.md) for ownership and third-party material.
