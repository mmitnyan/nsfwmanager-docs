# Privacy & Local Processing  
## How NSFW Manager Protects Your Data

NSFW Manager is designed with a strict privacy-first philosophy:  
**your files never leave your computer.**

This document explains how NSFW Manager handles data, what is processed locally, and how privacy is enforced across all features.

---

## 🔒 Core Principles

### **1. 100% Local Processing**
All detection happens locally:
- Images  
- Videos  
- Thumbnails  
- Metadata  
- Engine inference  
- Cache storage  

No file content is ever uploaded, transmitted, or shared.

### **2. No Telemetry**
NSFW Manager does **not** collect:
- Usage analytics  
- Crash reports  
- File names  
- File paths  
- Detection results  
- Hardware information  

### **3. No Cloud Dependencies**
The application does not rely on:
- Remote AI models  
- Cloud inference  
- Online scanning  
- External storage  

Everything runs offline.

---

# 🧩 What NSFW Manager Processes

### **Images & Videos**
Processed locally using ONNX engines.  
Frames and thumbnails are extracted locally.

### **Metadata**
Only basic metadata is read:
- Dimensions  
- Duration  
- File size  
- Timestamps  

Metadata is never transmitted.

### **Cache**
The SQLite cache stores:
- File hash (MD5 optional)  
- Engine results  
- Thumbnail  
- Timestamp  

Cache is stored in:
%APPDATA%/NsfwManager/scan_cache.db


Related documentation:  
**[Cache System](ca://s?q=Show_SQLite_schema)**

---

# 🔐 Licence Validation (Privacy-Safe)

Licence validation is the **only** network request NSFW Manager performs.

It sends:
- Licence key  
- Machine identifier (hashed)  
- Version number  

It does **not** send:
- File names  
- File content  
- Detection results  
- Folder structure  
- User identity  

Related documentation:  
**[Licence System](ca://s?q=Open_licence_system)**  
**[SSL Security](ca://s?q=Open_SSL_security)**

---

# 🛡 Quarantine Privacy

Quarantined files remain local:
- Stored in `%APPDATA%/NsfwManager/Quarantine/`  
- Never uploaded  
- Never transmitted  
- Never shared  

Related documentation:  
**[Quarantine](ca://s?q=Open_quarantine_feature)**

---

# 🧪 Diagnostics Privacy

Diagnostics show:
- Engine status  
- GPU availability  
- Cache integrity  

Diagnostics do **not** send any data externally.

Related documentation:  
**[Diagnostics](ca://s?q=Open_diagnostics_panel)**

---

# 📦 Version History

### **v2.0.3**
- Licence hardening  
- MD5 strict mode  
- Improved cache privacy  

### **v2.0.2**
- Public documentation added  

### **v2.0.0**
- Privacy-first architecture introduced  

### **v1.0.0**
- Initial local-only processing  

---

# 📌 Summary

NSFW Manager is built around one promise:  
**your sensitive files stay on your machine, always.**

No telemetry, no cloud scanning, no external storage — just fast, private, local detection.

---
