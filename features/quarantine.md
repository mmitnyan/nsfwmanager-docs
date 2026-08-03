# Quarantine Mode  
## Safe Isolation & Controlled Review of Sensitive Files

The **Quarantine** feature allows users to isolate detected sensitive files in a secure location, review them later, and restore or delete them safely.  
It is designed to prevent accidental exposure, avoid surprises, and maintain full control over flagged content.

Quarantine is accessible from:
- Right‑click → **Quarantine**
- Action buttons in the main screen
- Double‑click (if configured)
- The dedicated **Quarantine Manager** panel

Introduced in v1.0.0 and significantly expanded in **[v2.0.0](ca://s?q=Open_v2.0.0_release_notes)** and **[v2.0.3](ca://s?q=Open_v2.0.3_release_notes)**.

---

## 🎯 Purpose

Quarantine provides a safe workflow for:
- Isolating sensitive or uncertain files  
- Reviewing flagged content without deleting it  
- Restoring files to their original location  
- Keeping the main folders clean and “surprise‑free”  
- Avoiding accidental exposure during browsing or sharing  
- Preparing files for deletion or reorganization

It is one of the core pillars of NSFW Manager’s privacy‑first design.

---

# 🧩 How Quarantine Works

When a file is quarantined:

1. It is **moved** to a secure quarantine directory.
2. Its **original path** is stored in the database.
3. The file becomes **hidden** from normal scans.
4. The user can **preview**, **restore**, or **delete** it from the Quarantine Manager.
5. A **log entry** is created for traceability.

Quarantine is fully compatible with:
- Image detection  
- Video detection  
- Multi‑engine results  
- The **[Properties Panel](ca://s?q=Open_properties_panel)**  
- The **[SQLite Cache](ca://s?q=Show_SQLite_schema)**

---

# 📁 Quarantine Directory Structure

Quarantined files are stored in:
%APPDATA%/NsfwManager/Quarantine/


Inside this directory, NSFW Manager organizes files by:
- Category (if enabled)
- Session (optional)
- Original extension

This structure ensures:
- No filename collisions  
- Easy restore  
- Predictable organization  

---

# 🧭 Quarantine Manager

The **Quarantine Manager** provides a dedicated interface to manage isolated files.

### Features:
- List of all quarantined files  
- Preview panel (image/video)  
- Restore button  
- Permanent delete button  
- File metadata  
- Original path display  
- Search and filtering  
- Session-based grouping  

The preview system is asynchronous, identical to the main screen preview.

---

# 🔄 Restore Workflow

Restoring a file:
- Moves it back to its original folder  
- Recreates missing directories if needed  
- Updates the cache  
- Removes quarantine metadata  
- Refreshes the main screen list  

If the original folder no longer exists, NSFW Manager prompts the user to choose a new location.

---

# 🗑 Delete Workflow

Deleting a quarantined file offers two options:

### **Recycle Bin**
- Safer  
- Allows undo  
- Recommended for most users

### **Permanent Delete**
- Irreversible  
- Useful for sensitive or unwanted content  
- Requires confirmation

Both workflows update:
- Logs  
- Cache  
- Quarantine metadata  

---

# ⚙️ Settings Related to Quarantine

Quarantine behavior is configurable in the **Settings** panel:

- Organize by category  
- Preserve folder structure  
- Create log file  
- Custom quarantine directory  
- Double‑click action (Quarantine)  
- Right‑click menu integration  

See **[Settings](ca://s?q=Open_settings_panel)** for details.

---

# 🧪 Engine & Cache Interaction

When a file is quarantined:
- Its cached detection result remains stored  
- It is excluded from future scans  
- Engine results remain accessible in the **Properties Panel**  
- MD5 validation ensures integrity  
- Restore revalidates the cache entry

This ensures consistent behavior across all engines.

---

# 🐞 Error Handling

Quarantine gracefully handles:
- Locked files  
- Missing original directories  
- Permission issues  
- Corrupted files  
- Duplicate filenames  
- Cache inconsistencies  

Clear error messages guide the user through recovery steps.

---

# 📦 Version History

### **v2.0.3**
- Right‑click “Quarantine”  
- Configurable double‑click action  
- Improved restore reliability  
- Better preview integration  
- Session-based grouping  

### **v2.0.2**
- Stability improvements  
- Better error messages  
- Documentation added  

### **v2.0.1**
- Video preview support in Quarantine Manager  

### **v2.0.0**
- Major refactor of quarantine logic  
- New Quarantine Manager  
- Category-based organization  
- Log file support  

### **v1.0.0**
- Initial quarantine feature  
- Basic restore/delete workflow  

---

# 📌 Summary

Quarantine is a core feature of NSFW Manager, providing a safe, controlled environment for handling sensitive files.  
It prevents accidental exposure, supports detailed review, and integrates seamlessly with the detection engines, preview system, and cache architecture.

It is one of the most important privacy‑focused features of the application.

---

