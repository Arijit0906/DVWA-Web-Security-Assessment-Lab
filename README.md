# DVWA Web Security Assessment Lab
<img width="727" height="78" alt="{6BA40572-6A08-41A5-82A9-10D01916F108}" src="https://github.com/user-attachments/assets/0b2e87fe-07f0-4227-802d-4bc7ce1170f9" />

A hands-on web application security assessment performed in an authorized, isolated **Damn Vulnerable Web Application (DVWA)** lab environment using **Kali Linux**.

## Project Overview

This project demonstrates practical web application security testing across **13 vulnerability categories** using Easy, Medium, and Hard DVWA security levels.

The assessment focused on understanding common web vulnerabilities, identifying weaknesses through controlled testing, analyzing application behavior and HTTP requests, and documenting security impact and remediation recommendations.

### Assessment Scope

* **13 web vulnerability categories**
* **Easy, Medium, and Hard** security levels
* Local Kali Linux virtual machine
* Isolated testing environment
* Manual vulnerability testing and analysis
* Burp Suite and Wireshark-assisted analysis
* Security findings and remediation documentation

> **Note:** The Impossible security level is not included in the current assessment scope.

---

## Lab Environment & Setup

The assessment was performed using a **Kali Linux virtual machine** running locally in an isolated lab environment.

DVWA was installed and accessed locally through the Kali VM. No real-world or unauthorized systems were used during testing.

### Environment

| Component        | Details                                     |
| ---------------- | ------------------------------------------- |
| Operating System | Kali Linux                                  |
| Application      | Damn Vulnerable Web Application (DVWA)      |
| Environment      | Local Virtual Machine                       |
| Testing Type     | Authorized Web Application Security Testing |
| Target           | Local DVWA instance                         |
| Testing Scope    | Easy / Medium / Hard                        |

### DVWA Installation

DVWA was installed directly inside the Kali Linux environment using the following commands:

```bash
sudo apt update
sudo apt install dvwa -y
```

The DVWA service was then started with:

```bash
sudo dvwa-start
```

After starting the service, DVWA was accessed locally through:

```text
http://127.0.0.1:42001
```

The default DVWA credentials used to access the application were:

```text
Username: admin
Password: password
```

The required configuration permissions were set using:

```bash
sudo chmod 777 /etc/dvwa/config
```

> **Security Note:** The above credentials are DVWA's default laboratory credentials and are used only for this intentionally vulnerable local application. They are not credentials for any real system.

### Isolated Testing Environment

The complete assessment was conducted within a **local Kali Linux virtual machine** specifically used for security testing.

The testing environment was designed to keep the intentionally vulnerable application separated from real-world systems. All vulnerability testing, request manipulation, and traffic analysis were performed against the local DVWA instance.

---

## Tools & Technologies

* **Kali Linux** — Security testing environment
* **Nmap** — Network and service reconnaissance
* **Burp Suite** — HTTP interception, request analysis, and web security testing
* **Wireshark** — Network traffic analysis
* **DVWA** — Intentionally vulnerable web application
* **Git & GitHub** — Version control and security documentation

---

## Testing Methodology

The assessment followed a structured security testing workflow:

1. **Lab Setup**

   * Installed DVWA inside a local Kali Linux virtual machine.
   * Started the DVWA service and configured the application.
   * Confirmed access to the local DVWA instance.

2. **Reconnaissance**

   * Identified the target application and available services.
   * Used Nmap for controlled service enumeration.

3. **Vulnerability Testing**

   * Tested 13 vulnerability categories.
   * Performed testing at Easy, Medium, and Hard levels.
   * Analyzed application behavior and security controls.
   * Used Burp Suite where HTTP request interception or manipulation was required.

4. **Traffic Analysis**

   * Examined relevant network traffic using Wireshark.
   * Observed HTTP communication and application behavior.

5. **Documentation**

   * Recorded testing methodology and observations.
   * Documented security impact.
   * Provided remediation recommendations.
   * Included supporting screenshots in the final assessment report.
---
## Disclaimer

* This project is intended **only for educational and ethical security testing purposes**.
* All testing was performed in an **isolated local DVWA lab environment**.
* Never test real systems, applications, or networks without **explicit written authorization**.
* The techniques demonstrated in this repository should only be used on systems you own or have permission to assess.

---

## Author

**Arijit Nayak**

B.Tech CSE — IoT, Cyber Security & Blockchain Technology


**LinkedIn:** [Arijit Nayak](https://in.linkedin.com/in/arijit-nayak-69108a255)

