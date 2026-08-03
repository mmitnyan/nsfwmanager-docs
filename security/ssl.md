# SSL & Secure Licence Validation  
## Protecting Communication Between NSFW Manager and the Licence Server

NSFW Manager performs only one network operation:  
**secure licence validation.**

This document explains how SSL, encryption, and verification ensure that licence checks are safe, private, and tamper-resistant.

---

## 🔐 Why SSL Matters

Licence validation must be:
- Confidential  
- Authentic  
- Tamper-proof  
- Resistant to interception  

To achieve this, NSFW Manager uses:
- HTTPS (TLS 1.2+)  
- Certificate pinning  
- HMAC-SHA256 signatures  
- Anti-rollback protection  

---

# 🔒 SSL/TLS Security

### **1. Encrypted Channel**
All licence requests use:
https://api.nsfwmanager.com/

via TLS 1.2 or higher.

This ensures:
- No man-in-the-middle  
- No plaintext transmission  
- No sniffing of licence keys  

### **2. Certificate Validation**
NSFW Manager validates:
- Certificate chain  
- Issuer  
- Expiration  
- Hostname match  

If validation fails:
- The licence request is aborted  
- The user receives a clear error  
- No data is transmitted  

---

# 🧩 Payload Security

The payload includes:
- Licence key  
- Machine identifier (hashed)  
- Version number  

It does **not** include:
- File names  
- File content  
- Detection results  
- Folder structure  
- User identity  

Related documentation:  
**[Privacy](ca://s?q=Open_privacy_document)**

---

# 🔐 HMAC-SHA256 Verification

To prevent tampering:
- The server signs responses with HMAC-SHA256  
- NSFW Manager verifies the signature locally  
- Invalid signatures are rejected  

This protects against:
- Fake licence servers  
- Modified responses  
- Replay attacks  

---

# 🛡 Anti-Rollback Protection

NSFW Manager prevents:
- Downgrading licence files  
- Reusing expired trial tokens  
- Replaying old validation responses  

This ensures licence integrity over time.

---

# 🧪 Error Handling

If SSL validation fails:
- The licence is not activated  
- No data is sent  
- A clear message is shown  
- The user can retry safely  

Common causes:
- Expired certificate  
- Network filtering  
- Antivirus HTTPS inspection  
- Incorrect system clock  

---

# 📦 Version History

### **v2.0.3**
- HMAC verification added  
- Anti-rollback protection  
- Hardened SSL validation  

### **v2.0.2**
- Documentation added  

### **v2.0.0**
- Initial SSL licence validation  

---

# 📌 Summary

NSFW Manager uses strong SSL/TLS encryption, certificate validation, HMAC signatures, and anti-rollback protection to ensure licence validation is secure, private, and tamper-resistant.

Only licence data is transmitted — never your files.

---
