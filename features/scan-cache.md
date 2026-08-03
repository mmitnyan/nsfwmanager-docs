# Scan Cache  
## How NSFW Manager Accelerates Repeated Scans

The scan cache system allows NSFW Manager to dramatically speed up repeated scans of the same directory. Instead of reprocessing every file, the application stores previous scan results in a local SQLite database and reuses them when appropriate. This feature is optional and disabled by default.

---

## 📌 Overview

The scan cache is designed for users who frequently scan the same folders — for example, synced phone photo directories or recurring media collections. When enabled, NSFW Manager stores:

- file path  
- file size  
- last modified timestamp  
- detection score  
- detection reason  
- engine used  

On subsequent scans, cached results are reused unless the file has changed.

---

# ⚡ Performance Benefits

### First Scan
The first scan with cache enabled is similar in speed to a normal scan.  
The only additional overhead is writing results to the SQLite database.

### Subsequent Scans
Re-scanning the same directory becomes **extremely fast**:

- Files already in cache are skipped  
- Only new or modified files are analyzed  
- Large directories can be re-scanned in seconds  

This is ideal for:

- iPhone/Android photo sync folders  
- NAS or cloud-synced directories  
- Daily or weekly repeated scans  
- Large collections where only a few files change over time

---

# 🧩 MD5 Verification (Optional)

The cache system includes an optional integrity check:

### **Use MD5 verification**
- Disabled by default  
- When enabled, NSFW Manager computes an MD5 hash for each file  
- Ensures cached results are valid and the file has not changed  
- Slightly slower on repeated scans due to hashing overhead  

Recommended when:

- files may be edited or replaced  
- external tools modify metadata  
- users want strict accuracy for cached results

Not recommended when:

- scanning extremely large files  
- prioritizing maximum speed over strict validation

---

# 🗄 Cache Storage

The scan cache is stored in a local SQLite database under:
%LOCALAPPDATA%\NsfwManager\scan_cache.db


This location is:

- user-writable  
- safe for per-user MSI installations  
- isolated from system directories  
- compatible with roaming profiles (when applicable)

---

# 🔍 How NSFW Manager Determines Cache Validity

When scanning a directory, NSFW Manager checks:

1. **File path**  
2. **File size**  
3. **Last modified timestamp**  
4. **MD5 hash** (only if enabled)  
5. **Engine used**  

If all conditions match the cached entry:

- The cached score and reason are reused  
- The file is skipped during analysis  
- The scan continues to the next file

If any condition differs:

- The file is re-scanned normally  
- The cache entry is updated

---

# 🧹 Clearing the Cache

Users can clear the scan cache from the **Cache** tab in Settings.

Clearing the cache:

- removes all stored entries  
- forces a full re-scan on the next run  
- is useful if the database becomes large or corrupted  
- does not affect quarantine or user settings

---

# 🛠 When to Enable the Scan Cache

### Recommended
- Frequently scanned directories  
- Phone photo sync folders  
- Large collections with few changes  
- Daily or weekly scanning workflows  
- Users who want maximum speed on repeated scans

### Not Recommended
- One-time scans  
- Highly dynamic directories with constant file changes  
- Environments where strict validation is required but MD5 is disabled

---

# 📁 Log Locations

Cache-related events may appear in:
%APPDATA%\Roaming\NsfwManager\logs\NsfwManager.log
%LOCALAPPDATA%\NsfwManager\logs\startup.log
%LOCALAPPDATA%\NsfwManager\logs\execution.log


These logs help diagnose:

- cache initialization  
- database write errors  
- MD5 verification failures  
- fallback to normal scanning  

---

# 📌 Summary

The scan cache system provides:

- dramatically faster repeated scans  
- optional MD5 integrity verification  
- safe per-user storage  
- automatic detection of modified files  
- seamless integration with all engines  

When enabled, NSFW Manager becomes significantly more efficient for recurring workflows, especially in large or frequently updated directories.

---



