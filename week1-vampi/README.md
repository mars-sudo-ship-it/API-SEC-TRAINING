# Week 1 – VAmPI API Security Lab

## 📌 Overview
This week focused on testing the **VAmPI vulnerable API** using Postman, Burp Suite, and 42Crunch.  
The objective was to simulate real-world API security assessments, identify vulnerabilities, and document findings with professional rigor.

---

## 🛠️ Tools & Setup
- **Postman** → API request crafting & replay
- **Burp Suite** → Proxy interception, traffic manipulation
- **42Crunch** → OpenAPI spec validation & security audit
- **Docker/Kali VM** → Lab environment for reproducibility

---

## 🎯 Objectives
- Configure Postman → Burp proxy for traffic interception  
- Run baseline requests against VAmPI endpoints  
- Validate OpenAPI spec with 42Crunch  
- Document vulnerabilities with CVSS scoring  

---

## 🔎 Key Findings (Week 1 Vulnerabilities)
During testing, the following **four vulnerabilities** were identified:

1. **Broken Object Level Authorization (BOLA)**  
   - Unauthorized access to other users’ data by manipulating object IDs.  
   - OWASP API1:2019  

2. **Broken Authentication**  
   - Weak/default credentials and poor token validation allowed unauthorized login.  
   - OWASP API2:2019  

3. **Excessive Data Exposure**  
   - API responses leaked sensitive fields (e.g., password hashes, internal IDs).  
   - OWASP API3:2019  

4. **Mass Assignment**  
   - Insecure handling of input allowed privilege escalation (e.g., setting `role=admin`).  
   - OWASP API6:2019  

---

## 📂 Evidence

- **Report & Pocs :** [report.md](./report.md)  

---

## 📊 CVSS Scoring Example
| Vulnerability            | CVSS Base Score | Severity | Vector |
|---------------------------|-----------------|----------|--------|
| BOLA                      | 8.7             | High     | AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:N |
| Broken Authentication     | 7.5             | High     | AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N |
| Excessive Data Exposure   | 6.5             | Medium   | AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N |
| Mass Assignment           | 7.8             | High     | AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:N |

---

## 📚 OWASP API Security Mapping
- **API1:2019 – Broken Object Level Authorization**  
- **API2:2019 – Broken Authentication**  
- **API3:2019 – Excessive Data Exposure**  
- **API6:2019 – Mass Assignment**

---

## 💡 Reflections
Week 1 was challenging due to proxy setup and spec compatibility issues.  
Persistence and minimal working solutions helped overcome frustration.  
This lab reinforced the importance of **layered testing** (Postman → Burp → 42Crunch) for reliable findings.

---

## 🔗 Links
- [LinkedIn Reflection Post](#) *(to be added)*  
- [Medium Article: Why API Security Matters](#) *(to be added)*  

---

## ✅ Next Steps
- Week 2: Explore API Authentication (JWTs, API keys, token-based access control)  
