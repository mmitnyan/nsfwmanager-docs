# Video Decoding Errors
## When Videos Appear in the Corrupted Section or Fail to Scan

When NSFW Manager cannot extract frames from a video, the file is placed in the **Corrupted** section of the main screen with an error message. This page explains the most common causes and how to resolve them.

---

## How Video Processing Works

NSFW Manager extracts a set of sample frames from each video and sends those frames to the AI engine. The extraction uses one of two backends:

- **OpenCV** — the primary backend; fast, native decoding, no external process
- **FFmpeg** — the fallback (and required backend for several codecs); invoked as an external process bundled with the application

The backend is chosen automatically based on the video's codec, bit depth, and frame-rate type. If OpenCV fails at any point during extraction, NSFW Manager automatically retries with FFmpeg before reporting an error.

---

## Automatic Codec Routing

| Codec | Primary backend | Notes |
|---|---|---|
| H.264, MPEG-4, MJPEG, MPEG-1/2 | OpenCV | Reliably decoded by OpenCV |
| H.265 / HEVC | OpenCV with FFmpeg fallback | HEVC support varies by OpenCV build |
| VP9, AV1, VP8, ProRes, Theora, DNxHD | FFmpeg only | OpenCV cannot decode these |
| 10-bit or higher bit depth | FFmpeg only | OpenCV loses precision above 8-bit |
| Variable frame rate (VFR) | FFmpeg only | OpenCV seek errors on VFR timelines |

If you see an error for a codec in the "OpenCV with fallback" category, the automatic fallback to FFmpeg should have handled it. If it still fails, see the sections below.

---

## Common Error Scenarios

### "Frame extraction failed"

The extraction process completed but produced zero usable frames. Possible causes:

- The video has no readable duration (missing or corrupt container metadata). FFmpeg uses `ffprobe` to read duration; if the duration is zero or undetectable, frame positions cannot be calculated.
- The file is truncated or partially downloaded.
- The container format is valid but contains no video stream (audio-only file with a video extension).

**What to try:**
- Verify the file plays fully in another player (VLC, Windows Media Player)
- Re-download or re-copy the file if it may be incomplete
- Enable **Force FFmpeg** in Configuration → Engines if not already set, since FFmpeg is more tolerant of broken containers than OpenCV

### "Unexpected error"

An unhandled exception occurred during extraction. This can be caused by:

- A codec variant that is neither OpenCV-safe nor in the FFmpeg-only list (codec name unknown to the routing table)
- A file that triggers a bug in the decoding library for that specific container/codec combination
- Memory pressure during extraction of very large files

**What to try:**
- Enable **Force FFmpeg** in Configuration → Engines — FFmpeg handles a wider range of codec variants
- Check the log file (Configuration → Diagnostic → Open Log) for the detailed error message

### GPU Decode Failure (FFmpeg d3d11va)

When FFmpeg is used, NSFW Manager first attempts GPU-accelerated decoding via **Direct3D 11 Video Acceleration (d3d11va)**. If GPU decoding fails, it automatically retries on the CPU. This fallback is silent and transparent.

If you observe that video processing is unexpectedly slow for many files, or see GPU-related errors in the diagnostic log, this may indicate that the GPU path is consistently failing and the CPU fallback is being used for every file. Enabling **Force CPU** in Configuration → Engines disables the GPU attempt entirely and uses CPU from the start, which avoids the retry overhead.

### Files With No Duration

FFmpeg needs to know the total duration to calculate which timestamps to sample. If `ffprobe` cannot determine the duration (for example, in live-capture recordings where the duration was not written to the container), NSFW Manager will report an error rather than attempt to scan an unbounded number of frames.

**What to try:**
- Re-mux the file using FFmpeg to fix the container: `ffmpeg -i input.mp4 -c copy output.mp4`
- This rewrites the container metadata, which usually adds the correct duration

---

## Force FFmpeg Option

The **Force FFmpeg** checkbox in Configuration → Engines bypasses the automatic codec routing and always uses FFmpeg for every video, skipping the OpenCV attempt entirely.

**When to enable it:**
- A specific file or codec consistently fails with OpenCV even with the automatic fallback
- You are processing a large library of H.265/HEVC files where the OpenCV-then-fallback cycle adds unnecessary overhead
- You want deterministic behavior (same backend every time) for auditing purposes

**Trade-off:** FFmpeg processing is slightly slower than native OpenCV for common codecs (H.264, MPEG-4). Enable Force FFmpeg only when needed, not globally.

---

## Files That Are Truly Unreadable

Some files will appear in the Corrupted section regardless of backend or settings:

- Zero-byte files
- Files with a video extension but non-video content
- Encrypted or DRM-protected media
- Severely corrupted files where the container structure is gone

These cannot be recovered through configuration changes. They are expected in any real-world file collection.

---

## Related Pages

- [Video Support](../features/video-support.md) — supported video formats, frame sampling, and score logic
- [Detection Engines](../architecture/engine.md) — AI engine selection
- [Main Screen](../ui/main-screen.md) — the Corrupted section in context
