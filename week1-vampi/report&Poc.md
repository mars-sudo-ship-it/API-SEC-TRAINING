# Week 1 – VAmPI API Penetration Test Report

**Tester:** Memory Mahanya  
**Date:** 2025-11-22  
**Environment:** Kali Linux VM with Postman, Burp Suite, VS Code (42Crunch), Docker  
**Scope:** VAmPI mock API endpoints (user registration, profile, `/me`, `/createdb`)  
**Methodology:** Reconnaissance, proxy interception, authentication/authorization testing, mass assignment, static audit, dynamic verification

---

## 📌 Executive Summary
The engagement simulated an adversary targeting the VAmPI mock API.  
Key objectives included:
- Reconnaissance of API endpoints  
- Authentication and authorization testing  
- Mass assignment and schema validation  
- Documentation with OWASP API Top 10 mapping and CVSS v4.0 scoring  

**Business Impact:**  
Weaknesses allow unauthorized attribute injection, privilege abuse, and data integrity risks. In production, this could lead to account escalation, regulatory exposure (GDPR/CCPA), reputational damage, and financial loss.

---

## 🛠️ Approach
- **Reconnaissance:** Enumerated endpoints via Postman collections  
- **Proxy interception:** Routed Postman traffic through Burp Suite (127.0.0.1:8080)  
- **Authentication testing:** Verified bearer token enforcement  
- **Authorization bypass:** Manipulated identifiers and HTTP methods (BOLA/BFLA)  
- **Mass assignment:** Injected backend-only fields (`admin`, `role`) into payloads  
- **Static audit:** 42Crunch review of OpenAPI spec (schema gaps, missing constraints)  
- **Evidence collection:** Screenshots, Burp HTTP history, PoC requests  

---

## 🔎 Key Findings

### 1. Broken Object Level Authorization (BOLA)
- **OWASP Mapping:** API1:2023  
- **CVSS v4.0 Base Score:** 7.4 (High)  
- **Evidence:** Changing `/users/v1/1` → `/users/v1/2` with same token exposed another user’s data.  
- **Impact:** Attackers can enumerate IDs and access other users’ resources.  
- **Recommendation:** Enforce object-level checks server-side; constrain path parameters with regex/patterns.

---

### 2. Unconstrained Path Parameters
- **OWASP Mapping:** API8:2023 (Security Misconfiguration), API4:2023 (Unrestricted Resource Consumption)  
- **CVSS v4.0 Base Score:** 6.5 (Medium)  
- **Evidence:** `/users/v1/abc` and oversized IDs accepted; injection attempt `/me/?=1 OR 1=1` returned valid data.  
- **Impact:** Enables injection, fuzzing, and denial-of-service.  
- **Recommendation:** Define strict patterns, formats, and maxLength constraints; enforce server-side validation.

---

### 3. Broken Authentication
- **OWASP Mapping:** API2:2023  
- **CVSS v4.0 Base Score:** 7.1 (High)  
- **Evidence:** Endpoints like `/createdb` and `/` accessible without bearer token.  
- **Impact:** Sensitive functionality exposed to unauthenticated users.  
- **Recommendation:** Apply `security: [{ bearerAuth: [] }]` to all operations; enforce token validation server-side.

---

### 4. Broken Object Property Level Authorization (Mass Assignment)
- **OWASP Mapping:** API6:2023  
- **CVSS v4.0 Base Score:** 7.5 (High)  
- **Evidence:** Payloads with `admin: true`, `role: superuser` accepted during registration.  
- **Impact:** Privilege escalation and data corruption possible.  
- **Recommendation:** Set `additionalProperties: false` in schemas; whitelist allowed fields; enforce RBAC for sensitive attributes.

---

## 📊 CVSS Summary
| Vulnerability | OWASP Mapping | CVSS v4.0 Score | Severity |
|---------------|---------------|-----------------|----------|
| BOLA          | API1          | 7.4             | High     |
| Path Params   | API4/API8     | 6.5             | Medium   |
| Broken Auth   | API2          | 7.1             | High     |
| Mass Assignment | API6        | 7.5             | High     |

---

## 🧩 Recommendations (Prioritized)

**Immediate (Critical):**
- Enforce property whitelisting (Mass Assignment)  
- Strengthen object/function-level authorization (BOLA/BFLA)  

**High Priority:**
- Harden schema validation (patterns, maxLength, enums)  
- Apply strict authentication controls (JWT security, token lifetimes)  

**Medium Priority:**
- Minimize response data (avoid excessive exposure)  
- Implement rate limiting and abuse prevention  

---

## 📚 Regulatory & Business Mapping
- **Mass Assignment (API6):** GDPR/CCPA, PCI DSS → privilege escalation risks  
- **BOLA (API1):** GDPR/HIPAA → unauthorized access to PII  
- **Broken Auth (API2):** PCI DSS, ISO 27001 → authentication failures  
- **Schema Gaps (API4/API8):** NIST/ISO 27001 → resilience requirements  

---

## ⚡ Challenges Faced
- Antivirus blocking Burp Suite on host → resolved by shifting to Kali VM  
- Proxy misconfiguration (Postman → Burp) → fixed by importing Burp CA and adjusting proxy settings  

---

## 📂 Evidence
See `/screenshots/` for Postman and Burp captures, and `/poc/` for exploit requests.



