# HackTheBox — Fawn (Starting Point) — Writeup

**Author:** Janhavi Mohite
**Machine:** Fawn (Starting Point)
**Date:** 2025-09-18

---

## Objective

Retrieve the user flag from the target machine `10.129.198.24` using allowed enumeration and exploitation techniques. This was a beginner-friendly starting point box focused on finding an anonymous FTP service and reading a flag file.

---

## Environment

* Attacking host: Kali/Parrot machine inside HTB VPN (example IP: `10.10.15.20`)
* Target host: `10.129.198.24`
* Tools used: `nmap`, built-in `ftp` client, `ping`, standard shell utilities

---

## Summary of steps

1. Performed host discovery and service enumeration with `nmap`.
2. Confirmed the host is reachable via `ping`.
3. Discovered FTP (vsftpd 3.0.3) and tried anonymous login.
4. Listed the FTP directory, downloaded `flag.txt` and read it locally.

---

## Reconnaissance

### Nmap scan

I ran a targeted nmap scan to identify open services and versions:

```bash
nmap -sV -sC -p- 10.129.198.24
```

**Key findings:**

* Port 21/tcp - `ftp` (vsftpd 3.0.3)
* The service advertises `ftp-anon: Anonymous FTP login allowed (FTP code 230)`

(See `nmap.png` for the screenshot of the nmap output.)

---

## Confirm host is up

I verified connectivity with `ping`:

```bash
ping -c 4 10.129.198.24
```

The host responded consistently, confirming it was reachable. (See `ping.png` screenshot.)

---

## Accessing FTP

I used the system `ftp` client to connect and authenticate anonymously.

```bash
ftp 10.129.198.24
# When prompted for username: anonymous
# When prompted for password: <press Enter>
```

Server banner showed `vsFTPd 3.0.3` and allowed anonymous login.

---

## Enumerating FTP

After connecting I switched to binary mode and listed the directory:

```bash
binary
ls
```

Output showed a single file `flag.txt` (32 bytes). I downloaded it with:

```bash
get flag.txt
```

Transfer completed successfully. (See `ftp.png` screenshot for the FTP session and transfer progress.)

---

## Reading the flag

On my local attacking machine I changed into the working directory where the FTP client saved the file and displayed it:

```bash
ls -la
cat flag.txt
```

The file contained the user flag which completes the objective for this machine.

---

## Commands used (compact)

```
nmap -sV -sC -p- 10.129.198.24
ping -c 4 10.129.198.24
ftp 10.129.198.24
# in ftp client:
# username: anonymous
# password: <blank>
binary
ls
get flag.txt
exit
# locally
ls -la
cat flag.txt
```

---

## Observations & Notes

* The FTP server allowed anonymous logins; this is common on intentionally vulnerable starting boxes.
* Both control and data connections for classical FTP are clear-text; credentials and file contents transfer unencrypted.
* No further privilege escalation was required for this box — the objective was to find the flag via anonymous FTP.

---

## Remediation advice (for real systems)

* Disable anonymous FTP unless explicitly required.
* If file transfer is required, prefer secure alternatives such as SFTP (SSH File Transfer Protocol) or FTPS (FTP over TLS).
* Keep FTP server software up to date and restrict reachable directories and file permissions.

---

## References / Credits

* Lab: HackTheBox — Starting Point: Fawn
* Tools: nmap, ftp, ping

---

## Appendix — Screenshots

* `nmap.png` — Nmap service/version discovery and ftp-anon note
* `ping.png` — Ping results showing host is up
* `ftp.png` — FTP session showing anonymous login, file listing and file download

---

