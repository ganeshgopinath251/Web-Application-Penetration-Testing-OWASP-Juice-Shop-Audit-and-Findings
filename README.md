# Web-Application-Penetration-Testing-OWASP-Juice-Shop-Audit-and-Findings

## 🎯 Project Summary

This repository documents a security assessment of the OWASP Juice Shop web application. The project showcases exploitation techniques (Proof-of-Concept), detailed analysis, and professional remediation strategies for high-impact vulnerabilities. Focus areas include **Broken Access Control (BAC)**, **Cross-Site Scripting (XSS)**, and **SQL Injection (SQLi)**, serving as a portfolio demonstration of web application security auditing proficiency.

## 🔍 Scope and Methodology

The assessment targeted the application's core logic, authentication, and data retrieval mechanisms.

| Tool | Purpose |
| :--- | :--- |
| **Burp Suite (Community/Professional)** | Intercepting, analyzing, and modifying HTTP requests/responses. |
| **Browser Developer Tools** | Inspecting client-side storage (Cookies, Local Storage) and DOM manipulation. |
| **Online JWT Tools** | Decoding and forging JSON Web Tokens (for analysis). |
| **Target Environment** | OWASP Juice Shop (Self-Hosted/Docker Instance) |

## 📊 Key Vulnerability Findings

The following table summarizes the successfully exploited vulnerabilities, aligning them with the OWASP Top 10 classifications.

| Vulnerability | OWASP Category (2021) | Severity | Status | Detailed Report |
| :--- | :--- | :--- | :--- | :--- |
| **Broken Access Control** | A01:2021 | High | Solved | [REPORT/Broken-Access-Control.md](REPORT/Broken-Access-Control.md) |
| **SQL Injection** | A03:2021 | Critical | Solved | [REPORT/SQL-Injection.md](REPORT/SQL-Injection.md) |
| **Cross-Site Scripting** | A07:2021 | High | Solved | [REPORT/Cross-Site-Scripting.md](REPORT/Cross-Site-Scripting.md) |

---

## 📁 Repository Structure

This project is organized into a primary `REPORT/` directory containing the detailed findings for each vulnerability.

