# Handling False Positives
## When the Engine Flags Content It Shouldn't

A **false positive** is a file that NSFW Manager marks as detected even though you consider its content acceptable. This is a normal part of working with AI detection — no model is perfect. Understanding why false positives happen and how to address them lets you tune the tool for your specific content.

---

## Why False Positives Happen

The AI engines analyse visual features, not meaning. They cannot distinguish between:

- A swimsuit photo on a beach and lingerie in a catalogue
- A medical anatomy illustration and explicit content
- Classical art featuring nudity and modern explicit imagery
- A muscular athlete and flagged body content

When the AI finds visual patterns that statistically correlate with NSFW content, it reports a high score regardless of the context. The **threshold** is then your control over how many of those detections become actual results.

---

## Common Trigger Categories

| Content type | Why it triggers | Typical score range |
|---|---|---|
| Swimwear / beachwear | Skin exposure pattern matches training data | 0.50 – 0.80 |
| Lingerie or underwear ads | Intentional category for some engines | 0.70 – 0.95 |
| Medical / anatomical images | No context awareness in the model | 0.80 – 0.99 |
| Classical art with nudity | High visual match, no art-context awareness | 0.85 – 0.99 |
| Partially obscured figures | Uncertain predictions, varies widely | 0.40 – 0.70 |
| Low-resolution or blurry images | Random high scores on uncertain inputs | 0.50 – 0.90 |

---

## Step 1 — Understand Your Engine's Score Distribution

Different engines distribute scores differently. This affects how you should set your threshold:

**The Just Perfect (int8) and The Laid-Back One (fp16):** Scores cluster strongly at the extremes — either above 0.85 for a clear detection, or below 0.30 for a clear negative. Middle scores are rare. A threshold of 0.85 is a reasonable starting point; you will see few borderline cases.

**The Nit Picker (onnx):** Produces a more distributed range of scores across the 0.0–1.0 spectrum. A higher threshold (0.90 or above) is often needed to reduce false positives without losing true detections.

**The Rebel (ifnude):** Anatomically focused — it detects body parts rather than inferred scene type. It reliably flags exposed anatomy regardless of artistic or medical context. False positives are lower for normal photos but higher for art, sculpture, and medical content.

See [Detection Threshold](./detection-threshold.md) for a full guide on score interpretation and threshold selection.

---

## Step 2 — Raise the Threshold

The most direct fix is to increase the **Detection Threshold** in **Configuration → Detection**.

Raising the threshold by 0.05 to 0.10 steps lets you find the point where true positives are retained but borderline false positives are dropped. Make small adjustments and re-scan a known set of files to observe the effect.

**Trade-off:** A higher threshold reduces false positives but may increase false negatives — real content that scores below the new threshold will no longer appear in results. Balance this based on your use case.

---

## Step 3 — Use Quarantine as a Review Step

Instead of sending results directly to the Recycle Bin or a permanent delete, use **quarantine** for uncertain files:

1. Run a scan
2. Review flagged files in the Properties Panel (thumbnail + score visible)
3. Move anything you are unsure about to quarantine
4. Open the Quarantine Manager and inspect sessions at your own pace
5. Restore files that were false positives; delete confirmed ones

Quarantine is reversible. Permanent deletion is not. When working with a new content type or after changing your threshold, always quarantine first.

---

## Step 4 — Try a Different Engine

If one engine consistently produces too many false positives for your content, switching engines is a valid strategy:

- **int8 / fp16 → onnx:** The Nit Picker produces more intermediate scores and gives you finer threshold control at the cost of slower scans
- **int8 or onnx → ifnude:** For content where explicit anatomy is the specific concern and context (art, medical) is not present, ifnude's anatomical approach may produce fewer false positives
- **ifnude → int8 or onnx:** If ifnude is flagging art or medical images, switching to a scene-classification engine may better match your content

---

## Step 5 — Check Corrupted Files

Files in the **Corrupted** section of the main screen are not detection results — they failed to be decoded or processed at all. These are not false positives; they are a separate category. See [Video Decoding Errors](./video-decoding-errors.md) or verify the file is a valid image.

---

## Related Pages

- [Detection Threshold](../features/detection-threshold.md) — full threshold guide with recommended values
- [Detection Engines](../architecture/engine.md) — choosing the right engine for your content
- [Quarantine](../features/quarantine.md) — how to use quarantine as a review buffer
- [Quarantine Manager](../ui/quarantine-manager.md) — inspecting and restoring quarantined files
