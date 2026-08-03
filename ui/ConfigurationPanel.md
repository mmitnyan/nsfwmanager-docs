# Configuration Panel
## Centralized Control of Engines, Directories, Appearance, and Behaviour

The **Configuration Panel** is where you control everything about how NSFW Manager behaves. Open it from the Settings button in the toolbar or from the menu. Changes are saved immediately when you click OK or Apply.

The panel is organized into eight tabs. This page documents each tab, every setting, and — importantly — *why* each option exists and when you would want to change it.

---

## Directories Tab

Defines the three key folders that NSFW Manager uses.

**Scan Directory:** The folder NSFW Manager scans by default when you click Scan. You can change this at scan time, but setting a sensible default here avoids picking the folder every session. The scan is always recursive — it includes all subfolders at any depth.

**Quarantine Directory:** Where quarantined files are moved. Should be a local folder on the same drive as your scan directory when possible, to ensure fast moves (rename rather than copy+delete). You can choose any folder in your profile.

**Move Directory:** The destination used by the Move to Folder action. Set this to wherever you want to archive flagged files.

**Default Move Action:** Controls what the main Move button does — either Move to Quarantine or Move to Folder. This is a workflow preference. If you primarily quarantine for review, keep the default. If you have a well-known archive destination and rarely need the review step, set this to Move to Folder.

---

## Engines Tab

Controls which AI engine NSFW Manager uses and GPU/CPU behaviour.

**Active Engine:** Select from the four available engines. The current engine is shown in the toolbar.

**Model Variant:** For the built-in NSFW Manager engines, you can choose which model variant is active (int8, fp16, or onnx). Variants that have not been downloaded yet are shown as unavailable with a download button.

**Download:** Downloads additional model variants directly from the application. A progress dialog shows percentage, download speed, and total size. You can cancel at any time.

**Details:** Shows the detection method, library version, and file path for the selected engine. Useful for verifying which model you are running.

**Force CPU for photos:** When checked (the default), image analysis always runs on the CPU. When unchecked and a compatible GPU is detected, the GPU is used for image inference.

Why it is on by default: CPU inference is stable and predictable on all hardware. GPU inference is faster on modern dedicated GPUs but can cause compatibility issues on some systems, particularly with integrated or older graphics adapters. Enable GPU for images if you have a dedicated NVIDIA or AMD GPU and want faster scans.

Why CPU can be faster for photos: On integrated or low-end GPUs, the overhead of transferring data to the GPU and back can exceed any inference speedup. The GPU is not universally faster. Test on your hardware before committing to GPU mode for images.

**Force CPU for videos:** Same logic, but for video decoding and frame extraction. GPU decoding for video (using DirectX 11 hardware acceleration) is typically more beneficial than GPU inference for images — video decoding is a task GPUs excel at.

---

## Detection Tab

Controls the sensitivity and scope of what gets flagged.

**Detection Threshold:** The confidence score at which a file is considered detected. Files scoring at or above this value appear in your results. The default is 0.50.

See [Detection Threshold](../features/detection-threshold.md) for a full guide on tuning this setting.

**Image Formats:** Checkboxes for each supported format. Disable formats you never encounter to reduce scan time slightly. Note that HEIC/HEIF/AVIF show as unavailable if the optional pillow-heif plugin is not installed.

**Exclude junk files:** Skips system-generated files (Thumbs.db, .DS_Store, desktop.ini, AppleDouble files). On by default. Disable only if you have a specific reason to scan these files.

**Maximum file size (images):** Skip any image file larger than this value. Default is 100 MB. Set to 0 for no limit. See [Photo Support](../features/photo-support.md) for guidance on when to change this.

**Maximum file size (videos):** Skip any video file larger than this value. Default is 1000 MB.

**Frames to sample per video:** How many frames are analyzed per video. Default is 10. See [Video Support](../features/video-support.md) for guidance on this setting.

**Enable video detection:** Toggle to include or exclude video files from all scans.

**Show Corrupted Files section:** When enabled, files that could not be decoded appear in a separate section in the results list. Off by default since most users do not need to see decode errors. Enable it if you are diagnosing why certain files are not showing up in results.

---

## General Tab

Controls file actions and interface behaviour.

**File Deletion Mode:** Choose between Recycle Bin (default) and Permanent Delete. This applies globally to all delete actions. See [Delete](../features/delete.md) for guidance on which to choose.

**Confirm before delete:** Show a confirmation dialog before any deletion. On by default. Turn off for bulk reviewed workflows where you want faster operation.

**Show completion popup:** Show a brief notification after a successful file action. Turn off if you find it distracting during rapid sequential actions.

**Double-click action:** What happens when you double-click a file in the results list. Options are: Open Properties, Move to Default Folder, Move to Directory (with folder picker), and Quarantine.

Choose based on your workflow: if you typically want to review before acting, set it to Open Properties. If you want one-click dispatch to your archive, set it to Move to Default Folder.

---

## Language Tab

Select the UI language: English, French, or Spanish.

The change takes effect after restarting the application. The language selection also controls which localized AI personality portraits are displayed (if Funny mode is enabled in the Theme tab).

---

## Theme Tab

**Light / Dark theme:** Controls the visual theme. Takes effect after a restart.

**AI Personality Style:** Choose between Funny and Professional.

Funny mode displays illustrated character portraits for each engine in the interface, named after the engines' French personality names. This mode reflects the origins of the project and can make the application feel more approachable in personal use.

Professional mode shows no character portraits. Appropriate for corporate or workplace environments where the personality theme could feel out of place.

This setting takes effect immediately when you save.

---

## Cache Tab

Controls the scan cache.

**Enable scan cache:** Turn on persistent result caching. Off by default.

**Use MD5 verification:** Add a content-based integrity check on top of the default size+timestamp check. Off by default. See [Scan Cache](../features/scan-cache.md) for when to enable this.

**Cache Statistics:** Shows the number of cached files, results, and total cache size.

**Clear Cache:** Removes all cached results. Useful when you want a completely fresh re-analysis, or after significant configuration changes.

---

## Diagnostic Tab

Shows hardware and engine status. Useful for troubleshooting.

**ONNX Runtime version:** The version of the AI inference library installed.

**Available ONNX providers:** Which execution backends are available (CPU, DirectML, CUDA). This tells you whether GPU inference is available at all.

**GPU available / GPU in use / GPU type:** Quick status of GPU detection. If GPU is available but not in use, check whether Force CPU is checked in the Engines tab.

**Force CPU checkboxes:** These are duplicated here for convenience. Changing them here has the same effect as changing them in the Engines tab.

Why this tab exists: When filing a bug report or troubleshooting a performance issue, these values immediately answer the most common questions about your environment without requiring you to navigate to multiple settings.

---

## Related Pages

- [Licence Management Panel](./licence-panel.md) — activating and reviewing your licence
- [Detection Engines](../architecture/engine.md) — engine details and selection guide
- [Detection Threshold](../features/detection-threshold.md) — threshold tuning guide
- [Scan Cache](../features/scan-cache.md) — cache configuration guide
- [Delete](../features/delete.md) — delete mode selection
- [Quarantine](../features/quarantine.md) — quarantine behaviour options
