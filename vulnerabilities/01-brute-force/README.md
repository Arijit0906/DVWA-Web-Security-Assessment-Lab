# 🔓 Module: Brute Force

## 🟢 1. Low Difficulty

### 🔍 Vulnerability Analysis
* **The Flaw:** The login form lacks rate-limiting or account lockouts, allowing infinite login attempts.
* **The Bypass:** It is also vulnerable to **SQL Injection**. Unsanitized inputs allow an attacker to bypass password verification entirely using logical operators.

### 🛠️ Exploitation Steps
1. Opened the **Brute Force** tab.
2. Entered this SQL payload into the username field: `admin' or '1'='1'#`
3. Typed a random string in the password field and clicked **Login**.
4. **Result:** The system evaluated the statement as true and granted admin access.
<img width="767" height="482" alt="image" src="https://github.com/user-attachments/assets/83fe89e4-6ffe-4115-9d62-861f0b6143d9" />

---

## 🟡 2. Medium Difficulty

### 🔍 Vulnerability Analysis
* **The Flaw:** The application patches the SQL injection flaw but still fails to implement login rate-limiting, CAPTCHA, or delay mechanics. 
* **The Technique:** Because security is at medium, we can use an automated dictionary attack via an HTTP proxy tool to test multiple credential sets simultaneously.

### 🛠️ Exploitation Steps
1. Intercepted a login request using **Burp Suite Proxy** and sent it to **Intruder**.
2. Set the attack type to **Cluster Bomb** and highlighted the `username` and `password` parameters as payload positions.
3. Configured **Payload 1** (usernames) with a custom wordlist containing names like `admin`.
4. Configured **Payload 2** (passwords) with a custom wordlist containing common choices like `password`.
5. Executed the attack and sorted the results window by **Length**.
6. **Result:** Request #11 (`admin` / `password`) returned a unique response length of **5082 bytes**, confirming the correct combination.
<img width="480" height="251" alt="image" src="https://github.com/user-attachments/assets/c47f538e-15ff-4896-b221-b4f572f3240c" />
<img width="725" height="420" alt="{0F33D8BB-4BD7-4B3E-8531-FC2E37ADCFC5}" src="https://github.com/user-attachments/assets/b5b86f01-5059-4f7d-8c50-ec3cacff106a" />
<img width="586" height="239" alt="image" src="https://github.com/user-attachments/assets/8224a102-d20d-4f28-9ff1-05f7a21f27c4" />

---

## 🔴 3. High Difficulty

*(Awaiting your screenshot/details to complete this section)*
