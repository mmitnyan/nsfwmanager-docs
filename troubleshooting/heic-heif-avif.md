# HEIC, HEIF, and AVIF Support
## Scanning Apple and Modern Format Photos

HEIC, HEIF, and AVIF are modern image formats used by Apple devices (iPhone, iPad, Mac) and increasingly by other cameras and editing tools. NSFW Manager supports these formats through an optional plugin called **pillow-heif**.

---

## Installer vs. Source Installation

### MSI Installer (recommended)

When you install NSFW Manager using the official MSI installer, **pillow-heif is already bundled**. No additional installation is required.

To enable scanning of HEIC/HEIF/AVIF files:

1. Open **Configuration → Directories** (or the first-run Configuration dialog)
2. In the **Image Formats** section, ensure `.heic`, `.heif`, and `.avif` are checked
3. Click **OK** — NSFW Manager will now include these formats in scans

If these extensions are unchecked, the files are silently skipped even though the plugin is available.

### Running from Source

If you are running NSFW Manager directly from its Python source (development mode, not the installer), pillow-heif must be installed manually:

```
pip install pillow-heif
```

After installation, restart NSFW Manager. The formats `.heic`, `.heif`, and `.avif` will become available for selection in the Configuration dialog.

If pillow-heif is not installed in source mode, NSFW Manager automatically skips these formats without error — no files are processed and no warning is shown in the results. Check the Diagnostic tab (Configuration → Diagnostic) if you are unsure whether the plugin loaded successfully.

---

## Common Issues

### HEIC files appear in the Corrupted section

This usually means the file is genuinely unreadable, not a plugin issue. HEIC files from iOS 16+ may use HEVC-based encoding variants that require platform-level codecs. Verify the file opens in another application (e.g., Windows Photos with the HEVC extension installed).

### Files are simply not appearing in results at all

Check two things:

1. **Format is enabled:** Configuration → Directories → Image Formats — `.heic` must be ticked
2. **Max file size:** If the file is larger than the configured maximum (default 100 MB), it is skipped silently. Raw HEIC from high-resolution sensors can exceed this limit.

### AVIF files from cameras or browsers not scanning

AVIF files from some encoders use a sequence container (multiple images). NSFW Manager processes only the first frame of any multi-image container, which is the cover/primary image. This is correct behaviour.

---

## Format Notes

| Format | Common source | Notes |
|---|---|---|
| `.heic` | iPhone, iPad photos | Most common Apple format since iOS 11 |
| `.heif` | Various cameras, Apple | Container variant; functionally equivalent to HEIC |
| `.avif` | Web images, editing tools | AV1-based; growing adoption in browsers and apps |

AVIF is a separate format from HEIC/HEIF despite using similar container concepts. Both are supported through the same pillow-heif plugin.

---

## Related Pages

- [Photo Support](../features/photo-support.md) — full list of supported image formats and size settings
- [Configuration Panel](../ui/ConfigurationPanel.md) — where to enable/disable image formats
