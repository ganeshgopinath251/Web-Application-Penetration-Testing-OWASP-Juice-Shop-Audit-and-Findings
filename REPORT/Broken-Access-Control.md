# 🔓 Broken Access Control (BAC) Finding Report

**OWASP Top 10 Category:** A01:2021 - Broken Access Control
**Severity:** **High**

---

## 1. Description

* **Vulnerability:** Insecure Direct Object Reference (IDOR) against a collection endpoint. An authenticated user with a low-privilege role (`customer`) was able to bypass the server's authorization layer by accessing an API endpoint reserved exclusively for Administrators.
* **Target:** REST API Endpoint: `GET /api/Users`
* **Impact:** **Unauthorized disclosure of sensitive data** for all system users, including IDs, email addresses, and roles (`admin`, `deluxe`). This exposure provides critical information for further targeted attacks.

## 2. Step-by-Step Exploitation (Proof of Concept)

The attack successfully leveraged a valid customer session to access an admin-only API resource.

* **2.1 Initial Observation:** Logged in as a standard customer. The session used a valid JWT token with the claim `"role": "customer"`.
* **2.2 Attack Method:** The valid customer JWT was extracted and used to authenticate a direct request to the sensitive API endpoint. The request was sent via Burp Suite Repeater.
* **2.3 Request Sent (via Burp Repeater):**
    ```http
    GET /api/Users HTTP/1.1
    Host: 127.0.0.1:3000
    Authorization: Bearer [VALID_CUSTOMER_JWT_TOKEN]
    Accept: application/json
    ```
* **2.4 Result/Proof:**
    * The server returned a successful **HTTP 200 OK** status instead of a `403 Forbidden` error.
    * The response body contained a JSON array listing all user objects in the database, including all administrative and deluxe accounts.
    * **Response Snippet:**
        ```json
        {
          "status":"success",
          "data":[
            {"id":1,"email":"admin@juice-sh.op","role":"admin", ...},
            {"id":4,"email":"bjoern.kimminich@gmail.com","role":"admin", ...},
            // ... (Includes all users)
          ]
        }
        ```
  * **Screenshot: Successful Access to `/api/Users` as Customer**
        !(broken access control output.png)
