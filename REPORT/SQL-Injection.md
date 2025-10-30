# 🦠 SQL Injection (SQLi) Finding Report

**OWASP Top 10 Category:** A03:2021 - Injection
**Severity:** **High**

---

## 1. Description

* **Vulnerability:** **SQL Injection** discovered within a search or product listing API endpoint (`/rest/products/search`). The application failed to perform input sanitization on the search query parameter, allowing an attacker to inject SQL logic.
* **Target:** Product Search API (`GET /rest/products/search?q=[INPUT]`).
* **Impact:** Unauthorized data exposure, allowing the retrieval of all products, potentially including hidden, unreleased, or archived items. This confirms the application is vulnerable to broader data leakage via SQLi.

## 2. Step-by-Step Exploitation (Proof of Concept)

The attack uses a simple tautology to bypass the intended search filter and return all available records.

* **2.1 Initial Observation:** The product search functionality uses a query parameter (`q`) whose input is reflected in the API's backend query.
* **2.2 Attack Method:** A payload designed to inject a logically true condition was used to bypass the WHERE clause. The payload was URL-encoded for injection via the browser or Burp.
* **2.3 Payload Sent (Injected into search box):** ` ' OR 1=1 -- `
* **2.4 Result/Proof:**
    * The server returned a successful **HTTP 200 OK** status.
    * The response body contained a JSON array listing *all* products in the database (e.g., "Apple Juice," "Orange Juice"), confirming the database query's filtering was successfully bypassed.
    * **Screenshot: SQL Injection Bypass in Search API**
        !(sqli output.png
)

## 3. Recommended Remediation

* **Prepared Statements (Primary Fix):** Use **parameterized queries** for all database interactions. This ensures user input is strictly treated as **data** and can never be executed as part of the SQL command.
* **Input Validation:** Implement strict whitelisting for allowed characters or enforce data type checks on search parameters.
