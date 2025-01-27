# Hack the Box Labs

Most beginning machines will not have a write up as they are very straightforward and don't require much imagination. Instead I will update general methodology, tools, references, and lessons learned.

As the machines get more difficult, the writeups will be found in this folder by name of HTB machine.

## Machine's Pwned

| Machine | Date | General Concepts | Difficulty |
| -- | -- | -- | -- |
| Meow | 01.18.2025 | telnet | very easy |
| Fawn | 01.18.2025 | ftp | very easy |
| Dancing | 01.18.2025 | smb | very easy |
| Appointment | 01.18.2025 | sql injection | very easy |
| Redeemer | 01.19.2025 | redis | very easy |
| Sequel | 01.19.2025 | mysql, mariaDB | very easy |
| Crocodile | 01.19.2025 | ftp, gobuster | very easy |
| Responder | 01.19.2025 | php, RCE, responder, winrm | very easy |
| Cap | 01.24.2025 | IDOR, linpeas | retired easy | 
| -- | -- | -- | -- |

## Simple Methodology 

Since these are "Very Easy" boxes, there is not much to the methodology

1. `nmap -sC -sV {target_ip}`
- -sV : "Probe open ports to determine service/version info" [from man pages](https://linux.die.net/man/1/nmap)
- -sC : "Performs a script scan using the default set of scripts"  [from man pages](https://linux.die.net/man/1/nmap)
2. Search online about open ports and found services
3. Try default or anonymous credentials
4. If it is a http, try accessing from the web
 - Use `echo "{target-ip} {hostname}" | sudo tee -a /etc/hosts` if it reroutes to a hostname and cannot load.
 - Try enumerating the webpage with `Gobuster` or another fuzzer
 - Check for login pages to exploit from known credentials or sql injection attacks

## Basic Tool Reference

**NMAP**
- network scanning tool
- [GeeksforGeeks cheatsheet](https://www.geeksforgeeks.org/top-30-basic-nmap-commands-for-beginners/)

**GoBuster**
- web fuzzer
- [freeCodeCamp tutorial](https://www.freecodecamp.org/news/gobuster-tutorial-find-hidden-directories-sub-domains-and-s3-buckets/)

**SecList**
- wordlists - like a lot of wordlists

**Responder**
- exploit DNS failures
- [NotSoSecure's guide](https://notsosecure.com/pwning-with-responder-a-pentesters-guide)

**John the Ripper*
- hash cracker
- `john -w=<wordlist.txt> <hashfile>`

**Evil-WinRM**
- windows remote mangament shell for hacking
- [Evil-winrm github](https://github.com/Hackplayers/evil-winrm)

**LinPEAS**
- identify privilege escalation opportunities for Linux 
- 

## Basic Service Reference

| Service | Default Port | Connection CMD | Reference |
| -- | -- | -- | -- |
| Telnet | 23 | telnet {target-ip} | [telnet hackviser reference](https://hackviser.com/tactics/pentesting/services/telnet) | 
| FTP | 21  | ftp {target-ip} | [ftp hackviser reference](https://hackviser.com/tactics/pentesting/services/ftp) |
| SMB | 139, 445 | smbclient //{target-ip} /{share} | [smb hackviser reference](https://hackviser.com/tactics/pentesting/services/smb)

