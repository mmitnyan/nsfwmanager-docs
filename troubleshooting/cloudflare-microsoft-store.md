# Cloudflare & Microsoft Store Ingestion  
## Understanding and Resolving MSI Download Issues

This document explains why the Microsoft Store ingestion system was unable to download the NSFW Manager MSI package, and how Cloudflare configuration was adjusted to allow proper ingestion while keeping the domain secure.

---

## 📌 Background

During the submission of NSFW Manager to the Microsoft Store, the Partner Center displayed the following error:

> **“The package Url does not contain Win32 Package.”**

This message is misleading: it does **not** mean the MSI is invalid.  
It means the Microsoft Store ingestion bot **failed to download the MSI**.

The root cause was an interaction between **Cloudflare security features** and the **Microsoft ingestion bot**.

---

## 📌 Why Microsoft Could Not Download the MSI

The Microsoft Store ingestion bot:

- does **not** use a real browser  
- does **not** execute JavaScript  
- cannot solve CAPTCHA or challenges  
- often uses **empty** or **outdated** User-Agent strings  
- sometimes identifies as **Firefox 19**  
- originates from IP ranges belonging to **Microsoft Azure (ASN 8075)**

Meanwhile, Cloudflare was enforcing:

- **Bot Fight Mode**  
- **Managed Challenge**  
- **Custom Rules**  
- **Browser Integrity Check**  
- **User-Agent blocking**

As a result:  
👉 Cloudflare consistently blocked the ingestion bot  
👉 Microsoft could not download the MSI  
👉 The submission failed

---

## 📌 Diagnostic Findings

Cloudflare logs showed:

- requests from **ASN 8075 (Microsoft Corporation)**  
- **empty** or **non-standard** User-Agent strings  
- Cloudflare challenges applied to legitimate ingestion requests  
- repeated failed attempts to download the MSI

This confirmed that **Cloudflare was blocking Microsoft**, not the other way around.

---

## 📌 Solution: Whitelist Microsoft via ASN 8075

The most reliable solution is to create a Cloudflare rule that **allows all traffic originating from ASN 8075**, which is used by:

- Microsoft Store ingestion  
- Azure services  
- Windows Update  
- App Installer  
- Delivery Optimization  
- BITS

### ✔ Recommended Cloudflare Rule

**Condition:**  
- Field: **AS Num**  
- Operator: **equals**  
- Value: **8075**

**Action:**  
- **Skip**

**Modules to skip:**

- All remaining custom rules  
- All managed rules  
- Super Bot Fight Mode  
- User-Agent blocking  
- Browser Integrity Check  
- Security Level  
- Legacy rate limiting

**Execution order:**  
👉 **This rule must be placed at the top of the rule list.**

---

## 📌 Result

After applying the rule:

- Microsoft successfully downloaded the MSI  
- The error “The package Url does not contain Win32 Package” disappeared  
- The Win32 submission was accepted  
- The application review process began normally  
- Cloudflare security remained intact for all other visitors

---

## 📌 Recommendations for Future Releases

- Keep the ASN 8075 rule at the top of the Cloudflare rule list  
- Avoid challenges on `.msi` or `.msix` files for Microsoft ingestion  
- Avoid User-Agent-based blocking for legitimate bots  
- Check Cloudflare logs when submitting new versions of the MSI

---

## 📌 Summary

The issue was not caused by the MSI file, its signature, or its structure.  
It was caused by **Cloudflare blocking the Microsoft Store ingestion bot**.

Whitelisting **ASN 8075** ensures that Microsoft can properly ingest Win32 packages.

---
