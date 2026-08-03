# Delete & Permanent Delete  
## Safe Removal of Sensitive Files  
### NSFW Manager — Full Specification of Deletion Workflows

NSFW Manager provides two deletion modes designed for safety, reliability, and full control over sensitive files:

- **Recycle Bin Delete** (safe, reversible)  
- **Permanent Delete** (irreversible, secure)

This document describes **every detail** of the deletion subsystem, including file handling, fallback logic, cache updates, quarantine integration, Windows API behavior, and error handling.

---

# 🗑 Recycle Bin Delete  
## Safe, reversible deletion using Windows Shell API

Recycle Bin deletion uses the Windows Shell API to move files to the system Recycle Bin.

### Behavior
- File is moved to the Recycle Bin  
- Operation is **reversible**  
- Windows keeps metadata (original path, deletion date)  
- Works for both **images** and **videos**  
- Works for files in **Quarantine** and **Main Screen**  
- Cache entry is **invalidated** immediately  

### Advantages
- Safest deletion method  
- Allows undo  
- Prevents accidental data loss  
- Recommended for most users  

### Limitations
- Recycle Bin must be enabled on the drive  
- Network drives may not support Recycle Bin  
- Very large files may bypass Recycle Bin depending on Windows settings  

---

# ❌ Permanent Delete  
## Irreversible deletion using secure file removal

Permanent Delete bypasses the Recycle Bin and removes the file immediately.

### Behavior
- File is deleted using Windows secure delete API  
- No recovery possible  
- Cache entry is removed  
- Quarantine metadata is removed  
- Preview panel closes automatically  
- File disappears from the result list instantly  

### Use Cases
- Sensitive content  
- Unwanted files  
- Cleanup operations  
- Automated workflows  

### Safety
- Requires confirmation  
- Cannot be undone  
- Clear warning message  

---

# 🧩 Integration with Other Components

## 1. **Quarantine**
Deleting from Quarantine:
- Removes file from quarantine directory  
- Removes original path metadata  
- Updates logs  
- Updates cache  
- Refreshes Quarantine Manager list  

Related:  
**[Quarantine](ca://s?q=Open_quarantine_feature)**

---

## 2. **Properties Panel**
Deletion from Properties Panel:
- Closes panel automatically  
- Updates main screen  
- Updates cache  
- Updates thumbnails  

Related:  
**[Properties Panel](ca://s?q=Open_properties_panel)**

---

## 3. **Main Screen**
Deletion from Main Screen:
- Removes item from result list  
- Updates scan statistics  
- Updates cache  
- Refreshes preview  

Related:  
**[Main Screen](ca://s?q=Open_main_screen)**

---

## 4. **Cache System**
Deletion triggers:
- Removal of cached engine results  
- Removal of cached thumbnails  
- Removal of MD5 entry  
- Removal of metadata  

Related:  
**[Cache System](ca://s?q=Show_SQLite_schema)**

---

# 🔧 File Handling & Fallback Logic

NSFW Manager includes robust fallback logic to handle problematic files.

## Locked Files
If a file is locked by another process:
- Clear error message  
- Suggest closing the app using the file  
- Retry option  
- No crash  

## Missing Files
If the file was moved externally:
- Cache entry removed  
- Warning displayed  
- Item removed from list  

## Permission Errors
If Windows denies access:
- Error message  
- Suggest running as admin (rare)  
- No crash  

## Network Drives
Behavior depends on Windows:
- Recycle Bin may not exist  
- Permanent Delete used automatically  
- Clear message shown  

---

# 🖼 Image & Video Delete Behavior

Deletion works identically for:
- Images (JPG, PNG, BMP, TIFF, WEBP, etc.)  
- Videos (MP4, MOV, MKV, AVI, WEBM, etc.)

### Video-specific behavior
- Preview player stops immediately  
- Thumbnail cache removed  
- Video metadata removed  

### Image-specific behavior
- High-resolution preview closed  
- EXIF metadata removed from cache  

---

# 🧪 Batch Delete Behavior

When deleting multiple files:
- Operations run asynchronously  
- UI remains responsive  
- Each deletion updates cache  
- Errors are isolated per file  
- Progress indicator shown  

Batch delete is optimized for:
- Large folders  
- High-volume scans  
- Automated cleanup  

---

# ⌨️ Keyboard Shortcuts

| Action | Shortcut |
|-------|----------|
| Delete (Recycle Bin) | **Delete** |


Related:  
**[Shortcuts](ca://s?q=Open_shortcuts_feature)**

---

# 🛡 Safety Features

NSFW Manager includes multiple safety layers:

- Confirmation dialog for permanent delete  
- Clear warnings for irreversible actions  
- No silent permanent delete  
- No background deletion without user action  
- No deletion without preview availability  
- No deletion of files outside user-selected folders  

---

# 📦 Version History

### v2.0.3
- Improved permanent delete workflow  
- Better error messages  
- Faster cache invalidation  
- Preview auto-close  
- Right-click delete added  

### v2.0.2
- Stability improvements  
- Better handling of missing files  

### v2.0.1
- Unified delete for images & videos  

### v2.0.0
- New delete engine  
- Quarantine-aware deletion  
- Cache-aware deletion  

### v1.0.0
- Initial delete/permanent delete options  

---

# 📌 Summary

NSFW Manager provides a robust, safe, and reliable deletion system:

- Recycle Bin delete (safe, reversible)  
- Permanent delete (secure, irreversible)  
- Full integration with preview, cache, quarantine, and main screen  
- Advanced fallback logic  
- Clear error handling  
- Optimized batch deletion  
- Full support for images and videos  

This is the **complete** specification for deletion behavior in NSFW Manager.

---
