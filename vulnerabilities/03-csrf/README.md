# 🗺️ Module: Cross-Site Request Forgery (CSRF)

## 🟢 1. Low Difficulty

### 🔍 Vulnerability Analysis
* **The Goal:** Force an authenticated user into performing an unintended administrative action (such as changing their password) without their consent or knowledge.
* **The Flaw:** The application executes sensitive data updates using simple `GET` requests without validating the origin of the request or requiring un-guessable validation values (like unique anti-CSRF tokens).

### 💻 Code Flaw (Source Breakdown)
The application evaluates execution conditions purely based on the presence of URL-driven parameters:

```php
if( isset( $_GET[ 'Change' ] ) ) {
    // Get input
    $pass_new  = $_GET[ 'password_new' ];
    $pass_conf = $_GET[ 'password_conf' ];
    
    // ... (Database UPDATE query happens here without origin checking) ...
}
```
If the `Change` parameter exists inside the URL string (e.g., `?Change=Change`), the backend logic automatically triggers password modification. Because there are no restrictions verifying who or what website initiated the request, any external page can forge this action while the victim maintains an active browser session.

### 🛠️ Exploitation Steps
1. Navigated to the **CSRF** module while authenticated as the `admin` account.
2. Evaluated how the web interface builds requests, observing that the form appends new values straight into the visible browser address bar via URL arguments.
3. Created an exploit payload URL string capable of forcing a silent password reset to `abc`:
   ```text
   http://127.0.0
   ```
4. Executed the URL while logged into the portal session. 
5. **Result:** The database executed the update immediately, returning a `"Password Changed."` state. This confirms that forcing a logged-in user to click this link via an external phishing vector leads to a full account takeover.
<img width="894" height="252" alt="image" src="https://github.com/user-attachments/assets/8a80937a-b18d-41ae-8968-2a421bc59bc7" />


---

## 🟡 2. Medium Difficulty

### 🔍 Vulnerability Analysis
* **The Goal:** Force an authenticated user into modifying their credentials while bypassing the application's origin validation logic.
* **The Flaw:** The developer relies solely on checking if the server's domain string is contained anywhere inside the HTTP `Referer` request header. Because it uses a simple substring match instead of strict verification, an attacker can trick the validation by hosting the exploit on a custom subdomain or inclusion folder containing the target server's name.

### 💻 Code Flaw (Source Breakdown)
The main difference from the Low level is the implementation of a basic origin barrier checking function:

```php
if( stripos( \(_SERVER[ 'HTTP_REFERER' ],\)_SERVER[ 'SERVER_NAME' ] ) !== false ) {
    // ... (Executes password modification) ...
} else {
    echo "<pre>That request didn't look correct.</pre>";
}
```
* `$_SERVER['HTTP_REFERER']` logs the URL of the external page sending the request.
* `$_SERVER['SERVER_NAME']` identifies the domain of the application (e.g., `127.0.0.1` or `localhost`).
* `stripos(...) !== false` checks if the application's domain string is present *anywhere* in the Referer header value.

The vulnerability stems from using a loose substring search rather than checking the strict root domain or using proper anti-CSRF tokens.

### 🛠️ Exploitation Steps
1. Observed that a normal password modification request logs the authentic local host context inside the `Referer` header string (`http://localhost/dvwa/vulnerabilities/csrf/`), authorizing the change.
2. Verified that an attack originating from a regular external host (like `http://attacker.com`) fails and drops a `"That request didn't look correct."` validation error.
3. Bypassed the validation rule by hosting the malicious CSRF payload string inside an endpoint path named directly after the target server name (e.g., saving the file as `http://attacker.com` or naming a custom domain `://attacker.com`).
4. **Result:** When the browser automatically attaches the `Referer` header matching our file path, the application detects the string `localhost` within the text block, evaluates the check as valid, and forces the password change silently.
<img width="834" height="324" alt="image" src="https://github.com/user-attachments/assets/57d76f24-7d63-4257-9ab4-ecc6f11d8dc1" />


---

## 🔴 3. High Difficulty

*(Awaiting your High level walkthrough)*
