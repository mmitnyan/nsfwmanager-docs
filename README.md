# NSFW Manager — Documentation

NSFW Manager is a Windows desktop application that scans, reviews, and manages files detected as potentially inappropriate content. All processing is local — no files are ever uploaded to external servers.

## Features

- **Content scanning** — detect and flag NSFW images and videos on your local drives using multiple AI engines
- **Multiple AI engines** — choose between The Just Perfect (int8, fast), The Laid-Back One (fp16, GPU), The Nit Picker (onnx, thorough), and The Rebel (ifnude, granular anatomical labels)
- **Detection threshold** — tune sensitivity to your use case; adjust per engine behavior
- **Scan cache** — avoid rescanning unchanged files thanks to a persistent result cache
- **Video support** — frame-sampling detection for MP4, MOV, MKV, WebM, AVI
- **Quarantine** — safely isolate flagged files in timestamped session folders, with one-click restore
- **Move to Folder** — one-way archive to a custom destination
- **Delete** — send to Recycle Bin or permanently remove, with configurable confirmation
- **Properties Panel** — inspect file metadata and detection results before deciding
- **Keyboard shortcuts** — full keyboard-driven workflow for power users
- **Per-user MSI installer** — install without administrator rights

## Documentation Map

| Section | Description |
|---|---|
| [Getting Started](getting-started/) | Installation, first scan, and licence activation |
| [Features](features/) | In-depth guides for every feature including WHY each option exists |
| [Architecture](architecture/) | How NSFW Manager works under the hood (user-facing explanations) |
| [UI Reference](ui/) | Main screen, Properties Panel, and Configuration Panel |
| [Security](security/) | Licence security, privacy, and SSL communication |
| [Troubleshooting](troubleshooting/) | Performance tuning and common errors |
| [Changelog](changelog/) | Release notes for each version |

## Quick Links

- [Installation](getting-started/installation.md)
- [Your First Scan](getting-started/first-scan.md)
- [Licence Activation](getting-started/licence.md)
- [Detection Threshold Guide](features/detection-threshold.md)
- [Choosing an Engine](architecture/engine.md)
- [Latest Release — v2.0.3](changelog/v2.0.3.md)

## Contributing to the Docs

All documentation is written in Markdown. To suggest a change, open a pull request against this repository with your proposed edits.
