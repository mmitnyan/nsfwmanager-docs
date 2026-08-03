# Settings Overview
## Quick Reference for All Configuration Options

The Configuration Panel is the central place to adjust NSFW Manager's behavior. This page gives a brief overview of each tab. For deeper explanations of specific settings, follow the links to the relevant feature pages.

Open the Configuration Panel from the toolbar Settings button or from the menu.

---

## Directories Tab

Set the three folders NSFW Manager uses:
- **Scan Directory** — the folder to scan (scanned recursively)
- **Quarantine Directory** — where quarantined files are moved
- **Move Directory** — the destination for the Move to Folder action
- **Default Move Action** — whether the Move button uses quarantine or the custom folder

See: [Quarantine](features/quarantine.md), [Move to Folder](features/move-to-folder.md)

---

## Engines Tab

Choose the active AI engine and manage downloadable models:
- **Active engine** — select from The Just Perfect (int8), The Laid-Back One (fp16), The Nit Picker (onnx), or The Rebel (ifnude)
- **Download** — download optional engine models from within the app
- **Force CPU for photos** — use CPU for image inference (on by default)
- **Force CPU for videos** — use CPU for video decoding (on by default)

See: [Detection Engines](architecture/engine.md)

---

## Detection Tab

Control what gets scanned and how sensitive the detection is:
- **Detection threshold** — score at or above which a file is flagged (default 0.50)
- **Image formats** — enable or disable specific file format checkboxes
- **Exclude junk files** — skip system-generated files (Thumbs.db, .DS_Store, etc.)
- **Maximum file size (images)** — skip files above this size (default 100 MB)
- **Maximum file size (videos)** — skip video files above this size (default 1000 MB)
- **Frames to sample per video** — how many frames to analyze (default 10)
- **Enable video detection** — toggle video scanning on or off
- **Show Corrupted Files section** — display files that failed to decode

See: [Detection Threshold](features/detection-threshold.md), [Photo Support](features/photo-support.md), [Video Support](features/video-support.md)

---

## General Tab

Controls file actions and interface behavior:
- **File Deletion Mode** — Recycle Bin (default) or Permanent Delete
- **Confirm before delete** — show a dialog before any deletion (on by default)
- **Show completion popup** — brief notification after a successful action
- **Double-click action** — Open Properties, Move to Default Folder, Move to Directory, or Quarantine

See: [Delete](features/delete.md)

---

## Language Tab

Select the UI language: **English**, **French**, or **Spanish**. Takes effect after restart.

---

## Theme Tab

- **Light / Dark theme** — visual theme, takes effect after restart
- **AI Personality Style** — Funny (shows engine character portraits) or Professional (no portraits)

---

## Cache Tab

- **Enable scan cache** — persist scan results for faster repeat scans (off by default)
- **Use MD5 verification** — content-based integrity check (off by default)
- **Clear Cache** — remove all stored results and force a full re-analysis

See: [Scan Cache](features/scan-cache.md)

---

## Diagnostic Tab

Read-only system information for troubleshooting:
- ONNX Runtime version
- Available execution providers (CPU, DirectML, CUDA)
- GPU available / in use / GPU type
- Force CPU checkboxes (duplicate of Engines tab for convenience)

See: [Configuration Panel — Diagnostic Tab](ui/ConfigurationPanel.md#diagnostic-tab)
