# Main Screen
## Your Central Hub for Scanning and Reviewing Results

The **Main Screen** is where you spend most of your time in NSFW Manager. It displays scan results, provides a preview panel, and gives you direct access to all file actions.

---

## The Results List

The results list is divided into two sections:

**Detected files:** Every file whose detection score was at or above the configured threshold. Columns show the filename, folder path, detection score, and the label returned by the engine.

**Corrupted files:** Files that could not be decoded — corrupted images, unsupported codec variants, or files with wrong extensions. This section is hidden by default to keep the view clean.

Why two sections: Detected and corrupted files represent fundamentally different situations. A detected file is a normal file the engine analyzed and flagged. A corrupted file is a file the engine could not analyze at all. Mixing them would make it harder to understand what the scan found. The corrupted section is hidden by default because most users scanning their own photo libraries will encounter very few decode errors; showing the section by default would add visual noise for most users.

Why the corrupted section is useful: If you notice expected files are missing from results, enabling the corrupted section (Configuration → Detection → Show Corrupted Files section) reveals files that failed to decode. This helps distinguish "NSFW Manager considered this file safe" from "NSFW Manager could not read this file."

**Item counts:** Each section shows the number of items it contains, so you can see at a glance how many files were flagged and how many failed to decode.

---

## The Preview Panel

Clicking any file in the results list opens a preview in the side panel. For images, the preview displays the file at a fixed thumbnail size. For videos, it shows the frame that triggered the highest detection score.

The preview also shows: filename, file size, pixel dimensions, detection score, and detection label.

**Open Image / Open File:** Opens the file in your default system viewer or player.

**Open Directory:** Opens the folder containing the file in Windows Explorer. Note: Opening a directory consumes one of the 5 trial actions if you are in trial mode.

**Hide Image:** Hides the preview if you prefer more space for the results list.

---

## Scan Controls

**Scan Now:** Starts a new scan of the configured scan directory. The button changes to a progress indicator during scanning.

**Refresh:** After the first scan completes, this button replaces Scan Now. It re-scans the same folder with the same settings. Useful for checking whether new files have appeared or to apply a changed threshold without navigating to a different folder.

**Cancel:** Stops the scan in progress. The scan stops cleanly after the current file finishes, so partial results remain in the list.

**Clear Results:** Removes all results from the list without deleting any files from disk. A confirmation prompt appears first. Use this to reset the view before starting a fresh scan of a different folder.

---

## The Statistics Bar

The statistics bar at the bottom of the screen always shows:
- Total files scanned
- Number detected (above threshold)
- Number of decode errors
- Total scan duration
- Current engine name

These values help you quickly assess scan completeness and compare performance across runs.

---

## Right-Click Menu

Right-clicking any file in the results list opens a context menu with all available actions: Properties, Open/Play, Open Directory, Send to Default Folder, Send to Directory, Send to Quarantine, and Delete File(s).

When multiple files are selected, the header shows the count ("3 elements selected") and actions apply to all selected files.

---

## Keyboard Shortcuts

| Action | Shortcut |
|---|---|
| Open Properties | Ctrl+P |
| Send to Directory | Ctrl+T |
| Refresh | F5 |
| Delete (selected file) | Delete |
| Clear results (nothing selected) | Del |

For the full shortcut reference, see [Keyboard Shortcuts](../features/shortcuts.md).

---

## Related Pages

- [Properties Panel](./properties-panel.md) — detailed file view and per-file actions
- [Detection Threshold](../features/detection-threshold.md) — adjusting what gets flagged
- [Keyboard Shortcuts](../features/shortcuts.md) — full shortcut reference
