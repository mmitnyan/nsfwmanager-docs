# SQLite Scan Cache Database  
## How NSFW Manager Stores and Reuses Scan Results

NSFW Manager uses a local SQLite database to store scan results and dramatically accelerate repeated scans. This document explains how the database is structured, how entries are validated, and how the cache interacts with the detection engines.

---

## 📌 Overview

The scan cache is implemented using a lightweight SQLite database stored under:
%LOCALAPPDATA%\NsfwManager\scan_cache.db


The database contains one table that stores metadata for each scanned file, including:

- file path  
- file size  
- last modified timestamp  
- detection score  
- detection reason  
- engine used  
- optional MD5 hash (if enabled)

This allows NSFW Manager to skip re‑scanning files that have not changed.

---

## 📌 Detailed Overview

The scan cache database contains:

1. **`file_cache`**  
   Stores metadata for each scanned file.

2. **`engine_results`**  
   Stores detection results per engine, per model, per threshold.

This separation ensures:

- file metadata is stored once  
- each engine/model combination stores its own results  
- switching engines or thresholds forces re‑scan  
- MD5 verification remains optional  
- cache stays consistent across updates  

---

# 🗄 Table: `file_cache`

This table stores one entry per file.

```sql
CREATE TABLE file_cache (
    id     INTEGER PRIMARY KEY AUTOINCREMENT,
    path   TEXT UNIQUE NOT NULL,
    mtime  REAL NOT NULL,
    fsize  INTEGER NOT NULL,
    md5    TEXT
);


---

# 🗄 Database Structure

The SQLite database contains a single main table:

### **Table: scan_cache**

| Column | Type | Description |
|--------|------|-------------|
| `id` | INTEGER PRIMARY KEY | Unique entry ID |
| `file_path` | TEXT | Absolute path of the file |
| `file_size` | INTEGER | Size in bytes |
| `last_modified` | INTEGER | Last modified timestamp |
| `engine` | TEXT | Engine used (int8, fp16, full, ifnude) |
| `score` | REAL | Detection score |
| `reason` | TEXT | Detection label |
| `md5` | TEXT (optional) | MD5 hash if enabled |
| `created_at` | INTEGER | Timestamp of first scan |
| `updated_at` | INTEGER | Timestamp of last update |

This schema is intentionally simple to maximize speed and reliability.


Column Details
|-|-|
|Column	|Description|
|id|	Unique file identifier|
|path|	Absolute file path (unique)|
|mtime|	Last modified timestamp (float)|
|fsize|	File size in bytes|
|md5|	Optional MD5 hash (only stored when MD5 verification is enabled)|

This table allows NSFW Manager to quickly determine whether a file has changed since the last scan.

🗄 Table: engine_results
This table stores one entry per engine, per model, per file.

CREATE TABLE engine_results (
    file_id       INTEGER NOT NULL
                  REFERENCES file_cache(id) ON DELETE CASCADE,
    engine_id     TEXT NOT NULL,
    engine_model  TEXT NOT NULL,
    score         REAL,
    label         TEXT,
    threshold     REAL NOT NULL,
    cached_at     TEXT NOT NULL,
    PRIMARY KEY (file_id, engine_id, engine_model)
);

Column Details
|-|-|
|Column|	Description|
|file_id|	Foreign key referencing file_cache.id|
|engine_id|	Engine name (int8, fp16, full, ifnude)|
|engine_model|	Model variant (e.g., “default”, “fast”, “full”)|
|score|	Detection score returned by the engine|
|label|	Detection label (Pornography, Suggestive, Hentai, Safe, etc.)|
|threshold|	Threshold used during detection|
|cached_at|	Timestamp when the result was stored|

Why this design is powerful
- Multiple engines can store results for the same file  
Example: int8 + full + ifnude results coexist.

Multiple model variants are supported  
- Example: ifnude “fast” and ifnude “default” store separate entries.

Threshold is stored  
- Cached results remain valid even if the user changes their threshold later.

- Cascade delete  
Removing a file entry automatically removes all engine results.

---

⚡ How NSFW Manager Uses the Cache
On scan start:
1. Read file metadata (mtime, size)
2. Query file_cache for matching entry
3. If MD5 verification is enabled → compute MD5
4. Query engine_results for:
- - matching engine
- - matching model
- - matching threshold
5. If all fields match → reuse cached score
6. If any field differs → re-scan and update both tables

On re‑scan:
- If metadata matches → cached result reused
- If metadata differs → file reprocessed and cache updated

This ensures correctness while maximizing speed.

---

🔍 Cache Validation Logic
A cached entry is valid only if:
- path matches
- mtime matches
- fsize matches
- engine_id matches
- engine_model matches
- threshold matches
- md5 matches (only if MD5 verification is enabled)

If any of these differ, the file is re‑scanned.
This prevents false positives when:
- files are edited
- metadata changes
- engines are switched
- thresholds are adjusted
- users modify or replace files

---

🧩 MD5 Verification
MD5 verification is optional and disabled by default.

When enabled:
- NSFW Manager computes an MD5 hash for each file
- The hash is stored in file_cache.md5
- Future scans compare the hash to detect changes
- Re-scans are slightly slower due to hashing overhead

When disabled:
- Validation relies on size + timestamp
- Faster, but less strict

MD5 is recommended for directories where files may be edited or replaced.

---

🧹 Clearing the Cache
Users can clear the cache from the Cache tab in Settings.

Clearing the cache:
- deletes all database entries
- forces a full re-scan next time
- does not affect quarantine or user settings
- is safe and reversible

Useful when:
- the database grows large
- entries become outdated
- engines or thresholds change
- MD5 verification is toggled

---

🧪 Interaction With Detection Engines
Each engine stores its own results in the cache:
- int8 → fastest, binary scores
- fp16 → balanced, GPU-only
- full → maximum accuracy
- ifnude → granular anatomical labels

Cache entries include:
- engine name
- model variant
- threshold

This ensures cached results always match the active engine configuration.

---

📁 Log Locations
Cache operations appear in:

- %APPDATA%\Roaming\NsfwManager\logs\NsfwManager.log
- %LOCALAPPDATA%\NsfwManager\logs\startup.log
- %LOCALAPPDATA%\NsfwManager\logs\execution.log

These logs help diagnose:
- database initialization
- read/write errors
- MD5 mismatches
- engine switching behavior

---
📌 Summary
The SQLite schema used by NSFW Manager provides:

- fast and reliable caching
- engine‑specific results
- threshold‑aware validation
- optional MD5 integrity checking
- automatic cleanup via cascade deletes
- safe per-user storage

This design ensures that repeated scans are extremely fast while maintaining correctness and flexibility across all engines, including the granular ifnude model.

---


# ⚡ How the Cache Speeds Up Scanning

During a scan, NSFW Manager performs the following steps:

1. Read file metadata (size, timestamp)
2. Query the SQLite database for a matching entry
3. If MD5 verification is enabled, compute MD5 and compare
4. If all fields match:
   - **Reuse cached score and reason**
   - Skip engine inference
5. If any field differs:
   - Re-scan the file normally
   - Update the database entry

This approach ensures correctness while avoiding unnecessary work.

---



