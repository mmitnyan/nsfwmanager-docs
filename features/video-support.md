# Video Support  
## Full Multimedia NSFW Detection & Preview

NSFW Manager includes advanced video detection capabilities, allowing the application to analyze, preview, and classify sensitive content inside video files.

Video support was introduced in **[v2.0.1](ca://s?q=Open_v2.0.1_release_notes)** and expanded in **[v2.0.3](ca://s?q=Open_v2.0.3_release_notes)**.

---

## 🎯 Purpose

Video support ensures:
- Accurate NSFW detection inside videos  
- Fast preview with GPU decoding  
- Unified workflow with image detection  
- Full integration with cache, quarantine, and properties panel  

---

# 🎥 Supported Formats

- MP4  
- MOV  
- MKV  
- AVI  
- WEBM  

Unsupported codecs fall back to CPU decoding.

---

# 🚀 Detection Pipeline

## **1. Frame Extraction**
- Extracts frames at configurable intervals  
- Uses ffmpeg backend  
- Generates thumbnails  
- Handles corrupted frames gracefully  

## **2. Engine Inference**
- ONNX models applied to extracted frames  
- Multi-engine support (int8, fp16, full)  
- Score aggregation per video  

## **3. GPU Acceleration (d3d11va)**
- Hardware decoding  
- Automatic fallback to CPU  
- Significant performance boost  

---

# 🖥 Video Preview

The preview panel includes:
- Embedded video player  
- Seek bar  
- Frame thumbnails  
- Async loading  
- Metadata display (codec, duration, resolution)

Related documentation:  
**[Properties Panel](ca://s?q=Open_properties_panel)**

---

# 🧪 Cache Integration

Video detection results and thumbnails are cached:
- Instant re-scan  
- Engine-aware caching  
- Threshold-aware caching  
- MD5 validation (optional)

Related documentation:  
**[Cache System](ca://s?q=Show_SQLite_schema)**

---

# 📦 Version History

### **v2.0.3**
- Cached thumbnails  
- Improved preview stability  
- Right-click actions extended to videos  

### **v2.0.2**
- Stability improvements  
- Documentation added  

### **v2.0.1**
- Video detection engine introduced  
- GPU decoding added  
- Video preview panel created  

---

# 📌 Summary

Video support transforms NSFW Manager into a full multimedia detection tool, capable of analyzing both images and videos with high accuracy and performance.

---
