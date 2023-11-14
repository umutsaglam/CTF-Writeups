## Giriş

Hoşgeldiniz! Bu yazımda Tryhackme’de bulunan <a href="https://tryhackme.com/room/easyctf">Simple CTF</a> makinesinin çözümünü paylaşacağım.

Başlatalım.

* * *

nmap taraması başlatalım:

```
┌──(root㉿r3tr0)-[~]
└─# nmap -T5 -v -sV -p- 10.10.118.25
```

nmap çıktısı:

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2023-11-14 01:12 EST
NSE: Loaded 46 scripts for scanning.
Initiating Ping Scan at 01:12
Scanning 10.10.118.25 [4 ports]
Completed Ping Scan at 01:12, 0.10s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 01:12
Completed Parallel DNS resolution of 1 host. at 01:12, 0.00s elapsed
Initiating SYN Stealth Scan at 01:12
Scanning 10.10.118.25 (10.10.118.25) [65535 ports]
Discovered open port 80/tcp on 10.10.118.25
Discovered open port 21/tcp on 10.10.118.25
SYN Stealth Scan Timing: About 21.89% done; ETC: 01:15 (0:01:51 remaining)
Discovered open port 2222/tcp on 10.10.118.25
SYN Stealth Scan Timing: About 56.59% done; ETC: 01:14 (0:00:47 remaining)
Completed SYN Stealth Scan at 01:14, 92.02s elapsed (65535 total ports)
Initiating Service scan at 01:14
Scanning 3 services on 10.10.118.25 (10.10.118.25)
Completed Service scan at 01:14, 6.20s elapsed (3 services on 1 host)
NSE: Script scanning 10.10.118.25.
Initiating NSE at 01:14
Completed NSE at 01:14, 0.41s elapsed
Initiating NSE at 01:14
Completed NSE at 01:14, 0.36s elapsed
Nmap scan report for 10.10.118.25 (10.10.118.25)
Host is up (0.089s latency).
Not shown: 65532 filtered tcp ports (no-response)
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.3
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 99.41 seconds
           Raw packets sent: 131138 (5.770MB) | Rcvd: 71 (2.852KB)
```

1000 portunun altında kaç hizmet çalışıyor?

- 2

En yüksek portta hangi hizmet çalışıyor?

- ssh


