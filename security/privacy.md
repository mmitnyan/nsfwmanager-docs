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

NSFW Manager collects **no usage data** of any kind:

- No analytics or crash reports
- No file names, file paths, or folder structures
- No detection results or scores
- No information about what you scan or how often you use the application
- No hardware telemetry beyond the hashed machine ID used for licence binding

---

## The Only Outbound Network Request

The single network request NSFW Manager makes is **licence validation at startup**.

This request sends:
- Your email address and licence key
- A hashed machine identifier (a one-way hash — the raw hardware data is not transmitted)
- The application's major version number

This request is:
- Sent over HTTPS (TLS 1.2 or higher)
- Signed with HMAC-SHA256 to prevent tampering
- The only time any data leaves your machine

**What is never sent in this request:** file names, file content, folder paths, detection scores, scan history, or any information about what you have scanned.

---

## Model Downloads

When you choose to download an optional engine model (The Laid-Back One fp16 or The Nit Picker onnx) from Configuration → Engines, NSFW Manager downloads a model file from a CDN over HTTPS. This download:

- Sends no personal data beyond a standard HTTPS request
- Downloads only the model file you requested
- Is triggered explicitly by you — it does not happen automatically

---

## Why Local Processing Matters

Scanning for sensitive content is inherently personal. A cloud-based scanning service would require uploading your images to a third-party server, where they might be logged, reviewed by staff, used for model training, or subject to data breach risk.

NSFW Manager avoids all of these risks by keeping everything local. The AI model is on your machine. The results stay on your machine. The only thing that leaves is a licence key verification handshake.

---

## Related Pages

- [Licence Security](./licence-security.md) — details on the licence validation request
- [SSL and Secure Communication](./ssl.md) — how the validation request is protected
