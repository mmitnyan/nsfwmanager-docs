# Privacy and Local Processing
## Your Files Never Leave Your Computer

NSFW Manager is designed with a strict privacy-first approach. Every step of the detection process happens entirely on your machine. No image, no video frame, no filename, and no detection result is ever transmitted to an external server.

---

## What Is Processed Locally

**Images and videos:** Loaded, decoded, and analyzed by the AI engine running on your machine. The engine model is a file stored in your user profile — no cloud inference, no remote AI service.

**Video frames:** Extracted locally. The frames are never stored permanently unless you are viewing a preview in the Properties Panel (where they are held in memory temporarily).

**Scan results:** Stored in a local database in your user profile. Never transmitted.

**Preview thumbnails:** Generated locally and stored in the local cache if caching is enabled.

**Quarantine metadata:** Stored in the quarantine session folders on your machine.

---

## What Is Not Collected

NSFW Manager collects no usage data tied to your files or activity:

- No analytics or crash reports
- No file names, file paths, or folder structures
- No detection results or scores
- No information about what you scan or how often you use the application

The only exception is the minimal anonymous startup ping described below, which is sent solely while the application is used in trial or unlicensed mode and stops permanently once a paid licence is activated.

---

## Outbound Network Requests

NSFW Manager makes two possible outbound network requests, depending on your licence status.

### Licence Validation (Always, at Startup)

The application sends a licence validation request every time it starts, regardless of licence status.

This request sends:
- Your email address and licence key
- A hashed machine identifier (a one-way hash — the raw hardware data is not transmitted)
- The application's major version number

This request is:
- Sent over HTTPS (TLS 1.2 or higher)
- Signed with HMAC-SHA256 to prevent tampering

**What is never sent in this request:** file names, file content, folder paths, detection scores, scan history, or any information about what you have scanned.

### Anonymous Telemetry Ping (Trial / Unlicensed Use Only)

As long as NSFW Manager is used **without an active paid licence** (trial or unlicensed use), the application also sends a minimal anonymous ping to `api.nsfwmanager.com` each time it starts. The ping is fire-and-forget, sent in the background with a 5-second timeout, and fails silently if it cannot reach the server.

This ping contains only:
- A one-way SHA-256 hash of a random identifier generated locally on first launch (the raw identifier is never transmitted and cannot be reversed to identify you or your machine)
- The application version
- Your Windows version (10 or 11)
- Your licence tier (always "trial", since this ping is only ever sent for unlicensed use)

No file, filename, folder path, scan result, detection score, or personal data is ever included in this ping.

**This telemetry stops automatically as soon as a valid paid licence is activated on the machine.**

---

## Model Downloads

When you choose to download an optional engine model (The Laid-Back One fp16 or The Nit Picker onnx) from Configuration → Engines, NSFW Manager downloads a model file from a CDN over HTTPS. This download:

- Sends no personal data beyond a standard HTTPS request
- Downloads only the model file you requested
- Is triggered explicitly by you — it does not happen automatically

---

## Why Local Processing Matters

Scanning for sensitive content is inherently personal. A cloud-based scanning service would require uploading your images to a third-party server, where they might be logged, reviewed by staff, used for model training, or subject to data breach risk.

NSFW Manager avoids all of these risks by keeping everything local. The AI model is on your machine. The results stay on your machine. The only things that ever leave your machine are the licence key verification handshake and, while unlicensed, the minimal anonymous telemetry ping described above.

---

## Related Pages

- [Licence Security](./licence-security.md) — details on the licence validation request
- [SSL and Secure Communication](./ssl.md) — how the validation request is protected
