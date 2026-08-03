# Detection Engines  
## Understanding NSFW Manager’s AI Models and Performance Profiles

NSFW Manager includes multiple AI detection engines, each with different performance characteristics, accuracy levels, and hardware requirements. This document explains how each engine works, how fast it scans, and how to choose the right model for your workflow.

---

## 📌 Overview

NSFW Manager provides two categories of engines:

### **1. NSFW Manager Engines (Commercial)**
These engines are built into the application and fully licensed for commercial use.

### **2. Optional Engines (GPLv3)**
These engines are available for users who want additional detection styles but must be installed separately due to GPLv3 licensing.

All engines return:

- a **score** between `0.01` and `1.0`  
- a **reason label** describing the detected content  
- consistent output formats across the UI  

---

# ⚡ NSFW Manager Engines (Commercial)

These engines are ordered from **fastest to slowest**.

## 1. **int8 – Fast – “The Laid‑Back One”**  
**Hardware:** CPU or GPU  
**Speed:** Fastest  
**Accuracy:** Good  
**Best for:** Large batches, quick scans, general detection

This engine uses 8‑bit quantization for maximum speed.  
It is ideal for users who want fast results with minimal resource usage.

---

## 2. **fp16 – Balanced – “The Just Right”**  
**Hardware:** GPU only  
**Speed:** Medium  
**Accuracy:** Higher than int8  
**Best for:** Users with strong GPUs, balanced speed/accuracy

This engine uses half‑precision floating point (FP16).  
It requires GPU acceleration and is unavailable when “Force CPU” is enabled.

---

## 3. **full – Maximum – “The Nit Picker”**  
**Hardware:** CPU or GPU  
**Speed:** Slowest  
**Accuracy:** Highest  
**Best for:** Maximum precision, detailed analysis

This engine uses full‑precision weights and performs the most thorough analysis.  
It is slower but provides the most accurate results.

---

# 🟥 Optional Engines (GPLv3)

These engines must be installed separately due to GPLv3 licensing.  
They are not bundled with NSFW Manager.

## 1. **ifnude – Fast – “The Rebel”**  
**Hardware:** CPU or GPU  
**Speed:** Fast  
**Accuracy:** High for exposed‑body detection  
**Specialization:** Nudity classification with detailed anatomical labels

## 2. **ifnude – Default – “The Rebel”**  
Same model, different configuration.  
Provides more conservative detection thresholds.

---

# 🧠 CPU vs GPU Behavior

Performance varies depending on hardware:

### CPU
- Surprisingly fast for int8 and full models  
- More consistent across different systems  
- Default mode (to avoid GPU‑related support issues)

### GPU
- Required for fp16  
- Faster on modern NVIDIA/AMD cards  
- May be slower on low‑end or integrated GPUs  
- Can outperform CPU depending on model and content

Users can control CPU/GPU behavior in the **Diagnostics** tab:

- **Force CPU for photos** (enabled by default)  
- **Force CPU for videos** (enabled by default)

Disabling these options enables GPU acceleration.

---

# 🏷 Detection Labels

### NSFW Manager Engines (int8 / fp16 / full)
- **Pornography**  
- **Suggestive**  
- **Hentai**  
- **Drawings**  
- **Safe**

These labels are consistent across all commercial engines.

### ifnude (GPLv3)
- Exposed breast (F)  
- Exposed chest (M)  
- Exposed buttocks  
- Exposed genitalia (F)  
- Exposed genitalia (M)

These labels are more anatomically specific.

---

# 🎯 Choosing the Right Engine

### **Fastest scans**
Use **int8**  
Ideal for large folders, quick detection, or repeated scans.  
Produces very binary results: either clearly NSFW or clearly safe.

### **Balanced speed and accuracy**
Use **fp16**  
Requires GPU acceleration.  
More accurate than int8, but still produces mostly high‑confidence NSFW scores.

### **Maximum accuracy**
Use **full**  
Best for detailed analysis or sensitive environments.  
Still binary in behavior: NSFW detections typically appear above 0.85.

### **Most granular classification**
Use **ifnude**  
Requires external installation (GPLv3).  
Provides detailed anatomical labels and a wide score distribution.  
Allows users to set custom thresholds (e.g., 0.63) depending on personal criteria.  
Best for nuanced or borderline cases where user judgment is required.


Among all available engines, **ifnude** provides the most detailed and granular classification. It is capable of distinguishing specific anatomical exposure categories such as exposed chest, exposed breast, exposed buttocks, and exposed genitalia. This level of detail allows users to define their own threshold for what they consider “NSFW.”

For example, many users find that a threshold around **0.63** provides a good balance between sensitivity and accuracy. This flexibility is important because definitions of nudity vary widely: some users consider a visible navel to be inappropriate, while others only classify explicit exposure of genitalia as NSFW. With ifnude, users can fine‑tune the detection threshold to match their personal or organizational criteria.

In contrast, the three built‑in NSFW Manager engines (int8, fp16, full) are intentionally more binary in their behavior. They tend to classify content as either clearly NSFW or clearly safe, with very few borderline scores. Most NSFW detections from these engines appear above **0.85**, and images below that range are typically considered safe. This makes them fast and reliable for general detection, but less suitable for users who need fine‑grained control.

If you require the ability to make nuanced decisions based on subtle differences in content, **ifnude (“The Rebel”)** is the recommended engine.


---

# 📁 Log Locations

Engine initialization and performance events may appear in:
%APPDATA%\Roaming\NsfwManager\logs\NsfwManager.log
%LOCALAPPDATA%\NsfwManager\logs\startup.log
%LOCALAPPDATA%\NsfwManager\logs\execution.log


These logs help diagnose:

- engine loading failures  
- GPU initialization issues  
- performance bottlenecks  
- fallback to CPU mode  

---

# 📌 Summary

NSFW Manager provides multiple engines to match different performance and accuracy needs:

- **int8** → fastest  
- **fp16** → balanced  
- **full** → most accurate  
- **ifnude** → detailed nudity detection (GPLv3)

Users can fine‑tune performance using CPU/GPU settings, file format filters, and cache options.

---

