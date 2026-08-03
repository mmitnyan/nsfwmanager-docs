# Move to Folder
## One-Way File Organization to a Custom Destination

The **Move to Folder** action transfers flagged files to a directory you specify — either a configured default folder or any location you choose at the time of the action. Unlike quarantine, it is a direct move with no structured metadata or built-in restore workflow.

---

## What "Move to Folder" Does

When you use Move to Folder, NSFW Manager:

1. Moves the file from its current location to the destination folder
2. Optionally preserves the original subfolder structure inside the destination
3. Optionally creates a log file recording what was moved
4. Removes the file from the scan results list

The move is immediate and permanent in the sense that NSFW Manager does not track it further. If you move a file to a custom folder, restoring it to its original location is your responsibility.

---

## Move to Folder vs. Quarantine

Both actions move files away from their original location, but they serve different purposes:

| | Move to Folder | Quarantine |
|---|---|---|
| Destination | A folder you configure | The quarantine directory, organized by session and category |
| Metadata | None (just the file) | Full metadata: original path, detection score, engine, timestamp |
| Restore support | No built-in restore | One-click restore to original location from Quarantine Manager |
| Organization | Optional folder structure | Automatic: timestamped sessions with category subfolders |
| Log file | Optional | Automatic per session |
| Use case | "I know where these should go" | "I want to review and possibly restore these later" |

**Use quarantine when:**
- You are unsure about some detections and may want to restore files later
- You want a structured audit trail of what was moved when
- You are doing a first-pass review of a new folder

**Use Move to Folder when:**
- You have already reviewed the files and want to archive them in a specific location
- You are organizing content into a permanent archive structure
- You want the simplest possible workflow with no quarantine overhead

---

## Move to Folder vs. Delete

Moving to a folder keeps the file accessible in its new location, while deletion removes it from your drive (to the Recycle Bin or permanently). Use Move to Folder when:

- You want the content available in another context (a separate review drive, an archive)
- You are not ready to delete permanently but want it out of the main folder

---

## Options

### Preserve Folder Structure

When this option is enabled, NSFW Manager recreates the subfolder structure of the original file's location inside the destination folder.

**Example:** If you are scanning `C:\Users\Photos\` and move a file from `C:\Users\Photos\Vacation\2023\Italy\`, the file ends up at `<destination>\Vacation\2023\Italy\` rather than directly in `<destination>\`.

**When to enable:** You are scanning a large nested folder hierarchy and want to keep the organizational structure visible in the destination. This makes it easy to identify where each moved file came from without referencing a log.

**When to disable:** You want all moved files in a single flat folder. Simpler if you are just archiving everything to one location.

### Create a Log File

When enabled, NSFW Manager writes a text file alongside the moved files listing every file that was moved, its original path, detection score, and timestamp.

**When to enable:** You want an audit trail — for example, if you need to document what content was removed from a shared drive, or if you want to reference the list later without opening the destination folder.

**When to disable:** For personal use where you do not need a record. Saves a small amount of overhead and avoids an extra file in the destination.

---

## The Default Move Action Setting

In **Configuration → Directories**, you can set which action the main "Move" button uses by default: **Move to Quarantine** or **Move to Folder**. This controls the one-click button behavior in the main screen.

If you primarily archive files to a specific folder and rarely need quarantine, setting the default to Move to Folder saves you a click on every action.

If you prefer to review files before deciding, keep the default as Quarantine.

---

## Configuring the Destination Folder

The default destination for Move to Folder is set in **Configuration → Directories → Move Directory**. This is the folder used when you click the main "Move" button without additional prompts.

You can also choose a different destination each time by using **Send to Directory…** from the right-click menu or **Ctrl+T**, which opens a folder picker.
