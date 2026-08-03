# Asynchronous Image Loading  
## Improving UI Responsiveness When Previewing Large Files

This document explains how NSFW Manager uses asynchronous image loading to prevent UI freezes when previewing large photos or video thumbnails. The feature was introduced to improve responsiveness, especially when dealing with high‑resolution images or slow storage devices.

---

## 📌 Overview

Image previewing can be expensive on Windows systems, especially when:

- opening very large photos (20–80 MB)
- decoding high‑resolution images
- loading thumbnails from videos
- reading files from slow HDDs or network drives

To avoid UI blocking, NSFW Manager uses an **asynchronous loading pipeline** that decodes images in the background while keeping the interface responsive.

---

# ⚡ Why Asynchronous Loading Was Needed

Before async loading, users could experience:

- temporary UI freezes  
- stuttering when selecting files  
- slow preview rendering  
- delayed interaction with the file list  

These issues were caused by synchronous image decoding, which blocked the main UI thread.

Async loading solves this by offloading heavy work to background workers.

---

# 🧩 How Asynchronous Loading Works

When a user selects a file:

1. The UI immediately updates the preview area with a placeholder  
2. A background worker begins decoding the image  
3. The UI remains fully responsive  
4. Once decoding is complete, the preview is updated  
5. If the user switches to another file before decoding finishes, the previous task is discarded

This ensures that:

- the UI never freezes  
- preview updates feel instant  
- large images do not block interaction  
- video thumbnails load smoothly

---

# 🖼 Large Image Handling

Async loading is especially beneficial for:

- DSLR/RAW‑style images  
- high‑resolution JPEG/PNG files  
- HEIC/AVIF images from modern phones  
- large WebP or GIF files  
- video frames extracted during detection

Even when decoding takes time, the interface remains responsive.

---

# 🎥 Video Thumbnail Loading

When previewing a video:

- NSFW Manager displays the frame that triggered the detection score  
- The frame is decoded asynchronously  
- The UI remains responsive while the frame loads  
- If the user clicks “Hide Image,” the preview is replaced immediately

Async loading prevents video frame extraction from blocking the UI.

---

# 🧪 User Experience Improvements

With asynchronous loading:

- Selecting files feels instant  
- Scrolling through results is smoother  
- Previewing large files no longer causes stutters  
- Users can interact with the interface while images decode  
- The application feels faster and more modern

This significantly improves usability on systems with slower CPUs or HDDs.

---

# 📁 Log Locations

Async loading events may appear in:
%APPDATA%\Roaming\NsfwManager\logs\NsfwManager.log
%LOCALAPPDATA%\NsfwManager\logs\startup.log
%LOCALAPPDATA%\NsfwManager\logs\execution.log


These logs help diagnose:

- image decoding errors  
- unsupported formats  
- slow file access  
- video frame extraction issues  

---

# 📌 Summary

Asynchronous loading ensures that NSFW Manager remains responsive even when previewing large or complex media files. By decoding images and video frames in the background, the application avoids UI freezes and provides a smoother, more reliable user experience.

---

