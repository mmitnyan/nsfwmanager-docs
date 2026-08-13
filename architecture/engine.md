# Detection Engines
## Understanding the AI Models Available in NSFW Manager

NSFW Manager includes multiple AI detection engines, each designed with a different balance of speed, accuracy, and hardware requirements. This page explains what each engine does, when to choose it, and why the differences matter in practice.

---

## How Detection Engines Work

When you scan a file, NSFW Manager passes it to the selected AI engine. The engine analyzes the visual content and returns:

- A **score** between 0.0 and 1.0 representing how confident the AI is that the content is NSFW
- A **label** describing what type of content was detected (for example: suggestive, explicit, or safe)

This score is compared against your configured threshold. Files at or above the threshold appear in your results.

Different engines are trained differently, use different model architectures, and produce different score distributions. Choosing the right engine affects both the quality of detections and how quickly your scans complete.

---

## The Four Engines

### The Just Perfect (int8)

**Technical variant:** int8  
**Hardware required:** CPU or GPU  
**Bundled:** Yes — included with NSFW Manager, no download needed  
**Speed:** Fastest

This is the default engine. It uses 8-bit quantization, which reduces the model's precision slightly in exchange for much faster inference and lower memory usage. For NSFW detection, this trade-off is rarely noticeable in practice — the content either is or isn't explicit, and the reduced precision does not affect that judgment.

**Why choose it:** For most users and most collections, this engine is the right choice. It is fast, runs on any hardware (including older CPUs), and produces reliable results without requiring any additional setup. Start here.

**When to consider another engine:** If you are building a curated archive where borderline cases matter — content that might score 0.45 or 0.55 — the int8 model's slightly reduced sensitivity to ambiguous content may cause it to miss some borderline files that a more precise model would catch.

---

### The Laid-Back One (fp16)

**Technical variant:** fp16  
**Hardware required:** GPU only  
**Bundled:** No — must be downloaded from Configuration → Engines  
**Speed:** Medium

This engine uses 16-bit floating-point precision. It is more precise than the int8 model and produces slightly higher-accuracy results. However, it requires a dedicated GPU — it cannot run on CPU. If GPU use is disabled or no supported GPU is detected, NSFW Manager will warn you and switch to a compatible engine.

**Why choose it:** If you have a dedicated GPU and want slightly better accuracy without the slowness of the onnx model, this is a good middle option. It is faster than onnx on GPU hardware and more accurate than int8.

**When not to use it:** On a machine without a dedicated GPU, this engine is unavailable. Do not select it if you see a "Force CPU" warning in the Configuration → Engines panel.

---

### The Nit Picker (onnx)

**Technical variant:** onnx  
**Hardware required:** CPU or GPU  
**Bundled:** No — must be downloaded from Configuration → Engines  
**Speed:** Slowest

This engine uses full-precision weights and performs the most thorough analysis of the three built-in engines. It catches more borderline content and is less likely to miss a file that a faster model might give a marginal score.

**Why choose it:** For collections where completeness matters — forensic review, compliance audits, building a definitively clean archive. If you are willing to accept a longer scan in exchange for fewer missed detections, this is the right engine.

**When the slower speed is acceptable:** On smaller collections (under a few thousand files), the speed difference between int8 and onnx may be small enough to not matter. On large collections, the difference will be significant.

---

### The Rebel (ifnude)

**Technical variant:** ifnude (external engine)  
**Hardware required:** CPU or GPU  
**Bundled:** No — installed separately via your Python environment (pip install ifnude)  
**License:** GPLv3 (not included with NSFW Manager due to licensing)

This engine comes from an independent open-source project and works differently from the three built-in engines. Rather than returning a single "NSFW score," it returns separate confidence values for specific types of anatomical exposure — for example, exposed chest, exposed buttocks, or explicit nudity — with a label describing each.

**Why the difference matters:** The built-in engines (int8, fp16, onnx) are deliberately binary — they classify content as either NSFW or safe, with scores clustered above 0.85 for clearly explicit content and below 0.20 for clearly safe content. This makes them fast and easy to use but less useful for nuanced decisions.

The Rebel engine produces distributed scores across the full 0.0–1.0 range, making threshold tuning much more meaningful. A user who considers exposed shoulders acceptable but exposed breasts unacceptable can tune the threshold and label filters to reflect exactly that distinction.

**Why it requires separate installation:** The ifnude library is licensed under GPLv3. NSFW Manager uses a commercial licence that is not compatible with bundling GPLv3 code. You can install it alongside NSFW Manager if you choose to use it, but it must be installed independently.

**When to choose it:** When you need fine-grained classification and are willing to install an additional library. Particularly useful for moderation workflows where different types of content require different handling.

---

## Score Distributions: Why Threshold Tuning Works Differently Per Engine

Understanding how each engine distributes scores helps you choose the right threshold:

**The Just Perfect (int8), The Laid-Back One (fp16), The Nit Picker (onnx):** These engines tend to produce high scores (above 0.85) for clearly NSFW content and low scores (below 0.20) for clearly safe content. There are relatively few scores in the middle range. This means threshold adjustments between 0.30 and 0.70 usually produce the same results — the files that are flagged don't change much in that range.

**The Rebel (ifnude):** Scores are distributed across the full range. Many files will score between 0.40 and 0.80. Here, the threshold makes a significant difference — moving it from 0.50 to 0.60 may exclude a meaningful category of content.

See [Detection Threshold](../features/detection-threshold.md) for guidance on tuning.

---

## Automatic Fallback

If the engine you configured at last launch is no longer available when you start NSFW Manager (for example, you deleted the downloaded model file, or the ifnude package was uninstalled), the application automatically switches to the best available engine and shows a warning. You do not need to manually reconfigure.

---

## Downloading Engines

The fp16 and onnx models can be downloaded from **Configuration → Engines**. A progress dialog shows the download percentage, speed, and remaining data. You can cancel the download at any time.

Model files are stored in your user profile. Once downloaded, they are available permanently until you remove them.

---

## Related Pages

- [Detection Threshold](../features/detection-threshold.md) — how to tune sensitivity per engine
- [GPU Acceleration](../troubleshooting/performance.md#gpu) — when GPU helps and when it does not
- [Diagnostics](../ui/ConfigurationPanel.md#diagnostic-tab) — checking which engine and GPU are active
