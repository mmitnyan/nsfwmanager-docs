# Scan Cache
## Accelerating Repeated Scans with Persistent Results

The **scan cache** allows NSFW Manager to remember the result of every file it has analyzed. On subsequent scans of the same folder, already-analyzed files are skipped entirely — results are reused instantly from cache, and only new or changed files require actual AI analysis.

The cache is disabled by default. Enable it in **Configuration → Cache**.

---

## Why Enable the Scan Cache

Without the cache, every scan is a full analysis from scratch. This is fine for a one-time scan of a new folder. It becomes a significant time cost when you scan the same folder regularly.

**Enable the cache when:**
- You scan the same folder repeatedly (daily or weekly)
- You have a phone sync folder that accumulates new photos while most existing ones stay the same
- You have a large NAS or cloud-sync directory where only a fraction of files change between scans
- You want to re-scan after adjusting settings without waiting for a full re-analysis

With the cache enabled, a 10,000-file folder where 9,900 files haven't changed will rescan in seconds rather than hours.

**Leave the cache off when:**
- You are doing a one-time scan of a folder you will not scan again
- You scan different folders every time
- You frequently change the detection threshold — the cache stores results computed at a specific threshold, so changed settings can make cached results inconsistent (see the Threshold section below)
- You want to guarantee that every result is fresh

---

## How Cache Validity Is Determined

When scanning a file, NSFW Manager checks whether a valid cached result already exists. A result is considered valid if all of these match the stored record:

1. **File path** — same location on disk
2. **File size** — same byte count
3. **Last modified time** — same modification timestamp
4. **Engine** — same AI engine selected
5. **Model variant** — same model (int8, fp16, onnx)
6. **Detection threshold** — same threshold value

If all six match, the cached result is reused. If any one differs, the file is re-analyzed and the cache is updated.

---

## MD5 Verification (Optional)

By default, the cache uses file size and modification time to detect changes. This is fast and sufficient for most workflows.

**Enable MD5 verification** (Configuration → Cache → Use MD5 verification) to add a cryptographic content check on top of the standard validation.

**Why you would want MD5:**
- Cloud sync tools (Dropbox, OneDrive, iCloud Drive) sometimes update file modification timestamps when syncing without actually changing the file content. This would cause NSFW Manager to re-analyze files unnecessarily. With MD5, the actual content is verified, so unchanged files are correctly recognized as cached even if their timestamps were touched.
- Conversely, some tools replace files with identical names and timestamps but different content. Without MD5, NSFW Manager would reuse the old cached result for new content. With MD5, the content change is detected and the file is re-analyzed.

**Why MD5 is off by default:**
Computing an MD5 hash requires reading the entire file, even just to check the cache. For a collection of 10 GB of photos, this means reading 10 GB of data before any AI analysis even starts. For most users, modification time is a reliable enough indicator, and the overhead of MD5 is not justified.

**Use MD5 when:** you work with cloud-synced folders where files may have altered metadata, or when strict accuracy matters more than scan speed.

---

## The Cache and Detection Threshold

Cached results are stored together with the threshold value that was active when the file was analyzed. NSFW Manager automatically re-analyzes any file whose cached threshold no longer matches the current setting.

This means that if you change your threshold from 0.50 to 0.40, every file in cache will be re-analyzed once — because the detection decision (flagged vs. safe) may have changed. After that first re-scan at the new threshold, subsequent scans benefit from the cache again.

You can also manually clear the entire cache at any time from **Configuration → Cache → Clear Cache**. This forces a fresh full analysis on the next scan.

---

## Cache Statistics

The Configuration → Cache tab shows current cache statistics: the number of tracked files, the number of stored detection results, and the total size of the cache on disk. This lets you see at a glance how much the cache covers and how much space it uses.

---

## Related Pages

- [Detection Threshold](./detection-threshold.md) — why the threshold affects cache validity
- [Video Support](./video-support.md) — video scan results are also cached
