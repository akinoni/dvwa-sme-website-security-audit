# SME Website Security Audit — DVWA Lab (Capstone Project)

**Cybersecurity NextGen Cohort — Capstone Project**
**Fellow:** Akinwumi Oni · **Fellow ID:** FE/26/717977653

> ⚠️ **Lab-only project.** This repository documents a security assessment performed **exclusively against a self-hosted, isolated virtual lab** running the intentionally vulnerable [Damn Vulnerable Web Application (DVWA)](https://github.com/digininja/DVWA), used here as a stand-in for a real small-and-medium-enterprise (SME) website. No production system, third-party asset, or system outside the assessor's own VMware lab was accessed, scanned, or tested at any point.

---

## 📌 Project Overview

Small businesses are frequently targeted precisely because they lack dedicated security staff and often run web applications with known, fixable weaknesses. This project simulates a real-world SME website security audit end-to-end:

**Recon → Automated Scan → Manual Exploitation → Risk Rating → Remediation → Verification**

DVWA was deployed on a fresh Ubuntu Server VM (Apache + MariaDB + PHP) to stand in for the SME's live site, and assessed from a separate Kali Linux VM acting as the attacker, both isolated on a private VMware host-only network.

## 🎯 Objectives

- Perform a scoped, safe security review of a small web application
- Identify and risk-rate real vulnerabilities using industry-standard tools
- Prove exploitability of the highest-impact findings, not just flag them
- Remediate the most critical issues and verify the fix actually closes the gap
- Produce a professional deliverable a real SME owner or developer could act on

## 🧪 Lab Environment

| Component          | Details                                                                        |
| ------------------ | ------------------------------------------------------------------------------ |
| Attacker machine   | Kali Linux (VMware VM) — Firefox, OWASP ZAP 2.17.0, Nmap 7.98, Wireshark 4.6.4 |
| Target / SME host  | Ubuntu Server (VMware VM) — Apache 2.4.63, MariaDB 11.4.7, PHP 8.4             |
| Target application | [DVWA](https://github.com/digininja/DVWA) (security level: Low)                |
| Network            | Isolated VMware host-only/NAT segment (no internet-facing exposure)            |

## 🛠️ Tools Used

- **Nmap** — host discovery and service/version enumeration
- **OWASP ZAP** — automated spidering, active scanning, alert triage
- **Wireshark** — passive network traffic capture and analysis
- **Manual browser-based testing** — exploitation and verification of SQL Injection and Stored XSS
- **nano / bash / MariaDB CLI / OpenSSL** — remediation on the host (code fixes, TLS enablement)

## 🔎 Key Findings

| ID   | Finding                                             | Risk             | Status                     |
| ---- | --------------------------------------------------- | ---------------- | -------------------------- |
| F-01 | SQL Injection (in-band, error-based)                | 🔴 Critical      | ✅ Fixed & verified        |
| F-02 | Stored Cross-Site Scripting (XSS)                   | 🟠 High          | 🔧 Documented, fix pending |
| F-03 | Unencrypted HTTP traffic (no TLS in transit)        | 🟠 High          | ✅ Fixed & verified        |
| F-04 | Content Security Policy (CSP) header not set        | 🟡 Medium        | 🔧 Recommended             |
| F-05 | Missing anti-clickjacking header (X-Frame-Options)  | 🟡 Medium        | 🔧 Recommended             |
| F-06 | Directory browsing enabled                          | 🟡 Medium        | 🔧 Recommended             |
| F-07 | Server version disclosed via `Server` header        | 🟢 Low           | 🔧 Recommended             |
| F-08 | Session cookie missing `HttpOnly` flag              | 🟢 Low           | 🔧 Recommended             |
| F-09 | Session cookie missing `SameSite` attribute         | 🟢 Low           | 🔧 Recommended             |
| F-10 | `X-Content-Type-Options` header missing             | 🟢 Low           | 🔧 Recommended             |
| F-11 | Minor information disclosure (hidden file / banner) | ⚪ Informational | 🔧 Recommended             |

Full details, evidence, and remediation steps for every finding are in the [assessment report](./report).

## ✅ Remediation Highlights

- **SQL Injection (F-01):** Rewrote the vulnerable query in `vulnerabilities/sqli/source/low.php` from raw string concatenation to a **PDO prepared statement** with a bound parameter, then re-tested the same payloads to confirm the fix holds.
- **No Transport Encryption (F-03):** Enabled `mod_ssl` / `mod_headers`, generated a self-signed certificate, and activated the `default-ssl` Apache site so the application is now served over HTTPS.

## 📁 Repository Structure

```

sme-website-security-audit/
├── README.md # You are here
├── report/
│ ├── SME_Website_Security_Audit_Report_Akinwumi_Oni.docx
│ └── SME_Website_Security_Audit_Report_Akinwumi_Oni.pdf
├── evidence/
│ ├── kali-attacker/ # Recon, ZAP scans, exploitation screenshots
│ └── ubuntu-host/ # Environment setup & remediation screenshots
└── video/
└── walkthrough.md # Link to the 2–3 min video walkthrough

```

## 📄 Deliverables

- 📝 [Full Assessment Report (DOCX/PDF)](./report)
- 🖼️ [Evidence screenshots — Kali (attacker)](./evidence/kali-attacker)
- 🖼️ [Evidence screenshots — Ubuntu (SME host)](./evidence/ubuntu-host)
- 🎥 [2–3 minute video walkthrough](./video/walkthrough.md)

## ⚖️ Disclaimer

DVWA is **intentionally vulnerable** software distributed for security training and explicitly warns against deploying it on a public-facing host. All testing in this project was confined to a private, isolated virtual lab owned by the assessor. This repository is a training deliverable for the Cybersecurity NextGen Cohort capstone and should not be treated as a certified penetration test of any live system.

## 👤 Author

**Akinwumi Oni**
Fellow ID: FE/26/717977653
Cybersecurity NextGen Cohort
