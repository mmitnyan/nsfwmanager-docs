# Detection Threshold
## Tuning Sensitivity to Match Your Needs

The **detection threshold** is the single most impactful setting in NSFW Manager. It controls how confident the AI must be before a file is considered detected and shown in your results. Understanding it helps you eliminate false positives without missing real content.

---

## What the Detection Score Means

When NSFW Manager analyzes a file, the AI model produces a **confidence score** between `0.0` and `1.0`:

- `0.0` means the model is certain the content is safe
- `1.0` means the model is certain the content is NSFW
- Values in between represent degrees of uncertainty

The threshold is the **cut-off point**: any file with a score at or above the threshold is flagged and shown in your results. Files below the threshold are considered safe and are not listed.

---

## Why 100% Is Not a Useful Threshold

AI models are probabilistic by nature — they never return exactly `1.0` for NSFW content. Even an extremely explicit image will typically score `0.90–0.98`. Setting your threshold to `1.0` would mean nothing ever gets flagged, regardless of content.

Similarly, a score of `0.0` does not mean a file is definitely safe — it means the model found no relevant signals in that particular image.

A threshold of **0.50** is the logical midpoint: it flags any file that the model considers "more likely NSFW than not." This is the application default.

---

## The False Positive / False Negative Trade-Off

Adjusting the threshold always involves a trade-off between two types of errors:

### Lower threshold → more detections
- **Benefit:** catches more actual NSFW content, including borderline cases
- **Cost:** more false positives — legitimate images (swimwear, medical photos, artwork) may appear in results
- A threshold around `0.30–0.40` is appropriate when you want maximum coverage and are willing to review more files

### Higher threshold → fewer detections
- **Benefit:** fewer false positives; only high-confidence NSFW content is flagged
- **Cost:** some real content may be missed if the model is not fully confident
- A threshold of `0.65–0.80` works well when you trust the content is relatively clean and want to surface only obvious material

There is no universally "correct" value. The right threshold depends on what you are scanning and what outcome matters more to you.

---

## Recommended Starting Points by Use Case

| Use case | Suggested threshold | Reasoning |
|---|---|---|
| Family / child safety monitoring | 0.30 – 0.40 | Prioritize catching everything; false positives are acceptable overhead |
| General media library cleanup | 0.50 | Balanced; flags likely NSFW, filters clear safe content |
| Professional content review | 0.60 – 0.70 | Reduce noise in a mostly-clean archive; focus on clear violations |
| Forensic / compliance audit | 0.30 – 0.40 | Comprehensive coverage; every borderline file worth reviewing |

These are starting points. After your first scan, review the results and adjust: if you see too many swimwear or art photos, raise the threshold slightly. If you suspect real content is being missed, lower it.

---

## How Different Engines Interact with the Threshold

The threshold setting applies to all engines, but each engine produces scores with different distributions:

### The Just Perfect (int8), The Laid-Back One (fp16), The Nit Picker (onnx)
These built-in engines tend to be **binary in their scoring**. Most files score either very low (below `0.20`) or very high (above `0.85`). There are few borderline scores in between. This means:

- The threshold matters less in practice — almost any value between `0.30` and `0.80` produces identical results for most content
- The main effect of adjusting the threshold with these engines is whether ambiguous "suggestive" content (scoring `0.40–0.70`) appears in your results or not

### The Rebel (ifnude)
This engine produces a **much wider score distribution**. Files may score anywhere across the `0.0–1.0` range, with many landing in the `0.40–0.75` zone. With this engine:

- Threshold tuning has a much larger impact on your results
- A value around `0.50–0.65` is typically a good starting point
- Users who want to detect partial exposure (swimwear, underwear) will need a lower threshold than those who only want to flag explicit nudity

---

## The Threshold and the Scan Cache

The scan cache stores results **computed at a specific threshold**. If you change the threshold after enabling the cache, cached results may be stale:

- A file that was below the old threshold (not flagged) might now be above the new threshold — but because it was cached as "safe," it won't be re-analyzed automatically
- To ensure accurate results after changing the threshold, either **clear the cache** (Configuration → Cache → Clear Cache) or **rescan your directories**

This is also why the cache includes the threshold value in its validity check — NSFW Manager automatically re-analyzes a file if the threshold has changed since the cached result was recorded.

---

## Setting the Threshold

The threshold is configured in **Configuration → Detection** using a slider. Named levels appear along the slider to give a qualitative sense of what each range means. You can also type a value directly.

The threshold applies globally to all scanned files. There is no per-folder or per-engine threshold override.
