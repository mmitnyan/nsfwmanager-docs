# Photo Support
## Supported Formats, Size Limits, and Detection Behavior

NSFW Manager scans a broad range of image formats. This page covers every supported format, the file-size limit setting, the junk-file exclusion filter, and the specific behaviors for animated and Apple-format images.

---

## Supported Image Formats

NSFW Manager supports the following formats:

**Standard formats**
JPEG (`.jpg`, `.jpeg`), PNG (`.png`), BMP (`.bmp`), GIF (`.gif`), TIFF (`.tif`, `.tiff`), WebP (`.webp`), ICO (`.ico`)

**Advanced and raw-style formats**
HEIC (`.heic`), HEIF (`.heif`), AVIF (`.avif`), DDS (`.dds`)

**Specialist formats**
JPEG 2000 (`.jp2`, `.j2k`, `.jpf`, `.jpx`), portable bitmap family (`.ppm`, `.pgm`, `.pbm`)

All enabled formats are shown as checkboxes in **Configuration → Detection**. You can disable individual formats to skip file types you do not need to scan — for example, disabling `.ico` if you are not scanning application directories.

---

## Animated Images (GIF, WebP)

For animated GIF and animated WebP files, NSFW Manager analyzes only the **first frame**. Subsequent frames are not examined.

**Why:** Analyzing every frame of an animated image is equivalent to treating it as a video. Frame-by-frame analysis of animation would add significant scan time with little practical benefit, since the first frame is typically the most representative. If you need per-frame analysis of animated content, enabling video scanning covers common video formats, and video scanning uses configurable frame sampling.

---

## HEIC, HEIF, and AVIF (Apple Device Formats)

HEIC, HEIF, and AVIF are modern compressed image formats used by iPhones, iPads, and recent Android cameras.

**MSI installer:** The required plugin (`pillow-heif`) is bundled with the installer. No additional installation is needed. Enable `.heic`, `.heif`, and `.avif` in **Configuration → Directories → Image Formats** and they will be scanned immediately.

**Running from source:** `pillow-heif` must be installed manually (`pip install pillow-heif`). If it is absent, these formats are skipped silently — files are not scanned and no error is reported.

See [HEIC, HEIF, and AVIF Support](../troubleshooting/heic-heif-avif.md) for common issues with these formats.

---

## Maximum File Size

By default, NSFW Manager skips any image file larger than **100 MB**. Files above this limit are not scanned and do not appear in results.

**Configuration:** Configuration → Detection → Maximum file size to scan. Set to `0` to remove the limit entirely.

**Why the default is 100 MB:**
Photos above 100 MB are uncommon outside of specialized professional workflows — high-end DSLR RAW exports, layered composites, or panoramic stitches. These files take disproportionately long to load and decode, and they are rarely the content you are trying to screen. Capping at 100 MB keeps scan times predictable for typical collections.

**Why you might raise or remove the limit:**
- You are a photographer scanning a RAW archive where individual files regularly exceed 100 MB
- You are working with a professional media library and need complete coverage regardless of file size
- Set the limit to `0` for no restriction, or to a higher value (e.g., 500 MB) for a practical upper bound

**Why you might lower the limit:**
- Scanning a phone sync folder where photos are typically under 10 MB — a 20 MB limit would catch all real photos while skipping any accidentally included large files

---

## Junk File Exclusion

NSFW Manager includes a **junk file exclusion** filter that is enabled by default. It automatically skips the following system-generated files:

- `Thumbs.db` and `ehthumbs.db` — Windows thumbnail caches
- `.DS_Store` — macOS folder metadata files
- `desktop.ini` — Windows folder configuration files
- Apple double files (`._<filename>`) — macOS resource forks written to non-macOS drives

**Why it is on by default:** These files are created automatically by operating systems and are never photos. Scanning them wastes processing time and can produce false detection errors since their binary content is not image data. Most users scanning mixed-OS network shares or drives that have been used with macOS will encounter these files frequently.

**When to disable it:** You have a specific reason to scan these files. This is extremely uncommon in practice.

---

## What Happens When a File Cannot Be Decoded

If a file matches a supported extension but cannot be opened or decoded (corrupted file, truncated download, unrecognized variant), it is recorded as a **corrupted file** rather than a detection or a safe file. A "Corrupted files" section appears in the main results list, hidden by default.

You can make this section visible in **Configuration → Detection → Show Corrupted Files section**. This is useful when diagnosing why certain files are not appearing in detection results — if they show up as corrupted, the issue is with the files themselves rather than with NSFW Manager.

---

## Detection in the Properties Panel

For any scanned image, the [Properties Panel](../ui/properties-panel.md) shows the file's detection score, the label assigned by the active engine, the threshold that was active at scan time, and whether the result came from cache or a live scan. You can also see the image dimensions and file size there.


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
→ **Show SQLite Schema**

---

# 🗂 Quarantine & Delete Integration

Image support integrates with:
- Quarantine  
- Restore  
- Delete  
- Permanent Delete  
- Send to Directory  

Related documentation:  
→ **Open Quarantine Feature**  
→ **Open Delete Feature**

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
