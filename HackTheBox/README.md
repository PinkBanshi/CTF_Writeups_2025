# Hack the Box Labs


Instead of writing full writeups for each machine, but general methodology, tools, references, and lessons learned.

## Machine's Pwned

| Machine | Date | General Concepts | Difficulty |
| -- | -- | -- | -- |
| Meow | 01.18.2025 | telnet | very easy |
| Fawn | 01.18.2025 | ftp | very easy |
| Dancing | 01.18.2025 | smb | very easy |
| Appointment | 01.18.2025 | sql injection | very easy |
| Redeemer | 01.19.2025 | redis | very easy |
| Sequel | 01.19.2025 | mysql/ mariaDB | very easy |
| Crocodile | 01.19.2025 | ftp/gobuster | very easy |
| Responder | 01.19.2025 | php/RCE/responder/winrm | very easy |
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
- TBD

**GoBuster**
- web fuzzer
- [freeCodeCamp tutorial](https://www.freecodecamp.org/news/gobuster-tutorial-find-hidden-directories-sub-domains-and-s3-buckets/)

**SecList**
- wordlists - like a lot of wordlists

**Responder**
- TBD

**John the Ripper*
- hash cracker

**Evil-WinRM**
- windows remote mangament shell for hacking
- [Evil-winrm github](https://github.com/Hackplayers/evil-winrm)

## Basic Service Reference

| Service | Default Port | Connection CMD | Reference |
| -- | -- | -- | -- |
| Telnet | 23 | telnet {target-ip} | [telnet hackviser reference](https://hackviser.com/tactics/pentesting/services/telnet) | 
| FTP | 21  | ftp {target-ip} | [ftp hackviser reference](https://hackviser.com/tactics/pentesting/services/ftp) |
| SMB | 139, 445 | smbclient //{target-ip} /{share} | [smb hackviser reference](https://hackviser.com/tactics/pentesting/services/smb)


