# Hack the Box Labs

Most beginning machines will not have a write up as they are very straightforward and don't require much imagination. Instead I will update general methodology, tools, references, and lessons learned.

As the machines get more difficult, the writeups will be found in this folder by name of HTB machine.

## Machine's Pwned

| Machine | Date | General Concepts |
| -- | -- | -- |
| Meow | 01.19.2025 | telnet |
| Fawn | 01.19.2025 | ftp |
| Dancing | 01.19.2025 | smb |
| Appointment | 01.19.2025 | sql injection |

## Simple Methodology 

Since these are "Very Easy" boxes, there is not much to the methodology

1. `nmap -sV {target_ip}`
"Probe open ports to determine service/version info" [from man pages](https://linux.die.net/man/1/nmap)
2. Search online about open ports and found services
3. Try default or anonymous credentials

## Basic Service Reference

| Service | Default Port | Connection CMD | Reference |
| -- | -- | -- | -- |
| Telnet | 23 | telnet {target-ip} | [telnet hackviser reference](https://hackviser.com/tactics/pentesting/services/telnet) | 
| FTP | 21  | ftp {target-ip} | [ftp hackviser reference](https://hackviser.com/tactics/pentesting/services/ftp) |
| SMB | 139, 445 | smbclient //{target-ip} /{share} | [smb hackviser reference](https://hackviser.com/tactics/pentesting/services/smb)


