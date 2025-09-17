# HTB — Meow (Starting Point)

**Author:** Janhavi Mohite  
**Date:**  20/03/2005 
**Difficulty:** Starting Point

---

## 📝 Summary
Gained initial foothold via a web vector (file upload / RCE / vulnerable endpoint), then escalated to root via local misconfig/credentials.  

---

## 🎯 Target
- **IP Address:** <IP>
- **Top services:** (example) `80/tcp (http)`, `22/tcp (ssh)`

---

## 🔍 Reconnaissance
Commands I ran:

Key findings:
- Web endpoints: `/`, `/upload`, `/admin` (fill what you saw)
- Any suspicious headers or versions: `<note here>`

---

## 💥 Exploitation (Initial foothold)
Steps I followed (recreate or describe from memory):
1. Found an upload point at `/upload`. Tested allowed extensions and size limits.
2. Bypassed extension check by renaming `shell.php` to `shell.php7` or using polyglot.  
   Example exploit commands:

3. Upgraded the shell (if applicable):

Basic enumeration:

Escalation path (explain what worked for you):
- e.g., `sudo -l` showed `NOPASSWD` for `/usr/bin/somebinary` → exploited to get root.
- OR found credentials in `/home/<user>/notes.txt` → used to `su`/`ssh`.

Proof (redacted):
- `user.txt` → `REDACTED{user_flag}`
- `root.txt` → `REDACTED{root_flag}`

---

## 🛡️ Mitigation
- Disable insecure file uploads or validate file contents server-side.  
- Fix `sudo` misconfig / rotate leaked credentials / remove SUID where not needed.

---

## 📚 References
- Any blog, PortSwigger, or exploit-db entries you used (paste links).
