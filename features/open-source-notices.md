# Open Source Notices
## Third-Party Components Bundled in NSFW Manager

The **Open Source Notices** dialog lists every third-party component used in NSFW Manager, its licence, and the full licence texts as required by each component's terms. Open it from **Help → Open Source Notices**.

---

## Why This Dialog Exists

NSFW Manager uses open-source libraries. Several of these licences — notably Apache 2.0 — legally require that the full licence text and attribution notice be made accessible to the end user in any distribution. This dialog fulfils that obligation.

---

## Dialog Tabs

### Summary

A table of all third-party components with four columns:

| Column | Content |
|---|---|
| Component | Library name and scope |
| Licence | Human-readable licence name |
| SPDX | Standardised licence identifier |
| Compatibility | Whether the licence is compatible with NSFW Manager's proprietary licence |

**Components included:**

| Component | Licence | Notes |
|---|---|---|
| ONNX Models (model*.onnx) | Apache 2.0 | AI detection models bundled with the app |
| Pillow (PIL) | HPND | Image decoding and processing |
| send2trash | BSD 3-Clause | Recycle Bin integration |
| onnxruntime (Microsoft) | MIT | ONNX inference runtime |
| numpy | BSD 3-Clause | Numerical array operations |
| Python stdlib / tkinter (PSF) | PSF-2.0 | Python interpreter and UI toolkit |
| PyInstaller (build tool) | GPL-2.0+ with bootloader exception | Used to build the EXE; not distributed as a library |
| ifnude (optional, not bundled) | GPL-3.0 | Installed separately by the user; not included in the MSI |
| FFmpeg | LGPL-2.1+ / GPL-2+ | Video frame extraction, used as a subprocess |
| OpenCV (opencv-python) | Apache 2.0 | Primary video frame extraction library |
| pillow-heif (HEIC/AVIF) | BSD 3-Clause | HEIC, HEIF and AVIF image decoding |

### Apache 2.0

The full text of the Apache License, Version 2.0. Required for bundled ONNX models and OpenCV.

### NOTICE (Apache §4d)

The attribution notice required by Apache License 2.0, Section 4(d). This is the file that explicitly credits the original authors of Apache-licensed components.

### MIT / BSD / Other

Full licence texts for all MIT, BSD, and PSF-licensed components (onnxruntime, numpy, Pillow, send2trash, pillow-heif, Python).

### Video Processing

Licence texts for FFmpeg (LGPL-2.1+) and OpenCV (Apache 2.0), which handle video frame extraction.

---

## Licence Files on Disk

All licence texts are also available as files in the installation directory:

```
%LOCALAPPDATA%\NsfwManager\licenses\
├── apache-2.0.txt
├── NOTICE.txt
├── EULA.txt
├── LICENSE-pillow-heif.txt
├── README-LICENSES.txt
└── (other licence files)
```

---

## Legal Disclaimer

The **Legal Notice** link at the top of the dialog opens `nsfwmanager.com/legal-disclaimer.php` for the full online legal disclaimer covering intellectual property, warranties, and liability.

---

## Note on the ifnude Engine

The optional ifnude engine is licensed under GPLv3 and is **not bundled** in the NSFW Manager installer. It is installed independently by the user via pip. If you install ifnude, you are subject to its GPLv3 terms separately from the NSFW Manager licence. See [Detection Engines](../architecture/engine.md) for details.

---

## Related Pages

- [EULA](./eula.md) — the End-User Licence Agreement for NSFW Manager itself
- [Detection Engines](../architecture/engine.md) — which AI engine to use and why
