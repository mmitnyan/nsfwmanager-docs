# Performance Troubleshooting  
## Improving Scan Speed, Engine Selection, and Overall Responsiveness

This document explains how NSFW Manager handles performance, how engine selection affects scan speed, and how configuration options influence photo and video processing. It also provides guidance for users experiencing slow scans, high CPU usage, or UI delays.

---

## 📌 Overview

NSFW Manager performs local analysis on photos and videos using optimized ONNX models and Python-based processing pipelines. Performance varies depending on:

- the selected detection engine  
- CPU vs GPU usage  
- file formats and sizes  
- video decoding backend  
- enabled or disabled cache  
- user configuration choices  

This guide summarizes how each option affects performance and how to optimize the application for your system.

---

# ⚡ Engine Performance

The **Engine** tab contains the most impactful performance settings.

NSFW Manager provides multiple engines, ordered from **fastest to slowest**:

### **NSFW Manager Engines (Commercial)**
1. **int8 – Fast – “The Laid‑Back One”**  
   - CPU or GPU  
   - Fastest engine  
   - Lower precision, ideal for large batches

2. **fp16 – Balanced – “The Just Right”**  
   - GPU only  
   - Balanced speed and accuracy  
   - Requires disabling “Force CPU” in Diagnostics

3. **full – Maximum – “The Nit Picker”**  
   - CPU or GPU  
   - Highest accuracy  
   - Slowest engine

### **Optional Engines (GPLv3)**
1. **ifnude – Fast – “The Rebel”**  
2. **ifnude – Default – “The Rebel”**

These engines are fast but GPLv3 and therefore optional.

### Notes on CPU vs GPU
- GPU is not always faster; performance depends on GPU quality.  
- Some systems scan faster on CPU, especially with int8 or full models.  
- fp16 requires GPU and is unavailable when “Force CPU” is enabled.

### Detection Threshold
Changing the threshold **does not affect scan speed**.

---

# 🖼 Photo Detection Performance

The **Photo Detection** tab allows fine‑grained control over which image formats are scanned.

### Supported Formats (20 total)
`jpg bmp tiff ppm jp2 jpx heif jpeg gif webp pgm j2k dds avif png tif ico pbm jpf heih`

### Performance Tips
- Disabling formats reduces scan time.  
- Excluding “scan‑irrelevant files” skips filenames starting with `.` (e.g., `.filename.jpg`).  
- Large camera RAW‑style images (50–80 MB) take longer to decode.  
- “Maximum file size to scan (MB)” can skip extremely large files:
  - Default: **100 MB**
  - Range: **0–2000 MB** (0 = no limit)

This is useful when:
- Phone photos are typically <10 MB  
- DSLR/Canon/Nikon images are extremely large  
- Users want to skip professional camera formats to speed up scanning

---

# 🎥 Video Detection Performance

The **Video Detection** tab mirrors the photo settings.

### Supported Formats
`mp4 mov mkv webm avi`

All formats are enabled by default.

### Video Exclusions
Skips files starting with `.` (temporary or hidden files).

### Codec Support
- **OpenCV (default)**  
  - Fastest  
  - Falls back to FFmpeg automatically if decoding fails  
- **Force FFmpeg for all videos**  
  - More stable  
  - Significantly slower  
  - Recommended only for problematic video libraries

### Video Parameters
- **Maximum video size**  
  - Default: **1000 MB**  
  - Range: **0–10000 MB**  
  - Larger videos take longer to decode

- **Frames to sample per video**  
  - Default: **10**  
  - Range: **1–100**  
  - More frames = slower scan  
  - Fewer frames = faster but less thorough

---

# ⚙️ Cache Performance

The **Cache** tab contains two options:

### **Enable scan cache**  
- Disabled by default  
- First scan is similar to normal scan  
- Subsequent scans of the same directory are **extremely fast**  
- Ideal for folders that are frequently updated (e.g., iPhone sync folders)

### **Use MD5 verification**  
- Disabled by default  
- Ensures cached entries are valid  
- Slightly slower on re‑scan due to MD5 hashing  
- Recommended when files may change between scans

---

# 🛠 Diagnostic Options (CPU/GPU Control)

The **Diagnostic** tab contains two important toggles:

### **Force CPU instead of GPU for photos**  
### **Force CPU instead of GPU for videos**

Both are enabled by default to avoid GPU‑related support issues.

Effects:
- fp16 engine becomes unavailable unless GPU is enabled  
- CPU engines may outperform GPU on low‑end or older GPUs  
- GPU engines may outperform CPU on modern NVIDIA/AMD cards

---

# 📁 Directory Settings

The **Directories** tab affects workflow speed, not scan speed.

### Default Locations
- **Scan Directory:** `<user>\Pictures`  
- **Quarantine Directory:** `AppData\Local\NsfwManager\Quarantine`  
- **Move Directory:** `<user>\Documents\MyPrivatePictures`

### Default Action for Selected Files
- Move to quarantine (default)  
- Move to custom folder  

Choosing the right default action reduces UI clicks and speeds up workflow.

---

# 🧩 General Settings

### File Deletion
- **Recycle Bin** (recoverable)  
- **Permanent Deletion** (irreversible)

### Confirmation Options
- “Ask for confirmation before deleting file”  
- “Show confirmation popup after file action”

Disabling confirmations speeds up workflow for advanced users.

### Double‑Click Action
- Open Properties (default)  
- Move to Default Folder  
- Move to Directory  
- Move to Quarantine  

Customizing this improves productivity.

---

# 🎨 Theme & Personality

### Theme
- Light  
- Dark (default)

### AI Personality Style
- Funny (default)  
- Professional  

This affects UI presentation and engine naming, not performance.

---

# 📊 Main Screen Performance Notes

The main screen displays:

- File list  
- Score  
- Reason  
- Preview  
- Statistics  
- Actions (delete, move, open directory, quarantine management)

Previewing extremely large images or videos may take longer depending on file size and codec.

---

# 📁 Log Locations

Logs can help diagnose performance issues:

%APPDATA%\Roaming\NsfwManager\logs\NsfwManager.log
%LOCALAPPDATA%\NsfwManager\logs\startup.log
%LOCALAPPDATA%\NsfwManager\logs\execution.log


These logs include:
- decoding errors  
- slow file warnings  
- video thumbnail extraction issues  
- engine initialization details  

---

# 📌 Summary

Performance depends on:

- selected engine  
- CPU vs GPU usage  
- enabled formats  
- file sizes  
- video frame sampling  
- cache settings  
- codec backend  
- system hardware  

NSFW Manager provides extensive configuration options to tailor performance to your system and workflow.

---
