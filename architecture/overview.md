# How NSFW Manager Processes Your Files
## The Detection Pipeline from Folder to Results

This page explains what NSFW Manager does when you start a scan — from selecting a folder to displaying flagged results — in terms of what you observe and why each step is designed the way it is.

---

## Step 1: Folder Selection and File Collection

When you start a scan, NSFW Manager scans the selected folder **recursively** — it descends into every subfolder at any depth. There is no depth limit.

Before passing files to the AI engine, NSFW Manager first builds a list of files to process:

- Only files with extensions matching the enabled formats are included
- Junk files (Thumbs.db, .DS_Store, AppleDouble files) are skipped if junk exclusion is enabled
- Files above the configured size limit are skipped
- Files in excluded subdirectories (if configured) are skipped

This filtering step ensures the AI engine only sees files it can actually process.

---

## Step 2: Cache Check

If the scan cache is enabled, NSFW Manager checks whether a valid cached result exists for each file before sending it to the AI engine. A cached result is valid only if the file has not changed (same size, same modification time, same engine, same threshold — and same MD5 if that option is enabled).

Files with valid cached results are added to the results display instantly, without any AI analysis. This is why re-scanning a familiar folder with the cache enabled is so much faster than the first scan.

Files without a valid cache entry proceed to the analysis step.

See [Scan Cache](../features/scan-cache.md) for details.

---

## Step 3: AI Analysis

Each file that does not have a cached result is passed to the selected detection engine.

The engine returns:
- A **score** between 0.0 and 1.0
- A **label** describing what was detected (for example: explicit, suggestive, safe)

For video files, frames are extracted first and each frame is analyzed as an image. The highest frame score becomes the score for the video.

The analysis runs in the **background**: the user interface stays fully responsive during scanning. You can browse results that have already appeared while the scan continues processing other files.

---

## Step 4: Threshold Comparison and Results Display

After each file is analyzed, NSFW Manager compares the score against the configured detection threshold:

- Score at or above threshold: the file is added to the **Detected** section of the results list
- Score below threshold: the file is considered safe and not shown in results

If a file could not be decoded at all (corrupted file, unsupported codec), it is added to the **Corrupted files** section instead.

Results appear in real time — you do not need to wait for the entire scan to complete before reviewing and acting on already-detected files.

---

## Step 5: Preview and Actions

Once results are displayed, you can click any file to see a preview and its detection details in the [Properties Panel](../ui/properties-panel.md). You can also act on results immediately — quarantine, move, or delete — while the scan continues in the background.

After the scan completes, the progress bar is replaced by the total scan duration and a Refresh button. Pressing Refresh re-scans the same folder, applying the same settings.

---

## Design Principles

**Local only:** Every step described above happens on your machine. No file content, no detection results, no metadata leaves your computer. The only network activity is licence validation at startup.

**CPU-first by default:** GPU acceleration is optional for both image and video processing. The default is CPU-only, which works on every Windows machine and avoids driver compatibility issues. GPU can be enabled in Configuration → Engines for faster processing when supported hardware is available.

**Scan does not block review:** Scanning and reviewing results happen independently. A large scan over thousands of files does not prevent you from acting on files that have already been detected.

**Scan does not modify files:** The scan process only reads files. No modification, no copy, no upload. Files are only moved or deleted when you explicitly choose an action.

---

## Related Pages

- [Detection Engines](./engine.md) — details on each AI model
- [Scan Cache](../features/scan-cache.md) — how results are stored and reused
- [Detection Threshold](../features/detection-threshold.md) — how the threshold affects what gets flagged
- [Video Support](../features/video-support.md) — how video frame sampling works
