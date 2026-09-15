# 💻 Module: Command Injection

## 🟢 1. Low Difficulty

### 🔍 Vulnerability Analysis
* **The Goal:** Execute unauthorized operating system commands via an application input field intended only for network diagnostics.
* **The Flaw:** The application blindly appends user input directly into a shell execution function without sanitization, allowlisting, or input filtering.

### 💻 Code Flaw (Source Breakdown)
The application handles user input unsafely as seen in the source code snippet below:

```php
$target = $_REQUEST[ 'ip' ];

// ... (OS Determination logic) ...
$cmd = shell_exec( 'ping -c 4 ' . $target );
echo "<pre>{$cmd}</pre>";
```
The variable `$target` holds the raw user input. The application takes that input and executes it sequentially inside `shell_exec()` using the `ping -c 4` template, then the string wrapper displays the terminal value of the variable `$cmd`. 

Because there is zero data sanitization, an attacker can append and execute arbitrary terminal commands within this user prompt by simply utilizing standard shell command operators like `;`, `&&`, `||`, or `|`.

### 🛠️ Exploitation Steps
1. Navigated to the **Command Injection** panel.
2. Inputted a standard target IP (`127.0.0.1`) to verify normal utility execution.
<img width="407" height="284" alt="image" src="https://github.com/user-attachments/assets/2d1ee5fe-1c85-4162-8d37-f7a1c72c5495" />

3. Exploited the lack of character filtering by injecting a semicolon `;` command separator followed by a secondary malicious command to dump the system database accounts:
   ```text
   127.0.0.1; cat /etc/passwd
   ```
<img width="331" height="374" alt="image" src="https://github.com/user-attachments/assets/dbe46b74-0964-4a3a-a3cc-c685da27736b" />

## 🟡 2. Medium Difficulty

### 🔍 Vulnerability Analysis
* **The Goal:** Bypass the blocklist filter implemented by the developer to achieve remote code execution.
* **The Flaw:** The developer attempts to sanitize input by blocklisting specific command separators. However, because a blacklist approach is used instead of strict validation (allowlisting), several valid shell operators are completely left out.

### 💻 Code Flaw (Source Breakdown)
The application defines a limited blocklist array to swap out characters using `str_replace()`:

```php
// Set blacklist
$substitutions = array(
    '&&' => '',
    ';'  => '',
);

// Remove any of the characters in the array (blacklist).
$target = str_replace( array_keys( $substitutions ), $substitutions, $target );
```
While this successfully filters out double ampersands (`&&`) and semicolons (`;`), it fails because the blacklist is incomplete. It does not account for other powerful terminal execution operators like the single ampersand (`&`), double pipe (`||`), or single pipe (`|`). 

### 🛠️ Exploitation Steps
1. Opened the **Command Injection** module on Medium security mode.
2. Attempted to use the previous payload (`127.0.0.1; cat /etc/passwd`), which failed because the semicolon was stripped out by the backend code.
3. Crafted an alternate payload using the double-pipe operator (`||`). In Linux, the `||` operator acts as a logical OR, meaning if the first command fails or finishes, it runs the second command.
4. Inputted the payload directly into the target execution prompt:
   ```text
   || cat /etc/passwd
   ```
5. **Result:** The system processed the input, bypassed the incomplete `str_replace()` filter entirely, and successfully dumped the system's password file.

<img width="551" height="345" alt="image" src="https://github.com/user-attachments/assets/a83ad253-676c-4fae-aa54-5355b39bd31c" />


## 🔴 3. High Difficulty
5. **Result:** The server completed the initial ping sequence and immediately executed the trailing `cat` command, dumping the entire list of system users.



