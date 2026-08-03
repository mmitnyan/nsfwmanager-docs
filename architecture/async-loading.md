# Background Scanning and Responsive UI
## Why NSFW Manager Never Freezes During a Scan

Scanning large folders and displaying previews of high-resolution images are computationally expensive operations. NSFW Manager is designed so that neither of these operations ever blocks the user interface — you can review, act on, and navigate results while a scan is still running.

---

## How the Scan Runs in the Background

When you start a scan, NSFW Manager launches the detection work in a separate background thread. The main interface remains fully responsive throughout:

- Results appear in the list as they are detected — you do not wait for the full scan to complete
- You can click on detected files to preview them while scanning continues
- You can quarantine, delete, or move files while more are being analyzed
- The progress bar and current file indicator update in real time
- You can cancel the scan at any time via the Cancel button; the scan stops cleanly after the current file finishes

This design means a scan of 50,000 files is no more disruptive to your workflow than a scan of 50 files.

---

## How Preview Loading Works

When you click on a file in the results list, the Properties Panel displays a preview. For large files — high-resolution photos from a DSLR, HEIC images from a phone, or video frames — loading and decoding the preview can take a moment.

NSFW Manager loads previews asynchronously:

1. The Properties Panel opens immediately with a placeholder
2. Image decoding runs in the background
3. The preview updates when decoding is complete
4. If you click a different file before the current preview finishes loading, the previous decode task is cancelled and the new one starts

This means you can quickly scroll through many detected files in the results list without experiencing any freezes or delays.

---

## Why This Matters for Large Collections

Without background processing, scanning 5,000 photos might lock the interface for several minutes, preventing you from doing anything else in the application. With background scanning:

- You can start reviewing and acting on early results while the scan processes the rest of the folder
- You can adjust settings or check the configuration panel without stopping the scan
- The application feels responsive even on slower hardware or when scanning from a network drive

---

## Video Preview Loading

For video files, generating the preview frame also runs asynchronously. The frame shown in the Properties Panel is the frame that triggered the highest detection score. Extracting and decoding it happens in the background, so the panel opens immediately and the frame appears once it is ready.

If GPU decoding is enabled, video frames load noticeably faster.

---

## Related Pages

- [Main Screen](../ui/main-screen.md) — the results list and action buttons
- [Properties Panel](../ui/properties-panel.md) — preview and metadata display
- [Video Support](../features/video-support.md) — frame extraction and GPU decoding
