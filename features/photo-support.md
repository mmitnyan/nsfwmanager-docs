# Photo Support  
## Complete Image Support Specification  
### NSFW Manager — Formats, Limits, Scaling, Metadata & Engine Behavior

NSFW Manager includes a fully‑featured image detection pipeline capable of handling a wide range of formats, sizes, resolutions, metadata structures, and scaling rules.  
This document describes **every supported format**, **every size limit**, **every scaling rule**, **every fallback mechanism**, and **every advanced behavior** of the photo subsystem.

---

# 📁 Supported Image Formats (Complete List)

NSFW Manager supports **all major image formats**, plus several niche or legacy formats.

## Fully Supported Formats
- JPEG / JPG  
- PNG  
- BMP  
- GIF (first frame only)  
- WEBP  
- TIFF (single page)  
- ICO  
- HEIC  
- HEIF  
- AVIF  
- PPM / PGM / PBM  
- TGA  
- DDS (DirectDraw Surface)  
- PSD (flattened preview)  
- EXR (OpenEXR, flattened)  
- HDR (Radiance HDR)

## Partially Supported Formats
- TIFF multi‑page → first page only  
- GIF animated → first frame only  
- PSD multi-layer → flattened preview only  
- EXR multi-layer → flattened preview only  

## Unsupported Formats (Clear Error Message)
- RAW camera formats (CR2, NEF, ARW, RAF, ORF, DNG)  
- SVG (vector)  
- EPS (PostScript)  
- PDF (not an image format)

---

# 🖼 Maximum File Size & Resolution Handling

NSFW Manager includes a robust scaling system to handle extremely large images.

## Default Maximum File Size
- **200 MB** per image  
- Configurable in Settings → Detection → Max Image Size  
- **0 = illimité** (no limit)

## Default Maximum Resolution
- **32,768 × 32,768 px**  
- Configurable  
- **0 = illimité**



# 🧪 Engine Compatibility

All image formats are processed through the ONNX pipeline.

## Supported Engines
- int8 (fastest)  
- fp16 (GPU accelerated)  
- full (highest accuracy)  
- ifnude (external engine)

## Fallback Logic
If GPU unavailable:
- fp16 → always GPU  
- full → CPU fallback  
- int8 → CPU fallback 

If image cannot be decoded:
- Clear error message  
- No crash  
- Cache entry not created  

---

# 🧩 Metadata Extraction

NSFW Manager extracts:
- Dimensions  
- File size  
- Creation timestamp  
- Modification timestamp  
- EXIF orientation  
- EXIF camera metadata  
- Color profile (sRGB, AdobeRGB, DisplayP3)

Metadata is displayed in the **Properties Panel**  
→ **[Open Properties Panel](ca://s?q=Open_properties_panel)**

---

# 🧱 Scaling Configuration (Advanced)

Located in:  
Settings → Detection → Image Scaling  
→ **[Open Configuration Panel](ca://s?q=Open_configuration_panel)**

## Options
- Max resolution (default: 32768 px)  
- Max file size (default: 200 MB)  
- Scaling mode  
  - Lanczos  
  - Bicubic  
  - Nearest (fastest)  
- Memory cap  
  - Default: 512 MB per image  
  - **0 = illimité**

## Behavior
- Scaling applies before engine inference  
- Scaling does NOT affect cached thumbnails  
- Scaling does NOT modify original file  
- Scaling does NOT affect quarantine behavior  

---

# 🧩 Cache Integration

Image detection results are stored in SQLite:

## Cached Data
- Thumbnail  
- Engine results  
- MD5 hash  
- Timestamp  
- Resolution  
- File size  

## Cache Behavior
- Instant re-scan  
- Threshold-aware  
- Engine-aware  
- MD5 strict mode (optional)

Related documentation:  
→ **[Show SQLite Schema](ca://s?q=Show_SQLite_schema)**

---

# 🗂 Quarantine & Delete Integration

Image support integrates with:
- Quarantine  
- Restore  
- Delete  
- Permanent Delete  
- Send to Directory  

Related documentation:  
→ **[Open Quarantine Feature](ca://s?q=Open_quarantine_feature)**  
→ **[Open Delete Feature](ca://s?q=Open_delete_feature)**

---

# 📦 Version History

### v2.0.3
- MD5 strict mode  
- Faster preview  
- Better scaling stability  
- Improved cache validation  

### v2.0.2
- UI polish  
- Documentation added  

### v2.0.1
- Unified preview for images & videos  

### v2.0.0
- Multi-engine support  
- New preview panel  
- Threshold slider  
- Scaling configuration  

### v1.0.0
- Initial image detection  
- Basic preview panel  

---

# 📌 Summary

NSFW Manager provides one of the most complete image detection pipelines available:

- Huge format support  
- High-resolution handling  
- Smart scaling  
- GPU acceleration  
- Full metadata extraction  
- Instant caching  
- Quarantine integration  
- Configurable limits  
- **0 = illimité** everywhere  

This is the **definitive** specification for image support in NSFW Manager.

---
