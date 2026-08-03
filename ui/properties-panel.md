# Properties Panel  
## Detailed File Information & Media Preview

The **Properties Panel** provides an in-depth view of any scanned file.  
It centralizes metadata, engine results, preview capabilities, and contextual actions in a single, easy-to-access interface.

The panel is accessible from:
- Right‑click → **Properties**
- Double‑click (if configured)
- Keyboard shortcut **Ctrl+P**

This feature was introduced in **[v2.0.3](ca://s?q=Open_v2.0.3_release_notes)** as part of the UX expansion.

---

## 🎯 Purpose

The Properties Panel allows users to:
- Inspect file metadata (size, timestamps, dimensions)
- View detection results per engine/model
- Preview images and videos
- Understand why a file was flagged
- Perform contextual actions (open folder, move, delete)
- Compare cached vs. real-time detection values

It is designed to be fast, responsive, and fully asynchronous.

---

# 🧩 Panel Layout

The Properties Panel is divided into four main sections:

---

## 1. **File Metadata**

Displays essential information about the file:

- Full path  
- File name  
- Size (bytes, KB, MB)  
- Last modified timestamp  
- Creation timestamp  
- Dimensions (for images)  
- Duration, codec, resolution (for videos)  
- MD5 hash (if enabled)

This metadata is also used by the **SQLite scan cache**  
→ see **[Cache System](ca://s?q=Show_SQLite_schema)**.

---

## 2. **Preview Area**

### 🖼 Image Preview
- High‑resolution display  
- Async loading (prevents UI freeze)  
- Zoom and fit-to-window behavior  
- Supports large images (50–80 MB)

### 🎥 Video Preview
- Embedded video player  
- Frame extraction  
- Seek bar  
- GPU decoding (d3d11va) with CPU fallback  
- Thumbnail generation

Video preview was introduced in **[v2.0.1](ca://s?q=Open_v2.0.1_release_notes)**.

---

## 3. **Engine Results**

Shows detection results for each engine/model:

- Engine name (int8, fp16, full, ifnude)
- Model variant
- Score
- Label
- Threshold used
- Cached vs. real-time comparison
- Timestamp of cached result

This section is directly powered by the SQLite table `engine_results`.

If multiple engines are enabled, results are stacked vertically.

---

## 4. **Actions**

Contextual actions available directly from the panel:

- **Open file location**
- **Move to Directory** (Ctrl+T)
- **Move to Folder**
- **Quarantine**
- **Delete** / **Permanent Delete**
- **Copy metadata**
- **Copy MD5**
- **Refresh preview**

These actions mirror the right‑click menu introduced in **v2.0.3**.

---

# ⚙️ Behavior & Performance

### **Asynchronous Loading**
The panel loads preview and metadata asynchronously:
- UI remains responsive
- Switching files cancels previous tasks
- Large images and videos do not freeze the interface

### **Cache Integration**
If a cached result exists:
- It is displayed instantly
- Threshold and engine validation ensure correctness
- MD5 validation (optional) ensures integrity

### **Error Handling**
The panel gracefully handles:
- Corrupted images
- Unsupported video codecs
- Missing files
- Locked files
- Cache inconsistencies

---

# 🧭 Keyboard Shortcuts

| Action | Shortcut |
|-------|----------|
| Open Properties | **Ctrl+P** |
| Send to Directory | **Ctrl+T** |
| Close panel | Esc |
| Refresh preview | F5 |

Shortcuts are configurable in future versions (planned for v2.1.x).

---

# 📦 Version History

### **v2.0.3**
- Properties Panel introduced  
- Full metadata display  
- Video preview integration  
- Engine result comparison  
- Right‑click integration  
- Configurable double‑click action  

### **v2.0.1**
- Video preview backend created  
- GPU decoding added  
- Thumbnail extraction implemented  

### **v2.0.0**
- Early metadata extraction  
- Basic preview panel (images only)

---

# 📌 Summary

The Properties Panel is a central part of NSFW Manager’s UX.  
It provides a complete, detailed, and fast view of any scanned file, combining metadata, preview, engine results, and contextual actions in a single interface.

It is one of the most appreciated features introduced in **v2.0.3**, and serves as a foundation for future enhancements such as:
- system tray quick‑view  
- batch properties  
- metadata export  
- advanced engine comparison

---
