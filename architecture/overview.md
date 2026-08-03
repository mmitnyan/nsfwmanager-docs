# Architecture Overview  
## Internal Design of NSFW Manager

NSFW Manager is built as a modular desktop application designed for speed, reliability, and privacy. This document provides a high‑level overview of the internal architecture, including the detection pipeline, engine system, caching layer, UI components, video decoding, licensing, and MSI deployment model.

---

## 📌 Core Principles

NSFW Manager is designed around the following principles:

- **Local processing** — all detection happens on the user’s machine  
- **High performance** — optimized engines, caching, async preview  
- **User privacy** — no images are uploaded or transmitted  
- **Modularity** — engines, cache, UI, and video systems are independent  
- **Stability** — CPU‑first design, GPU optional  
- **Compatibility** — per‑user MSI, no admin rights required  

---

# 🧠 Detection Pipeline

The detection pipeline is the core of NSFW Manager. It processes images and videos using ONNX models and returns a score and label.

### Pipeline Steps

1. **Directory scan**  
   - Collects files based on enabled formats  
   - Applies exclusions (e.g., files starting with `.`)

2. **Cache lookup**  
   - Checks `file_cache` and `engine_results` tables  
   - Validates metadata, threshold, engine, and MD5 (optional)

3. **Engine inference**  
   - Runs the selected engine (int8, fp16, full, ifnude)  
   - Produces score + label  
   - Stores results in SQLite

4. **UI update**  
   - Adds result to the main list  
   - Updates statistics (detected, errors, total, scan time)

5. **Preview generation**  
   - Asynchronous image/video frame loading  
   - Prevents UI blocking

This pipeline is optimized for repeated scans and large directories.

---

# ⚙️ Engine System

NSFW Manager supports multiple engines with different performance and accuracy profiles.

### Built‑in Engines (Commercial)
- **int8** — fastest, binary behavior  
- **fp16** — balanced, GPU‑only  
- **full** — maximum accuracy  

### Optional Engines (GPLv3)
- **ifnude** — granular nudity classification  
- Supports detailed anatomical labels  
- Best for custom thresholds (e.g., 0.63)

### Engine Architecture

- Engines are loaded dynamically  
- CPU/GPU selection controlled by Diagnostics tab  
- Each engine stores results separately in SQLite  
- Switching engines forces re‑scan  
- Threshold stored per engine/model

---

# 🗄 SQLite Cache Layer

The cache layer dramatically accelerates repeated scans.

### Tables

#### `file_cache`
Stores file metadata:
- path  
- mtime  
- size  
- md5 (optional)

#### `engine_results`
Stores engine‑specific results:
- engine_id  
- engine_model  
- score  
- label  
- threshold  
- cached_at  

### Behavior

- First scan: normal speed  
- Subsequent scans: extremely fast  
- MD5 optional for strict validation  
- Cascade delete ensures consistency  

See **[SQLite Schema](ca://s?q=Show_SQLite_schema)** for full details.

---

# 🖼 Image Preview System (Async)

Large images can freeze UI if decoded synchronously.  
NSFW Manager uses an asynchronous preview loader:

- UI updates instantly  
- Decoding happens in background  
- Switching files cancels previous tasks  
- Video frames extracted asynchronously  
- “Hide Image” toggles preview instantly

This system ensures smooth interaction even with 50–80 MB images.

---

# 🎥 Video Decoding Architecture

Video detection uses two decoding backends:

### **OpenCV (default)**
- Fastest  
- Automatically falls back to FFmpeg  
- Ideal for most users

### **FFmpeg (forced mode)**
- More stable  
- Slower  
- Recommended for problematic video libraries

### Video Parameters
- Maximum video size (MB)  
- Frames to sample (1–100)  
- Format filters (mp4, mov, mkv, avi, webm)

Video detection extracts frames and runs them through the engine pipeline.

---

# 🧩 UI Architecture

The UI is divided into several functional modules:

### **Main Screen**
- Directory selection  
- Scan button  
- Results list  
- Preview panel  
- Statistics  
- Action buttons (delete, move, quarantine)

### **Settings Panel**
Tabs include:
- Engines  
- Photo detection  
- Video detection  
- Cache  
- Diagnostics  
- Directories  
- General  
- Theme  

Each tab controls a specific subsystem.

### **Quarantine Manager**
- Session‑based quarantine  
- Restore or permanently delete  
- Preview quarantined files

### **License Panel**
- Email + key validation  
- License retrieval  
- Purchase link  
- Demo mode status

---

# 🔐 Licensing Architecture

Licensing uses a secure client‑server model:

- HMAC‑derived keys  
- Email‑bound licenses  
- Optional machine binding  
- HTTPS validation  
- No offline activation  
- Demo mode with 5 actions per session

The client stores only:
- email  
- key  
- validation status  

No sensitive data is stored locally.

---

# 📦 MSI Deployment Model

NSFW Manager uses a **per‑user MSI**:

- No admin rights required  
- No UAC prompt  
- Silent install supported  
- No Program Files access  
- No HKLM registry writes  
- No privileged CustomActions  
- Start Menu shortcuts stored in `%APPDATA%`

This eliminates Windows Installer errors 1925, 1303, and 1603.

---

# 📁 Logging Architecture

Logs are stored in:
%APPDATA%\Roaming\NsfwManager\logs\NsfwManager.log
%LOCALAPPDATA%\NsfwManager\logs\startup.log
%LOCALAPPDATA%\NsfwManager\logs\execution.log


Logs cover:
- engine initialization  
- cache operations  
- video decoding  
- licensing  
- errors and warnings  

---

# 📌 Summary

NSFW Manager’s architecture is built around:

- modular engine system  
- fast SQLite caching  
- asynchronous preview loading  
- robust video decoding  
- secure licensing  
- per‑user MSI deployment  
- privacy‑focused local processing  

This design ensures high performance, stability, and flexibility across all supported engines and workflows.

---

