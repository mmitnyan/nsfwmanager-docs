# Delete & Permanent Delete
## Safe Removal of Sensitive Files

NSFW Manager provides two ways to remove detected files: **Recycle Bin delete** (safe and reversible) and **Permanent delete** (immediate and irreversible). The right choice depends on how confident you are in the detections and how you prefer to manage your workflow.

---

## Recycle Bin Delete (Default)

When you delete a file using the Recycle Bin mode, the file is moved to the Windows Recycle Bin. It stays there until you empty the Recycle Bin manually, and it can be restored at any time from Explorer before then.

**Why this is the default:** AI detection is not infallible. A first-pass scan at a moderate threshold will often flag some legitimate images alongside actual NSFW content. Using the Recycle Bin ensures you have a safety net — if you accidentally delete a photo you wanted to keep, you can recover it.

**When to use Recycle Bin:**
- First-time scan of any folder you have not reviewed before
- Scanning a large mixed collection where false positives are expected
- Any time you value recoverability over immediate finality

**Limitations:**
- Files move to the Recycle Bin only if the drive has one (local NTFS drives). Network drives and some external drives do not support Recycle Bin; files on those drives will be deleted permanently even in this mode.
- Very large files may bypass the Recycle Bin depending on your Windows settings (if the Recycle Bin size limit is set lower than the file size).

---

## Permanent Delete

Permanent delete removes the file immediately, bypassing the Recycle Bin entirely. The file is gone as soon as the operation completes and cannot be recovered through normal means.

**When to use Permanent delete:**
- You have already reviewed the detected files and are certain they should be removed
- You are running a recurring cleanup of a folder you scan regularly — you know the detection is reliable for that content
- You are working on a drive without Recycle Bin support and want consistent behavior
- Disk space is a concern and you do not want deleted files occupying the Recycle Bin

**Why confirmation is required by default:** Even in a reviewed workflow, permanent deletion is not reversible. The confirmation dialog is a last checkpoint. If you are doing bulk operations on many files and have already reviewed them, you can disable the confirmation in Configuration → General → Confirm before delete.

---

## Configuring the Delete Mode

The delete mode is configured globally in **Configuration → General → File Deletion**. It applies to all delete actions throughout the application — from the main screen, the right-click menu, and the Properties Panel.

You choose once per session which behavior you want. There is no per-file override.

---

## Confirmation Dialog

By default, NSFW Manager shows a confirmation dialog before any deletion. The dialog text changes based on the mode:

- **Recycle Bin mode:** "Send X file(s) to the Recycle Bin?"
- **Permanent mode:** "Permanently delete X file(s)? This cannot be undone."

**Why it is on by default:** Accidental deletions are easy to trigger, especially with keyboard shortcuts. The confirmation adds one extra click that prevents mistakes.

**When to disable it:** If you are in a verified workflow (you have reviewed the files before acting), the confirmation dialog adds friction without benefit. Disable it in Configuration → General → Confirm before delete.

---

## Completion Popup

After a successful deletion, NSFW Manager shows a brief confirmation popup ("X file(s) deleted"). You can disable this in Configuration → General → Show completion popup.

**When to keep it:** Useful for small batches where you want confirmation that the operation succeeded.

**When to disable it:** If you are processing large numbers of files in sequence, the popup appearing after every action becomes disruptive.

---

## Multi-Selection and Batch Delete

You can select multiple files in the results list (using Shift-click or Ctrl-click) and delete them all at once. The confirmation dialog shows the total count. This is faster than deleting one by one for large batches.

---

## Keyboard Shortcuts

| Action | Shortcut |
|---|---|
| Delete (Recycle Bin or Permanent based on your setting) | `Delete` |
| Permanent delete (regardless of setting) | `Shift+Delete` |

---

## Related Pages

- [Quarantine](./quarantine.md) — for a reversible, structured holding area before deciding to delete
- [Move to Folder](./move-to-folder.md) — for archiving files to a different location instead of deleting
