# Properties Panel
## Detailed File Information and Per-File Actions

The **Properties Panel** gives you an in-depth view of any individual file in your scan results: full metadata, a preview, the detection result, and every action you can take on that file — all in one place.

Open it by clicking any file in the results list, pressing Ctrl+P, double-clicking a file (if configured), or selecting Properties from the right-click menu.

---

## Why the Properties Panel Exists

The main results list is optimized for scanning through many files quickly — it shows filename, score, and label at a glance. The Properties Panel is optimized for making a decision about a single file:

- You can see the full-size preview before deciding to delete or quarantine
- You see why the file was flagged (the engine label, not just a score)
- You see the file path, size, and dimensions to confirm you have the right file
- All actions are available in one click without going back to the list

This matters most when a file's score is borderline — say 0.55 with a threshold of 0.50. Looking at the preview in the panel helps you decide whether the detection is accurate or a false positive.

---

## File Metadata

The top section shows:
- Full file path
- File name
- File size
- Last modified date
- Image dimensions (width × height) for images
- Duration, codec, and resolution for videos

---

## Preview Area

**Image files:** The preview displays the image in a fit-to-panel view. Loading is asynchronous — the panel opens immediately and the image appears once decoded. Large images (high-resolution DSLR photos, HEIC from phones) load in the background without blocking the interface.

**Video files:** The panel shows the specific frame that triggered the highest detection score — the "most flagged" moment in the video. The frame is decoded in the background.

**Why the panel shows the highest-score frame for videos:** This is the most useful frame to review when deciding whether to act on the detection. Showing a random frame or the first frame would often show content that looks fine, making it harder to verify the detection. The highest-score frame gives you the best evidence for the detection decision.

---

## Detection Results

Shows the engine that analyzed this file, the model variant used, the detection score, the threshold that was active, and the label assigned by the engine.

If the result came from cache, the panel indicates this and shows when the cached result was recorded. This helps you assess whether the cached result is still relevant (for example, if the file has since been modified, or if your threshold has changed).

---

## Actions

All file actions are available directly from the Properties Panel:

- **Open file location** — opens the folder in Windows Explorer
- **Send to Directory (Ctrl+T)** — move to a custom folder of your choice
- **Send to Default Folder** — move to your configured default folder
- **Send to Quarantine** — move to quarantine (see [Quarantine](../features/quarantine.md))
- **Delete / Permanent Delete** — remove the file (see [Delete](../features/delete.md))
- **Copy metadata** (Ctrl+C) — copy file path and detection details to clipboard
- **Copy MD5** (Ctrl+Shift+C) — copy the file's MD5 hash if computed
- **Refresh preview** (F5) — re-decode the preview

---

## Asynchronous Loading Behaviour

The Properties Panel never freezes the interface. When you click rapidly between files in the results list, each file's decode task is started and cancelled if you move to the next before it completes. Only the currently selected file's preview will decode to completion.

---

## Related Pages

- [Main Screen](./main-screen.md) — the results list that feeds into this panel
- [Quarantine](../features/quarantine.md) — the quarantine action available from this panel
- [Delete](../features/delete.md) — delete mode and confirmation settings
- [Background Scanning and Responsive UI](../architecture/async-loading.md) — why loading is async
