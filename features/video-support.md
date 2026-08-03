# Video Support
## Frame-Based NSFW Detection for Common Video Formats

NSFW Manager can analyze video files using the same detection engines used for images. Instead of examining every frame (which would be equivalent to scanning thousands of images), it samples a configurable number of frames distributed evenly across the video timeline and applies the AI engine to each one.

Video detection was introduced in v2.0.1.

---

## Supported Video Formats

MP4 (`.mp4`), QuickTime (`.mov`), Matroska (`.mkv`), WebM (`.webm`), AVI (`.avi`)

All five formats are enabled by default. You can disable individual formats in **Configuration → Detection**.

---

## How Video Detection Works

When NSFW Manager scans a video:

1. It extracts a set of frames distributed evenly across the video duration
2. Each frame is analyzed by the active detection engine, exactly like an image
3. The detection score reported for the video is the **highest score** from any individual frame
4. If any frame exceeds the configured threshold, the entire video is flagged

**Why the highest frame score:** A video is as explicit as its most explicit moment. One explicit frame anywhere in the video is enough reason to flag it. Reporting the highest score also helps you assess how severe the content is — a video scoring 0.95 contains more clearly explicit content than one scoring 0.55.

**Why frame sampling instead of full analysis:** A two-hour movie at 30 frames per second contains 216,000 frames — scanning every one would take longer than watching the film. Sampling 10 to 30 frames distributed uniformly across the timeline captures explicit content that lasts for any meaningful duration, while completing the scan in a fraction of the time.

---

## Configuring the Frame Count

The number of frames sampled per video is configurable from 1 to 100. The default is **10 frames**.

**Configuration:** Configuration → Detection → Frames to sample per video

**Why 10 is the default:** For a typical 2-hour video, 10 uniformly distributed frames means one frame roughly every 12 minutes. Any explicit sequence lasting longer than that will be detected. For casual use, this is a good balance between speed and coverage.

**When to increase the frame count:**
- If you suspect videos contain brief explicit moments (a few seconds of explicit content in otherwise clean video)
- For short clips (under 5 minutes) where 10 frames may oversample the same static scene rather than capturing different moments
- When scan accuracy matters more than scan speed
- Recommended range for thorough coverage: 20–30 frames

**When to decrease the frame count:**
- You have a very large collection (hundreds or thousands of videos) and speed is the priority
- The explicit content you are looking for tends to appear throughout a video rather than in brief flashes
- Even 3–5 frames may be sufficient for heavily explicit content that appears consistently

---

## GPU Acceleration for Video

Video decoding is separate from image inference, and it benefits significantly from GPU hardware.

NSFW Manager uses two video decoding backends:

1. **OpenCV** (default) — fast and reliable for most common formats. If OpenCV cannot decode a particular video, it falls back to FFmpeg automatically.
2. **FFmpeg** — more compatible with unusual codecs and edge-case formats. Slower than OpenCV but handles a wider range of files. Can be forced on for all videos via Configuration → Detection → Force FFmpeg for all videos.

**GPU acceleration (d3d11va):** When GPU use is enabled for video (Configuration → Engines → Force CPU for video unchecked), NSFW Manager uses DirectX 11 hardware acceleration for video decoding. This can be 5–10× faster than CPU decoding for common formats like H.264 and H.265.

If the GPU decoder fails for a specific video (unsupported codec, driver issue), NSFW Manager falls back to CPU decoding automatically — you do not need to intervene.

**When to enable GPU for video:** If you scan more than a handful of videos at a time and have a dedicated GPU, GPU decoding will make a noticeable difference. Even a mid-range GPU from the last few years will outperform CPU decoding significantly for video.

**When to keep CPU for video:** On machines with integrated graphics or older discrete GPUs, CPU decoding may be more stable and not significantly slower. The default is CPU to avoid compatibility issues.

---

## Maximum Video File Size

By default, NSFW Manager skips video files larger than **1000 MB** (1 GB). This prevents accidentally spending enormous amounts of time scanning a single large video file.

Set to `0` in Configuration → Detection for no limit.

**Why 1 GB default:** Most consumer video content — phone recordings, screen captures, downloaded clips — fits comfortably under 1 GB. Very large files (Blu-ray rips, raw footage) are often already-known content and may not need screening.

**When to raise the limit:** You are screening long professional video content, raw footage from cameras, or video archives where large files are the norm.

---

## Junk File Exclusion for Videos

When junk file exclusion is enabled, NSFW Manager skips Apple double files (`._<filename>.mov`) and files in iMovie cache directories. These are system-generated temporary files that are not real video content.

---

## Cache Integration

Video scan results are cached the same way image results are. After the first scan of a video, subsequent scans reuse the cached result instantly — the video does not need to be decoded or re-analyzed unless the file changes or the threshold is modified.

See [Scan Cache](./scan-cache.md) for details.

---

## Disabling Video Detection Entirely

If you only need to scan images, you can disable video scanning in Configuration → Detection. This excludes all video formats from scans and removes any video-related overhead.

---

## Related Pages

- [Photo Support](./photo-support.md) — image formats and behavior
- [Scan Cache](./scan-cache.md) — how results are stored and reused
- [Detection Threshold](./detection-threshold.md) — how the threshold applies to video scores
