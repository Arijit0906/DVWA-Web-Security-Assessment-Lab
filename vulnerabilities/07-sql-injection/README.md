# 💉 Module: SQL Injection (SQLi)

## 🟢 1. Low Difficulty

### 🔍 Vulnerability Analysis
* **The Goal:** Extract sensitive backend data (like user hashes) from the database by manipulating the input parameter.
* **The Flaw:** The application accepts input without validation and concatenates it directly into a database command, allowing arbitrary SQL execution.

### 💻 Code Flaw (Source Breakdown)
The application takes raw user input and appends it directly into the SQL string without sanitization:

```php
$id = $_REQUEST[ 'id' ];
$query  = "SELECT first_name, last_name FROM users WHERE user_id = '$id';";
```
Because the input string is executed directly by the database engine, characters like `'` break out of the intended query logic and allow unauthorized data extraction.

### 🛠️ Exploitation Steps
1. Navigated to the **SQL Injection** tab.
2. Tested regular input functionality by entering valid IDs (`1`, `2`, `3`), which successfully printed corresponding usernames on the screen.

  <img width="546" height="149" alt="image" src="https://github.com/user-attachments/assets/a21cb812-cdef-4c2d-869a-45ff05c1d22f" />
4. Verified error output by typing a single quote `'` to break the query syntax.
5. Used a basic Boolean payload to enumerate all users by forcing a true condition:
   ```text
   %' OR '1'='1
   ```
<img width="367" height="338" alt="{56A358BD-8701-4F9A-B559-3174D0D98038}" src="https://github.com/user-attachments/assets/30d852c7-9d93-4d3e-8afb-3e673376e588" />

5. Extracted usernames and password hashes directly from the database table:
   ```text
   %' UNION SELECT user, password FROM users#
   ```
<img width="541" height="345" alt="{C46FA05B-40AD-4E91-8D01-7FC50E2A95E2}" src="https://github.com/user-attachments/assets/fcd52017-57e7-4cde-9a8d-91165e6f95a2" />

---

## 🟡 2. Medium Difficulty

### 🔍 Vulnerability Analysis
* **The Goal:** Bypass defensive functions (`mysqli_real_escape_string`) and parameter restrictions to extract user credentials.
* **The Flaw:** While the developer added code protection to filter out single quotes and switched the input container to a dropdown menu list, the backend SQL query handles the parameter as an unquoted integer field.

### 💻 Code Flaw (Source Breakdown)
The application attempts to neutralize payloads using an escape filtering function, but fails because the variable is not wrapped in quotes inside the query string:

```php
$id = $_POST[ 'id' ];
$id = mysqli_real_escape_string($GLOBALS["___mysqli_ston"], $id);
$query  = "SELECT first_name, last_name FROM users WHERE user_id = $id;";
```
Because `$id` is numerical (`WHERE user_id = $id`), an attacker does not need to inject a single quote (`'`) to break the structure. The payload can be injected as raw numerical text directly after the integer.

### 🛠️ Exploitation Steps
1. Navigated to the lab interface which presents a rigid dropdown element displaying 5 valid users.
2. Selected an option and intercepted the resulting HTTP POST request via **Burp Suite Proxy**.
3. Sent the request to **Burp Repeater** and modified the `id` body parameter to inject a `UNION SELECT` statement without using quotes:
   ```text
   id=1 UNION SELECT user, password FROM users&Submit=Submit
   ```
   <img width="700" height="390" alt="image" src="https://github.com/user-attachments/assets/73a548e2-f826-49f9-ac72-614ecc494997" />

4. Forwarded the request and received a database dump containing usernames alongside MD5 password hashes in the output text fields.
5. Copied the target MD5 hash (`e99a18c428cb38d5f260853678922e03`) and used an online decoder tool to crack the hash, revealing the plaintext credential: `abc123`.
  <img width="586" height="487" alt="image" src="https://github.com/user-attachments/assets/b1491773-06c5-4511-8e04-42d7333551fe" />

6. Successfully authenticated into the portal using the extracted user account (`gordonb` / `abc123`).
<img width="580" height="145" alt="image" src="https://github.com/user-attachments/assets/c51e0ff3-c9dd-45f1-81ca-77b7b3907ef6" />

---

## 🔴 3. High Difficulty

*(Awaiting your High level walkthrough)*
