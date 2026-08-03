# Configuration Panel  
## Centralized Control of Engines, Thresholds, Directories & Application Behavior

The **Configuration Panel** is the main interface for adjusting how NSFW Manager behaves.  
It provides full control over detection engines, thresholds, directories, appearance, quarantine rules, video settings, and licence management.

Introduced in **[v2.0.0](ca://s?q=Open_v2.0.0_release_notes)** and expanded in **[v2.0.3](ca://s?q=Open_v2.0.3_release_notes)**, it is one of the most important components of the application.

---

## 🎯 Purpose

The Configuration Panel allows users to:

- Select detection engines  
- Adjust sensitivity thresholds  
- Configure directories  
- Enable GPU acceleration  
- Customize quarantine behavior  
- Change theme and language  
- Configure video scanning  
- Manage licence keys  
- Access diagnostics  

It is designed to be intuitive, responsive, and fully integrated with the rest of the application.

---

# 🧩 Panel Structure

The Configuration Panel is divided into multiple tabs:

---

## 1. **Detection Settings**

Controls how NSFW Manager analyzes files.

### Options:
- Engine selection (int8, fp16, full, ifnude)  
- Detection threshold slider  
- GPU/CPU fallback toggle  
- MD5 strict mode (v2.0.3+)  
- Cache behavior (instant reuse, validation)

Related documentation:  
**[Engines](ca://s?q=Show_engine_documentation)**  
**[Diagnostics](ca://s?q=Open_diagnostics_panel)**

---

## 2. **Directory Settings**

Defines where NSFW Manager scans and moves files.

### Options:
- Scan directories list  
- Exclusion list  
- Default “Send to Directory” target  
- Default quarantine directory  
- Restore behavior (preserve structure)

Related documentation:  
**[Quarantine](ca://s?q=Open_quarantine_feature)**  
**[Delete](ca://s?q=Open_delete_feature)**

---

## 3. **Appearance**

Controls the look and feel of the application.

### Options:
- Light/dark theme  
- Localized avatars (EN/FR/ES)  
- UI language  
- Icon pack (v2.1.x planned)

Related documentation:  
**[Main Screen](ca://s?q=Open_main_screen)**

---

## 4. **Video Settings** (v2.0.1+)

Controls how videos are scanned and previewed.

### Options:
- Frame sampling interval  
- Maximum video size  
- GPU decoding toggle (d3d11va)  
- CPU fallback  
- Thumbnail extraction mode  

Related documentation:  
**[Video Support](ca://s?q=Open_video_support)**

---

## 5. **Quarantine Settings**

Controls how quarantined files are organized.

### Options:
- Organize by category  
- Preserve folder structure  
- Create log file  
- Double‑click action (v2.0.3+)  
- Session grouping (v2.1.x planned)

Related documentation:  
**[Quarantine](ca://s?q=Open_quarantine_feature)**

---

## 6. **Licence Panel**

Manages licence keys and activation.

### Options:
- Enter licence key  
- Validate key  
- View licence status  
- Trial/free/paid mode  
- Error messages and troubleshooting  

Related documentation:  
**[Licence System](ca://s?q=Open_licence_system)**

---

## 7. **Diagnostics**

Provides system, engine, and cache health information.

### Includes:
- Engine status  
- GPU availability  
- Cache integrity  
- Logs  
- MD5 validation  
- Cache rebuild tools  

Related documentation:  
**[Diagnostics](ca://s?q=Open_diagnostics_panel)**

---

# ⚙️ Behavior & Persistence

Settings are stored in:
%APPDATA%/NsfwManager/config.json


### Behavior:
- Changes apply instantly  
- No restart required  
- Invalid values are sanitized  
- Missing config files are auto‑generated  
- Backups created during updates  

---

# 🧪 Integration with Other Components

The Configuration Panel interacts with:

- **[Main Screen](ca://s?q=Open_main_screen)**  
- **[Properties Panel](ca://s?q=Open_properties_panel)**  
- **[Quarantine Manager](ca://s?q=Open_quarantine_feature)**  
- **[SQLite Cache](ca://s?q=Show_SQLite_schema)**  
- **[Video Support](ca://s?q=Open_video_support)**  
- **[Diagnostics](ca://s?q=Open_diagnostics_panel)**  

This ensures consistent behavior across the entire application.

---

# 📦 Version History

### **v2.0.3**
- Double‑click action  
- Localized avatars  
- MD5 strict mode  
- Better theme consistency  
- Improved directory validation  

### **v2.0.2**
- Documentation added  
- UI polish  

### **v2.0.1**
- Video settings tab added  

### **v2.0.0**
- Full Configuration Panel introduced  
- Multi‑tab architecture  
- Engine selection  
- Directory management  
- Quarantine settings  
- Licence panel  

---

# 📌 Summary

The Configuration Panel is the control center of NSFW Manager.  
It provides full customization of detection engines, directories, appearance, quarantine behavior, video scanning, and licence management — making the application flexible, powerful, and user‑friendly.

---

