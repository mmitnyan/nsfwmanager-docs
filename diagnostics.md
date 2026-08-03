# Diagnostics
## Understanding Engine Status, GPU, and Cache Information

The **Diagnostic** tab in the Configuration Panel provides a read-only view of your NSFW Manager environment — specifically the AI runtime, GPU availability, and your current engine/CPU settings. This information is useful for troubleshooting performance issues and for providing accurate details when requesting support.

Access it from the Configuration Panel → Diagnostic tab.

---

## Engine and Runtime Information

**ONNX Runtime version:** The version of the AI inference library installed with NSFW Manager. Different versions can affect compatibility with certain GPU drivers or affect which execution providers are available.

**Available providers:** Lists which ONNX execution backends are available on your system. Common values:
- CPUExecutionProvider — always present; CPU inference is always available
- DmlExecutionProvider — DirectML, available on Windows with a compatible DirectX 12 GPU (covers most NVIDIA, AMD, and Intel GPUs from the last several years)
- CUDAExecutionProvider — NVIDIA CUDA; available only if you have installed CUDA drivers separately

**GPU available:** Whether a compatible GPU was detected. If this shows No, GPU acceleration cannot be used for image inference.

**GPU in use:** Whether GPU acceleration is currently active for image inference. This will be No if "Force CPU for photos" is checked in the Engines tab, even if a GPU is available.

**GPU type:** The name of the detected graphics adapter.

---

## Force CPU Toggles

The Diagnostic tab includes duplicates of the Force CPU checkboxes from the Engines tab:

**Force CPU instead of GPU for photos:** When checked, image inference always uses the CPU. When unchecked and a GPU is available, the GPU is used.

**Force CPU instead of GPU for videos:** When checked, video decoding always uses the CPU. When unchecked and a GPU is available, DirectX 11 hardware decoding (d3d11va) is used for video frames.

These are duplicated here so you can quickly toggle them while viewing the diagnostic information, without switching tabs.

---

## When to Check the Diagnostic Tab

**You enabled GPU but scans are not faster:** Check "GPU in use" — if it shows No, the GPU is not being used despite being available. Verify that "Force CPU" is unchecked.

**You are filing a bug report:** Include the ONNX Runtime version, available providers, GPU available/in use, and GPU type in your report. This saves several back-and-forth messages.

**You want to verify your configuration:** After changing CPU/GPU settings, the Diagnostic tab confirms which settings are actually in effect.

---

## Cache Information

The Configuration Panel → Cache tab (not the Diagnostic tab) contains cache-related information: number of cached files, total results stored, and total cache size. See [Scan Cache](features/scan-cache.md) for details.

---

## Logs

Application logs are written to:
`
%APPDATA%\NsfwManager\logs\NsfwManager.log
`

If you experience unexpected behavior, the log file is the first place to check for error messages. You can open the log folder directly from the Help menu.
