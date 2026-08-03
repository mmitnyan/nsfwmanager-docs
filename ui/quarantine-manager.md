# Quarantine Manager
## Reviewing, Restoring, and Deleting Quarantine Sessions

The **Quarantine Manager** lists every quarantine session created on this machine, lets you inspect the files inside each one, restore originals to their source location, or permanently delete sessions you no longer need.

---

## How to Open

- Click **Manage Quarantine** in the left panel of the main screen
- Or open the **Actions** menu → **Manage Quarantine**

The Quarantine Manager is always available regardless of licence status — you can always access your quarantined files even during trial mode.

---

## Session List

The main area shows one row per quarantine session, sorted from most recent to oldest.

| Column | What it shows |
|---|---|
| Session | Timestamp-based identifier (e.g., `session_20260801_143022_041`) |
| Date | Date and time the session was created |
| Files | Number of files currently present on disk inside this session (files already restored are excluded from the count) |
| Path | Full path to the session folder on disk |

A **session** is created each time you move one or more files to quarantine. A single batch operation — even if it covers hundreds of files — produces exactly one session. This keeps sessions meaningful and easy to trace back to a specific scan run.

Sessions where all files have been restored are automatically removed from the list.

**Multi-select:** Hold Ctrl or Shift to select multiple sessions. Restore and Delete operations apply to all selected sessions at once.

---

## Action Buttons

**Refresh** — Rescans the quarantine directory and updates the list. Use this if you have manually moved files outside the application.

**Open Folder** — Opens the selected session folder in Windows Explorer. Useful for manual inspection or copying individual files.

**Restore** — Moves all files in the selected session(s) back to their original locations. If the original path no longer exists, the parent folder is recreated. If a file with the same name already exists at the destination, the restored file receives a `_restored_N` suffix to avoid overwriting. After restore, the session disappears from the list if it becomes empty.

**Delete** — Permanently removes the selected session folder(s) and all files inside them. A confirmation dialog is shown before deletion. This action cannot be undone — the files are not sent to the Recycle Bin.

**Detail** (enabled when one session is selected) — Opens the [Session Detail view](#session-detail).

**Close** — Closes the Quarantine Manager.

---

## Session Detail

Double-clicking a session, or clicking **Detail**, opens the Session Detail dialog for that session.

The detail view shows each file in the session as a card with:

- **Thumbnail** (120 × 120 px) — a preview rendered from the quarantined file on disk
- **Category** — the detection category assigned when the file was quarantined: `explicit`, `suggestive`, `unsafe`, `unknown`, or `errors`
- **Detection score** — the AI confidence score at the time of quarantine
- **Filename** — the original filename
- **Original directory** — where the file came from before quarantine

Only files physically present on disk inside the session folder are shown. If any files were already restored or manually removed, they are excluded from the list automatically.

---

## Session Folder Structure

Each session folder contains:

```
session_YYYYMMDD_HHMMSS_mmm/
├── explicit/          ← files above the explicit threshold
├── suggestive/        ← files scoring in the suggestive range
├── unsafe/            ← files classified as unsafe
├── unknown/           ← files with no clear classification
├── errors/            ← files that caused processing errors
├── session_metadata.json   ← session info (ID, timestamp, statistics)
├── quarantine_log.json     ← full restore map (original paths)
└── quarantine_report.txt   ← human-readable summary
```

The JSON log is what makes restore possible — it records the exact original path for every file. Do not delete or modify these log files manually.

---

## Related Pages

- [Quarantine](../features/quarantine.md) — how the quarantine system works and why it exists
- [Move to Folder](../features/move-to-folder.md) — alternative to quarantine for immediate sorting
- [Main Screen](./main-screen.md) — where the Manage Quarantine button lives
