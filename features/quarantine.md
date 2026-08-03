# Quarantine
## Safe Isolation and Controlled Review of Sensitive Files

The **Quarantine** feature moves flagged files to a secure, dedicated location where you can review them, restore them to their original path, or delete them — without those files being visible in normal folders while you decide.

Quarantine is the recommended first action for most workflows. It is reversible, organized, and keeps a complete record of everything that was moved.

---

## Why Use Quarantine Instead of Deleting Directly

Detection is not perfect. AI models can flag artwork, medical images, swimwear photos, or other legitimate content as NSFW — especially at lower thresholds. If you delete immediately without reviewing, you may lose files you wanted to keep.

Quarantine solves this by creating a **review stage** between "flagged" and "deleted":

1. Move flagged files to quarantine
2. Open the Quarantine Manager to review them
3. Restore any files that were false positives
4. Delete the rest

This workflow means a mistake is always recoverable. For first-time scans of a large, unfamiliar collection, quarantine is the safest approach.

---

## Why Use Quarantine Instead of Move to Folder

Unlike [Move to Folder](./move-to-folder.md), quarantine stores full metadata for every file:

- The original path the file came from
- The detection score and label
- The engine that flagged it
- The exact timestamp of the quarantine action

This metadata enables one-click restore, a human-readable summary report, and a searchable per-session log. If you need to undo an entire batch or review what was moved two sessions ago, quarantine provides the tools to do that; a custom folder move does not.

---

## How Quarantine Is Organized: Sessions and Categories

Each time you quarantine files, NSFW Manager creates a new **timestamped session folder**. Sessions are independent: quarantining files tomorrow will not mix with what you quarantined today.

Inside each session, files are organized into **category subfolders** based on the detection label:

| Subfolder | Contents |
|---|---|
| `explicit/` | Files labeled as explicit, nude, pornographic, or sexual |
| `suggestive/` | Files labeled as suggestive, partial nudity, or underwear |
| `unsafe/` | Files labeled as unsafe or inappropriate but not explicitly NSFW |
| `unknown/` | Files whose label did not match any known category |
| `errors/` | Files that could not be moved cleanly |

**Why sessions:** A session is a complete, isolated record of one quarantine action. You can restore or delete an entire session at once. If you realize an entire scan was misconfigured (wrong threshold, wrong folder), you can undo it as a batch rather than file by file.

**Why categories:** Different categories typically require different decisions. You might be comfortable with suggestive images (swimwear, underwear) staying in your library, but want all explicit content removed. Category folders let you review one group at a time and make decisions at the category level rather than file by file.

**Why the "organize by category" option:** In Configuration → Directories, you can disable category subfolder organization. When disabled, all files in a session are placed in a single flat folder. Use this if you prefer a simpler layout and always make decisions on individual files rather than by category.

---

## What Gets Stored in Each Session

Each session folder contains:

- The moved files, organized into category subfolders
- `session_metadata.json` — the engine name, model variant, and creation timestamp for the session
- `quarantine_log.json` — a per-file record with original path, quarantine path, category, score, label, timestamp, and engine
- `quarantine_report.txt` — a human-readable summary showing total files moved, broken down by category and by confidence level (high, medium, low)

**Confidence levels:** High means the detection score was above 80%, Medium is 50–80%, and Low is below 50%. The report groups files by these tiers so you can see at a glance how many detections were high-confidence vs. borderline.

---

## The Quarantine Manager

Open the Quarantine Manager from the application menu to manage all quarantine sessions.

The main list shows every session that still has files in it. Sessions that have been fully restored or deleted are automatically hidden from the list.

**Columns:** Session ID, date and time, number of files, folder path.

**Session actions:**
- **Open Folder** — open the session folder directly in Windows Explorer
- **Detail** — view thumbnails of every file in the session, with its score, original path, quarantine date, and engine
- **Restore** — move all files in the selected sessions back to their original paths. If the original folder no longer exists, it is recreated. If a file with the same name already exists at the destination, the restored file gets a `_restored_N` suffix to avoid overwriting anything.
- **Delete** — permanently delete the entire session and all its files (with a confirmation dialog)

You can select multiple sessions at once for bulk restore or delete.

---

## The Quarantine Directory

The quarantine directory is fully configurable in **Configuration → Directories**. You can set it to any local folder. The default is a location in your user profile.

The quarantine directory should be on the **same drive** as the files you are scanning, when possible. Moving files between drives works but is slower (the file must be copied then deleted rather than simply renamed).

---

## Restoring Files

When you restore files from the Quarantine Manager:

- Each file is moved back to its **exact original path**
- If the folder that contained it was deleted, NSFW Manager recreates it
- If a file already exists at the destination with the same name, the restored file is saved with a suffix (`_restored_1`, `_restored_2`, etc.) so neither file is overwritten
- After restoring all files in a session, the now-empty session folder is removed from the quarantine directory, and the session disappears from the Quarantine Manager list

---

## Accessing Quarantine from the Main Screen

You can send files to quarantine from multiple places:

- Right-click → **Send to Quarantine**
- The main action button (if quarantine is set as your default action in Configuration → Directories)
- Double-click, if you have configured the double-click action to quarantine (Configuration → General)
- Keyboard shortcut **Q** when a file is selected in the main results list

---

## Related Pages

- [Move to Folder](./move-to-folder.md) — for one-way moves without structured metadata
- [Delete](./delete.md) — for permanent or Recycle Bin removal
- [Detection Threshold](./detection-threshold.md) — for adjusting what gets flagged in the first place
