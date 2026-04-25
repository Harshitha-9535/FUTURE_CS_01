# 🔐 Web Application Vulnerability Assessment
### Target: [demo.testfire.net](http://demo.testfire.net) (IBM AltoroMutual)

![Assessment Type](https://img.shields.io/badge/Assessment-Passive%20%2F%20Non--Intrusive-blue)
![Risk Level](https://img.shields.io/badge/Overall%20Risk-HIGH-red)
![Findings](https://img.shields.io/badge/Findings-11%20Total-orange)
![OWASP](https://img.shields.io/badge/Framework-OWASP%20Testing%20Guide%20v4.2-brightgreen)
![Status](https://img.shields.io/badge/Status-Complete-success)

---

## 📋 Project Overview

This project is a **passive, non-intrusive vulnerability assessment** of the IBM AltoroMutual demo web application (`demo.testfire.net`). This application is an **intentionally vulnerable banking app** created by IBM Corporation for security training and awareness purposes.

The assessment was conducted following the **OWASP Testing Guide v4.2** methodology and covers HTTP header analysis, port scanning, cookie security, and security misconfiguration identification.

> ⚠️ **Disclaimer:** This assessment was conducted entirely passively for educational and portfolio purposes. No attacks were performed. `demo.testfire.net` is a publicly available intentionally vulnerable application provided by IBM for security training.

---

## 🎯 Assessment Details

| Field | Details |
|-------|---------|
| **Target** | http://demo.testfire.net |
| **Application** | IBM AltoroMutual (Online Banking Demo) |
| **Assessment Type** | Passive / Non-Intrusive |
| **Date** | April 2026 |
| **Framework** | OWASP Testing Guide v4.2 |
| **Analyst** | Security Analyst |

---

## 📊 Findings Summary

| Severity | Count | Findings |
|----------|-------|---------|
| 🔴 **HIGH** | 4 | Missing CSP, Missing HSTS, Missing X-Frame-Options, Server Version Disclosure |
| 🟠 **MEDIUM** | 3 | Insecure Cookie (Secure flag), Insecure Cookie (HttpOnly), HTTP Not Enforced to HTTPS |
| 🟢 **LOW** | 2 | Missing X-Content-Type-Options, Missing Referrer-Policy |
| 🔵 **INFO** | 2 | SecurityHeaders.com Grade F, Error Messages Exposed |

**Overall Risk Rating: 🔴 HIGH**

---

## 🛠 Tools Used

| Tool | Purpose |
|------|---------|
| `curl` | HTTP header retrieval and response analysis |
| `Nmap` | Port scanning and service version detection |
| `OWASP ZAP` | Passive automated vulnerability scanning |
| `SecurityHeaders.com` | Automated security header grading |
| `Browser DevTools` | Cookie flag inspection, manual header verification |
| `Whois / nslookup` | Domain and DNS reconnaissance |

---

## 🔍 Key Vulnerabilities Identified

### 🔴 HIGH Severity

**VA-01 — Missing Content Security Policy (CSP)**
- No `Content-Security-Policy` header present
- Enables unrestricted XSS execution → session hijacking, credential theft
- OWASP: A05:2021 | CWE-693

**VA-02 — Missing HTTP Strict Transport Security (HSTS)**
- No `Strict-Transport-Security` header on HTTPS responses
- Allows SSL-stripping attacks on shared networks
- OWASP: A02:2021 | CWE-319

**VA-03 — Missing X-Frame-Options (Clickjacking)**
- No `X-Frame-Options` or `frame-ancestors` CSP directive
- Application can be embedded in malicious iframes → clickjacking on login page
- OWASP: A05:2021 | CWE-1021

**VA-04 — Server Version Disclosure**
- `Server: Apache-Coyote/1.1` and `X-Powered-By: Servlet/2.5` exposed
- Tomcat version visible in error pages
- OWASP: A05:2021 | CWE-200

### 🟠 MEDIUM Severity

**VA-05 — Session Cookie Missing Secure Flag**
- `JSESSIONID` transmitted over HTTP → network sniffing attack
- CWE-614

**VA-06 — Session Cookie Missing HttpOnly Flag**
- `document.cookie` accessible via JavaScript → XSS session exfiltration
- CWE-1004

**VA-07 — HTTP Not Redirected to HTTPS**
- Full application accessible over unencrypted HTTP on port 80
- No 301 redirect to HTTPS — all data transmitted in cleartext

---

## 🔧 Quick Remediation Reference

```http
# Add these headers to ALL HTTP responses:

Content-Security-Policy: default-src 'self'; script-src 'self'; object-src 'none'; frame-ancestors 'none'
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin

# Fix session cookie:
Set-Cookie: JSESSIONID=...; Path=/; Secure; HttpOnly; SameSite=Strict

# Suppress server headers in Tomcat server.xml:
<Connector server=" " />
```

---

## 📁 Project Structure

```
vuln-assessment/
├── README.md                          ← This file
├── report/
│   └── Vulnerability_Assessment_Report_demo.testfire.net.docx
└── screenshots/
    ├── curl_header_output.png          ← curl -sI response
    ├── nmap_scan_results.png           ← Port scan output
    ├── zap_alerts_overview.png         ← OWASP ZAP alerts panel
    ├── security_headers_grade.png      ← SecurityHeaders.com grade F
    ├── devtools_cookie_flags.png       ← Browser DevTools cookies tab
    └── devtools_response_headers.png  ← Browser DevTools network tab
```

---

## 📄 Report Contents

The full professional report (`/report/*.docx`) includes:

1. **Title Page** — Project metadata, classification, disclaimer
2. **Executive Summary** — Risk scorecard and key findings overview
3. **Scope** — In-scope components, constraints, assessment period
4. **Methodology** — 4-phase testing lifecycle, standards referenced
5. **Tools Used** — Tool descriptions and versions
6. **Findings Summary Table** — All 11 findings with risk ratings
7. **Detailed Vulnerability Analysis** — Per-finding: description, evidence, impact, remediation
8. **Risk Summary** — CIA impact matrix, overall rating justification
9. **Conclusion & Remediation Roadmap** — Prioritised P1/P2/P3 fix plan
10. **References** — OWASP, NIST, MDN, CWE sources

---

## 📚 Learning Outcomes

This project demonstrates:

- ✅ Passive web application security assessment methodology
- ✅ HTTP security header analysis and interpretation
- ✅ Cookie security flag analysis (Secure, HttpOnly, SameSite)
- ✅ Service fingerprinting and version disclosure identification
- ✅ OWASP Top 10 (2021) mapping of real-world findings
- ✅ Professional security report writing for academic and industry audiences
- ✅ Practical use of industry-standard security tools (ZAP, Nmap, curl)

---

## 🔗 Useful Resources

- [OWASP Top 10](https://owasp.org/Top10/)
- [OWASP Testing Guide v4.2](https://owasp.org/www-project-web-security-testing-guide/)
- [OWASP Secure Headers Project](https://owasp.org/www-project-secure-headers/)
- [Mozilla Web Security Guidelines](https://infosec.mozilla.org/guidelines/web_security)
- [SecurityHeaders.com](https://securityheaders.com)
- [SSL Labs SSL Test](https://www.ssllabs.com/ssltest/)
- [CWE Top 25](https://cwe.mitre.org/top25/)

---

## ⚖️ Legal & Ethical Notice

This project was conducted **entirely passively** on an **intentionally vulnerable demonstration application** (`demo.testfire.net`) provided by IBM Corporation for security training purposes.

- ❌ No active exploitation was performed
- ❌ No user data was accessed
- ❌ No denial of service or stress testing
- ✅ Assessment methodology complies with OWASP Testing Guide v4.2
- ✅ Suitable for academic submission and professional portfolio

> Performing security testing on systems you do not own or have explicit written permission to test is **illegal** in most jurisdictions. Always obtain proper authorisation before conducting any security assessment.

---

*Report generated: April 2026 | Framework: OWASP Testing Guide v4.2 | Classification: Academic / Portfolio*
