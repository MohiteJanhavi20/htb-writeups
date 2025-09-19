HackTheBox — Dancing (Starting Point) — Writeup

Author: Janhavi Mohite
Machine: Dancing (Starting Point)
Date: 2025-09-20
IP - 10.129.132.65

Objective

Retrieve the user flag from the target machine <TARGET_IP> using allowed enumeration and exploitation techniques. This was a beginner-friendly starting point focused on SMB enumeration and anonymous access.

Environment

Attacking host: Kali/Parrot machine inside HTB VPN (example IP: 10.10.15.20)

Target host: <TARGET_IP>

Tools used: nmap, smbclient, smbmap (optional), standard shell utilities (cat, ls)

Summary of steps

Performed service enumeration with nmap.

Enumerated SMB shares with smbclient -L.

Connected anonymously to the workshare share.

Navigated directories, downloaded flag.txt using get, and read the flag locally.

Reconnaissance
Nmap scan

I ran targeted nmap scans to find open SMB ports and services:

# quick service scan (only SMB related ports)
nmap -sV -p 139,445 <TARGET_IP>

# or a more aggressive scan for full service/version & scripts
nmap -sV -sC -p- <TARGET_IP>


Key findings:

Port 445/tcp — microsoft-ds (SMB) often shows up in nmap -sV.

(If present) NetBIOS ports 137–139 may also be seen on older systems.

(See nmap.png for a screenshot of the nmap output if you captured one.)

Enumerating SMB

List available shares (try anonymous access first):

smbclient -L //<TARGET_IP> -N


-L lists shares on the host.

-N tells smbclient not to prompt for a password (attempt anonymous).

From the output I found a share named workshare (or workshares depending on output). I connected to it anonymously:

smbclient //<TARGET_IP>/workshare -N


After connection I saw two directories inside the share. One of those directories contained flag.txt.

Downloading the flag

At the smb: prompt:

smb: \> ls
smb: \> cd <directory-containing-flag>
smb: \> get flag.txt
smb: \> exit


The get command downloads files from the SMB share to your local working directory.

Reading the flag

On the attacking machine:

ls -la
cat flag.txt


The flag printed to the terminal and completes the user objective.

Commands used (compact)
nmap -sV -p 139,445 <TARGET_IP>
smbclient -L //<TARGET_IP> -N
smbclient //<TARGET_IP>/workshare -N
# inside smbclient:
#    ls
#    cd <dir>
#    get flag.txt
#    exit
# locally
ls -la
cat flag.txt


If you prefer a quick share-permission view:

smbmap -H <TARGET_IP>

Observations & Notes

SMB (Server Message Block) is the service used for Windows file sharing — most modern SMB runs over TCP 445.

smbclient -L //<IP> -N is very useful to quickly see which shares allow anonymous access.

The get command in the SMB shell downloads files; put uploads.

Always ensure you only test machines you are authorized to (HTB/CTF boxes are safe to practice on).
