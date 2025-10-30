# 🚀 Cross-Site Scripting (XSS) Finding Report

**OWASP Top 10 Category:** A07:2021 - Identification and Authentication Failures (Historical A03)
**Severity:** **High**

---

## 1. Description

* **Vulnerability:** Reflected Cross-Site Scripting (XSS). The application failed to perform context-sensitive encoding on user-supplied input from the search query parameter before reflecting it into the HTML of the results page.
* **Target:** The main search input field (URL parameter `q`).
* **Impact:** Potential for **session hijacking** (stealing cookies), user redirection to malicious sites, and execution of arbitrary JavaScript within the context of the vulnerable website, compromising user trust and data integrity.

## 2. Step-by-Step Exploitation (Proof of Concept)

The attack bypassed typical script filters using a benign tag combined with a JavaScript event handler.

* **2.1 Initial Observation:** Input placed in the search field is immediately displayed on the results page, indicating reflection.
* **2.2 Attack Method:** An image tag payload with an `onerror` event handler was used to execute code when the image failed to load.
* **2.3 Payload Sent (Injected into the search box):**
    ```html
    <img src="x" onerror="alert('XSS')">
    ```
* **2.4 Result/Proof:**
    * The browser failed to load the image (`src="x"`), triggering the `onerror` event handler.
    * The embedded JavaScript command (`alert('XSS')`) executed successfully, resulting in a browser pop-up box.
    * **URL Snapshot:** The payload is visible and executed directly from the URL.
        ```
        127.0.0.1:3000/#/search?q=<img%20src="x"%20onerror="alert('XSS')">
        ```
    * **Screenshot: Successful XSS Alert Box in Browser**
        !(xss output.png)

## 3. Recommended Remediation

* **Context-Sensitive Output Encoding (Primary):** All untrusted user-supplied data must be encoded based on the exact location in the HTML where it will be rendered. For the HTML body, angle brackets (`<` and `>`) must be converted to their HTML entities (`&lt;` and `&gt;`).
* **Content Security Policy (CSP):** Implement a strict CSP header to instruct the browser to only execute scripts from trusted, whitelisted sources. This helps block malicious inline scripts.
