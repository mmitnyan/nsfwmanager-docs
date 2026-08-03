# License Security  
## How NSFW Manager Protects License Keys and Validates User Access

This document explains how NSFW Manager handles license validation, key security, machine binding, and demo‑mode restrictions. It describes the client‑side and server‑side mechanisms used to ensure that only legitimate users can unlock the full functionality of the application.

---

## 📌 Overview

NSFW Manager uses a secure, server‑validated licensing system based on:

- HMAC‑derived license keys  
- Email‑bound license generation  
- Optional machine binding  
- Encrypted API communication  
- Server‑side verification of license status  
- Graceful fallback to demo mode  

The system is designed to be simple for users while preventing unauthorized use.

---

# 🔐 License Key Structure

A license key is generated from the user’s email using a secure hashing method.  
This ensures:

- each key is unique  
- keys cannot be guessed  
- keys cannot be forged  
- keys cannot be reused across accounts  

The key is **not** reversible and does not expose the user’s email.

---

# 🌐 Server‑Side Validation

When a user enters their email and license key, NSFW Manager sends a validation request to:
https://api.nsfwmanager.com/api/license/validate.php


### The server checks:

- whether the email exists  
- whether the key matches the stored value  
- whether the license is active  
- whether the license is expired  
- whether the license is bound to a machine  
- whether the machine ID matches (if applicable)  
- the license plan (trial, pro, lifetime)

### The server returns:

- `valid` (true/false)  
- `reason` (if invalid)  
- `plan`  
- `expires_at`  
- `machine_id`  
- `first_activation`  

All responses are JSON and transmitted over HTTPS.

---

# 🖥 Machine Binding (Optional)

Some licenses may be bound to a specific machine.  
When machine binding is enabled:

1. NSFW Manager computes a **SHA‑256 hardware identifier**  
2. The identifier is sent to the server  
3. The server stores the machine ID on first activation  
4. Future validations must match the stored machine ID  

### Benefits

- prevents sharing of license keys  
- ensures one license = one machine  
- protects commercial licenses  
- allows controlled activation behavior  

Machine binding is optional and depends on the license plan.

---

# 🔒 Security Considerations

### No Local Trust  
The application does **not** trust local license files.  
All validation is performed server‑side.

### No Offline Activation  
Offline activation is intentionally not supported to prevent:

- key sharing  
- reverse engineering  
- offline cracking  
- unauthorized redistribution  

### No Sensitive Data Stored Locally  
Only minimal metadata is stored locally:

- email  
- license key  
- validation status  

No passwords or server secrets are stored on the client.

---

# 🧪 Demo Mode Restrictions

When no valid license is present, NSFW Manager operates in **demo mode**.

### Demo mode allows:

- scanning  
- previewing  
- quarantine management  
- restoring quarantined files  
- rescanning directories  

### Demo mode limits:

Users can perform **five actions** per session:

- delete  
- move  
- quarantine  

After five actions:

- destructive actions are disabled  
- scanning remains fully available  
- quarantine viewing remains available  
- restarting the application resets the counter  

This ensures users can evaluate the software without unrestricted use.

---

# 🔁 License Retrieval

The License panel allows users to:

- enter email + password to retrieve a license  
- enter email + key directly  
- open the purchase page on the official website  
- view current license status  
- refresh license validation  

All communication uses HTTPS.

---

# 📁 Log Locations

License validation events may appear in:
%APPDATA%\Roaming\NsfwManager\logs\NsfwManager.log
%LOCALAPPDATA%\NsfwManager\logs\startup.log
%LOCALAPPDATA%\NsfwManager\logs\execution.log


These logs help diagnose:

- invalid key errors  
- expired license warnings  
- machine mismatch issues  
- server communication failures  

---

# 📌 Summary

NSFW Manager’s licensing system provides:

- secure HMAC‑based key generation  
- server‑side validation  
- optional machine binding  
- encrypted communication  
- demo‑mode restrictions  
- safe per‑user storage  

This ensures that legitimate users can unlock the full application while preventing unauthorized use.

---

