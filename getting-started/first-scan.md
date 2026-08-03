# Your First Scan
## NSFW Detection in Under a Minute

This guide walks you through your first scan with NSFW Manager from launch to taking action on results.

---

## Step 1: First Launch and Initial Configuration

When you open NSFW Manager for the first time, the Configuration Panel opens automatically. This happens because no scan directory, quarantine directory, or move directory has been set yet — NSFW Manager needs to know where to scan and where to put files before it can do anything useful.

**Why the panel opens automatically:** Rather than scanning a random default location, NSFW Manager asks you to explicitly choose your directories. This prevents accidental scans of system folders or unintended locations.

Set at minimum:
- **Scan Directory:** the folder you want to scan (e.g., your Photos folder or an external drive)
- **Quarantine Directory:** where flagged files will go when you quarantine them

You can leave the Move Directory at its default for now. Click OK to save and return to the main screen.

On subsequent launches, the Configuration Panel does not open automatically — your settings are remembered.

---

## Step 2: Start the Scan

Click **Scan Now** on the main screen.

NSFW Manager scans all files in the configured directory and every subfolder recursively. The progress bar shows the current file being processed and the percentage complete.

Results appear in real time as files are flagged — you do not need to wait for the full scan to finish before reviewing the first detections.

**Tip:** For your first scan of an unfamiliar collection, consider leaving the threshold at its default value of 0.50. You can always adjust and re-scan after seeing what the default catches. See [Detection Threshold](../features/detection-threshold.md) for guidance.

---

## Step 3: Review Detected Files

Detected files appear in the **Detected** section of the results list with their score and detection label.

Click any file to open the **Properties Panel** and see a preview alongside the detection details. The preview is the most useful tool for verifying whether a detection is accurate or a false positive.

**What you will typically see in a first scan:**
- Clearly explicit files with scores above 0.85
- Some borderline files (swimwear, art, or medical images) with scores between 0.50 and 0.75
- Corrupted or unsupported files in the Corrupted section (if you have enabled that section)

---

## Step 4: Take Action

For each detected file, you have several options:

**Quarantine (recommended for first-time scans):** Moves the file to your quarantine directory, preserving it for later review. This is the safest option because you can restore any quarantined file if it turns out to be a false positive. See [Quarantine](../features/quarantine.md).

**Move to Folder:** Moves the file to your configured archive folder. Use this when you know exactly where the file should go. See [Move to Folder](../features/move-to-folder.md).

**Delete:** Removes the file permanently or to the Recycle Bin, depending on your Configuration → General setting. See [Delete](../features/delete.md).

You can also right-click any file for a context menu with all available actions.

---

## Step 5: After the Scan

Once the scan completes, the progress bar is replaced by the total scan duration and a **Refresh** button. Pressing Refresh re-scans the same folder — useful if new files were added while you were reviewing results.

To scan a different folder, open Configuration and change the scan directory, or use the folder selector that appears when you click the Scan button.

---

## Related Pages

- [Detection Threshold](../features/detection-threshold.md) — tuning what gets flagged
- [Detection Engines](../architecture/engine.md) — choosing the right engine
- [Quarantine](../features/quarantine.md) — the recommended first-time action
- [Scan Cache](../features/scan-cache.md) — speeding up repeat scans
