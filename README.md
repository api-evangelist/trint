# Trint (trint)

Trint is an AI-powered audio and video transcription platform built for media, journalism, and enterprise content teams. It transcribes speech to text in dozens of languages, provides a collaborative editor, translation, captions, and story-building tools, and exposes a documented REST API at [dev.trint.com](https://dev.trint.com) for uploading and transcribing media, listing and exporting transcripts, translating, running realtime transcription, and receiving webhook events.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/trint/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/trint/refs/heads/main/apis.yml)

## Access Model (Important)

The Trint API is a **paid, plan-gated** developer surface, not a self-serve free API:

- **No permanent free tier.** Trint sells per-seat subscriptions (Starter, Advanced, Enterprise), typically billed annually, with a trial rather than a free plan.
- **API access is plan-dependent.** Trint's developer docs state the API is "accessible on many of our plans," but recommend an **Enterprise** account for heavy usage. **Full API access, SSO (SAML), SCIM provisioning, and EU data residency are Enterprise features.** Confirm whether a given non-Enterprise plan includes API keys directly with Trint.
- **US vs EU tenant.** The default base URL is `https://api.trint.com`; EU (data-residency) Enterprise accounts use `https://api.eu.trint.com`. Direct media uploads go to the separate host `https://upload.trint.com`.
- **Authentication** is HTTP Basic using the API Key ID as the username and the API Key Secret as the password (Base64-encoded `Authorization` header). A legacy `api-key` header is also supported.

## Grounding and Modeling

This catalog entry is grounded in Trint's public developer docs at [dev.trint.com](https://dev.trint.com), including its machine-readable index at [dev.trint.com/llms.txt](https://dev.trint.com/llms.txt).

- **Confirmed** against the docs: the upload host and `POST https://upload.trint.com/` upload-and-transcribe flow with its query parameters; the list endpoint `GET https://api.trint.com/transcripts/` (with `limit`/`skip` pagination, per Trint's own example); Basic-auth and legacy `api-key` authentication; the export formats (JSON, text, CSV, Word `.docx`, SubRip `.srt`, EDL, Premiere XML); the RTMP-based realtime flow; and the webhook event types.
- **Honestly modeled** (marked `x-modeled: true` in the OpenAPI): exact request paths for exports, files/folders/shared-drives retrieval, translations, and realtime start/stop/status, where Trint's individual reference pages were not machine-readable at authoring time but the capabilities are listed in its documented index. Verify exact paths against the live reference during reconciliation.

## Tags

- Audio Transcription
- Transcription
- Speech-to-Text
- Media
- Journalism
- AI
- Captions

## Timestamps

- **Created:** 2026-07-11
- **Modified:** 2026-07-11

## APIs

### Trint Upload and Transcribe API

Upload a media file directly to Trint (POST to `https://upload.trint.com`) or ingest asynchronously from a source URL to start automatic speech-to-text transcription, with options for language, speaker diarization, custom dictionary, folder, and workspace. Supports MP4, MOV, WAV, MP3, and more.

- **Human URL:** [https://dev.trint.com/reference/upload](https://dev.trint.com/reference/upload)
- **Base URL:** `https://upload.trint.com`

#### Tags

- Audio Transcription
- Speech-to-Text
- Upload

#### Properties

- [Documentation](https://dev.trint.com/docs/uploading-files-to-trint)
- [API Reference](https://dev.trint.com/reference/upload)
- [OpenAPI](openapi/trint-openapi.yml)
- [Postman Collection](collections/trint.postman_collection.json)
- [Open Collection](collections/trint.opencollection.json)

### Trint Transcripts and Files API

List and retrieve transcripts (files), folders, and shared drives. The list endpoint `GET https://api.trint.com/transcripts/` (with `limit` and `skip` pagination) is confirmed in Trint's public docs; it returns files belonging to the current user or accessible via a shared drive.

- **Human URL:** [https://dev.trint.com/reference/page](https://dev.trint.com/reference/page)
- **Base URL:** `https://api.trint.com`

#### Tags

- Transcription
- Files
- Media

#### Properties

- [Documentation](https://dev.trint.com/docs/trint-api-keys)
- [API Reference](https://dev.trint.com/reference/page)
- [OpenAPI](openapi/trint-openapi.yml)
- [Postman Collection](collections/trint.postman_collection.json)
- [Open Collection](collections/trint.opencollection.json)

### Trint Export API

Export transcripts and captions in multiple formats - JSON, plain text, CSV, Microsoft Word (`.docx`), SubRip captions (`.srt`), Edit Decision List (`.edl`), and Premiere XML. Export formats are grounded in Trint's public export docs; exact request paths are honestly modeled.

- **Human URL:** [https://dev.trint.com/docs/exporting-files-from-trint](https://dev.trint.com/docs/exporting-files-from-trint)
- **Base URL:** `https://api.trint.com`

#### Tags

- Export
- Captions
- Media

#### Properties

- [Documentation](https://dev.trint.com/docs/exporting-files-from-trint)
- [OpenAPI](openapi/trint-openapi.yml)
- [Postman Collection](collections/trint.postman_collection.json)
- [Open Collection](collections/trint.opencollection.json)

### Trint Translations API

List supported translation languages, create translations of a transcript into a target language, and retrieve translations for a file. Trint markets translation into dozens of languages; endpoint paths here are modeled from the documented capability index.

- **Human URL:** [https://dev.trint.com](https://dev.trint.com)
- **Base URL:** `https://api.trint.com`

#### Tags

- Translation
- Localization
- Media

#### Properties

- [Documentation](https://dev.trint.com)
- [OpenAPI](openapi/trint-openapi.yml)
- [Postman Collection](collections/trint.postman_collection.json)
- [Open Collection](collections/trint.opencollection.json)

### Trint Realtime Transcription API

Run live transcription by generating an RTMP stream key/URL and starting, stopping, and polling the status of a realtime stream. Realtime media ingest uses **RTMP**, a media streaming protocol - **not a WebSocket**. Results and lifecycle are surfaced via status polling and webhooks.

- **Human URL:** [https://dev.trint.com](https://dev.trint.com)
- **Base URL:** `https://api.trint.com`

#### Tags

- Realtime
- Live Transcription
- RTMP

#### Properties

- [Documentation](https://dev.trint.com)
- [OpenAPI](openapi/trint-openapi.yml)
- [Postman Collection](collections/trint.postman_collection.json)
- [Open Collection](collections/trint.opencollection.json)

### Trint Webhooks API

Register and de-register webhook endpoints and list event types. Trint POSTs lifecycle events - `ACTIVITY_STARTED`, `MEDIA_TRANSFER_COMPLETE`, `MEDIA_TRANSFER_FAILED`, `TRANSCRIPT_COMPLETE`, `TRANSCRIPT_FAILED` - to your URL as ordinary HTTP callbacks, not over a WebSocket.

- **Human URL:** [https://dev.trint.com](https://dev.trint.com)
- **Base URL:** `https://api.trint.com`

#### Tags

- Webhooks
- Events
- Callbacks

#### Properties

- [Documentation](https://dev.trint.com)
- [OpenAPI](openapi/trint-openapi.yml)
- [Postman Collection](collections/trint.postman_collection.json)
- [Open Collection](collections/trint.opencollection.json)

## Common Properties

- [Authentication](authentication/trint-authentication.yml)
- [LinkedIn](https://www.linkedin.com/company/trint)
- [Website](https://trint.com/)
- [Documentation](https://dev.trint.com)
- [Plans](plans/trint-plans-pricing.yml)
- [Rate Limits](rate-limits/trint-rate-limits.yml)
- [Fin Ops](finops/trint-finops.yml)
- [Blog](https://trint.com/blog)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
