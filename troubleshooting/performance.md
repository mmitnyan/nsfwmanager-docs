# Performance Troubleshooting
## Improving Scan Speed and Engine Responsiveness

This guide explains what factors affect NSFW Manager's scan speed and how to tune the application for your hardware and workflow.

---

## Engine Selection

The biggest single factor in scan speed is the choice of engine. In order from fastest to slowest:

1. **The Just Perfect (int8)** — fastest; runs on CPU or GPU; bundled by default
2. **The Laid-Back One (fp16)** — medium speed; requires a dedicated GPU
3. **The Nit Picker (onnx)** — slowest; most thorough; runs on CPU or GPU
4. **The Rebel (ifnude)** — comparable to int8 in speed; requires separate installation

For large collections where speed matters most, use The Just Perfect (int8). It is accurate enough for the vast majority of use cases.

---

## CPU vs GPU for Image Analysis

Both Force CPU toggles are **on by default**. GPU is not always faster for image analysis — it depends on your hardware.

**GPU helps when:**
- You have a dedicated NVIDIA or AMD GPU from the last few years
- You are scanning many large images in sequence
- The GPU supports DirectML or CUDA

**GPU may not help (or may be slower) when:**
- You have an integrated GPU (Intel UHD, AMD Radeon integrated) — the overhead of passing data to the GPU may exceed the inference speedup
- You are using an older GPU with limited compute capability
- Your GPU is under thermal throttling (laptop with cooling limitations)

**Recommendation:** Test both settings on your machine. Enable GPU (uncheck Force CPU for photos in Configuration → Engines), run a scan of a known folder, note the time, then re-enable Force CPU and compare. Use whichever is faster.

To enable GPU for image analysis: Configuration → Engines → uncheck **Force CPU instead of GPU for photos**. The Diagnostic tab shows whether the GPU is actually being used.

---

## GPU for Video Decoding

GPU acceleration for video (d3d11va hardware decoding) tends to provide a larger speedup than GPU inference for images. Video decoding — converting compressed video to raw frames — is a task that GPU hardware decoders are specifically designed for and typically 5–10× faster than CPU decoding.

To enable: Configuration → Engines → uncheck **Force CPU instead of GPU for videos**.

**Force FFmpeg option:** Configuration → Detection includes an option to force FFmpeg for all videos instead of defaulting to OpenCV. FFmpeg is more compatible with unusual codecs but noticeably slower. Only enable this if you are having decoding failures with specific video files. OpenCV with automatic fallback to FFmpeg is the right setting for most users.

---

## Scan Cache

The most dramatic performance improvement for repeated scans is enabling the scan cache. After the first full scan, every subsequent scan of the same folder skips already-analyzed files and completes in a fraction of the time.

Enable it in Configuration → Cache → Enable scan cache.

See [Scan Cache](../features/scan-cache.md) for details and caveats.

---

## Image File Size Limit

Large image files (professional RAW exports, panoramic stitches) take significantly longer to decode than normal photos. If your collection includes very large files you do not need to screen, raise the maximum file size limit (Configuration → Detection → Maximum file size to scan).

Alternatively, lower the limit to skip very large files entirely if they are not relevant to your use case. Default is 100 MB.

---

## Video Frame Count

More frames per video = slower scan. The default is 10 frames. If you are scanning a large collection of videos and speed is the priority, reducing to 5 frames may be acceptable depending on your content.

Configuration → Detection → Frames to sample per video.

---

## Disabling Unneeded Formats

If you only need to scan JPEG and PNG files, disabling all other formats in Configuration → Detection removes a small amount of overhead per file (format matching). The effect is minor but measurable on very large collections.

---

## Directory Location

Scanning files on a slow HDD is significantly slower than scanning from an SSD, especially for preview loading. If performance is critical and you are scanning from a mechanical drive, consider that the bottleneck may be storage I/O rather than CPU or GPU compute.

Network drives (NAS, SMB shares) add latency on top of I/O overhead. GPU and cache optimizations help less when the bottleneck is network throughput.
