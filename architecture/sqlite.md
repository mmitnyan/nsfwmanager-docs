# The Scan Cache Explained
## What Gets Stored and Why It Helps

This page explains the scan cache from a behavioral perspective — what NSFW Manager remembers about your files, how long it keeps that information, and what triggers a re-analysis. For configuration options, see [Scan Cache](../features/scan-cache.md).

---

## What the Cache Stores

When the scan cache is enabled and NSFW Manager analyzes a file, it stores a record of:

- The file's location (path), size, and last modification time
- Which engine and model variant was used
- What threshold was active
- The detection score and label returned by the engine
- Optionally, an MD5 fingerprint of the file content (if MD5 verification is enabled)

This record is kept on your local machine in a database file in your user profile. It is never transmitted anywhere.

---

## How the Cache Makes Subsequent Scans Faster

On the second and later scans of the same folder, NSFW Manager compares each file against its stored record. If the file's size, modification time, engine, model, and threshold all match the stored record, the analysis is skipped and the stored result is used directly.

For a folder of 10,000 photos where 200 were added since the last scan:
- 9,800 files: result retrieved from cache instantly
- 200 new files: analyzed by the AI engine

The total scan time is dominated by those 200 new files rather than the full 10,000.

---

## When a Cached Result Becomes Invalid

A cached result is discarded and the file is re-analyzed whenever any of these change:

- **File content changed** — a different modification time or file size (or a different MD5 if that option is enabled)
- **Engine changed** — you switched from one engine to another
- **Threshold changed** — a different threshold means the detection decision may have changed

The threshold invalidation in particular is worth understanding: if a file scored 0.55 under a threshold of 0.60 (below threshold, not flagged), and you then lower the threshold to 0.50, that file should now be flagged. Automatically re-analyzing it on the next scan ensures your results reflect your current settings.

---

## Clearing the Cache

You can clear the entire cache at any time from **Configuration → Cache → Clear Cache**. This removes all stored records and forces a full re-analysis on the next scan.

This is useful when:
- You have changed engines and want a clean slate
- You suspect the cache contains stale data from a previous configuration
- You want to verify that a fresh scan agrees with your cached results

---

## Cache Size and Storage

The cache database is stored locally in your user profile. The Configuration → Cache panel shows the current size and the number of records stored. Cache size grows with the number of files you have scanned. For most personal collections, it remains small (a few MB). For very large collections (hundreds of thousands of files), it may grow to tens of MB.

---

## Related Pages

- [Scan Cache](../features/scan-cache.md) — configuration, MD5 option, and use-case guidance
- [Detection Threshold](../features/detection-threshold.md) — why threshold changes invalidate cache entries
