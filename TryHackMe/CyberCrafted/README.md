## Giriş

Hoşgeldiniz! Bu yazımda Tryhackme’de bulunan <a href="https://tryhackme.com/room/cybercrafted">CyberCrafted</a> makinesinin çözümünü paylaşacağım.

Geliştirme aşamasındaki bir Minecraft sunucusunun IP adresini buldun. Rootlayabilir misin?

Başlayalım.

## Görev 1 

1.
Kaç tane port açık?

nmap taraması başlatalım.

```
┌──(root㉿r3tr0)-[~]
└─# nmap -T5 -v -sV -p- 10.10.207.243
Starting Nmap 7.94SVN ( https://nmap.org ) at 2023-11-10 23:33 EST
```
nmap çıktısı
```
┌──(root㉿r3tr0)-[~]
└─# nmap -T5 -v -sV -p- 10.10.207.243
Starting Nmap 7.94SVN ( https://nmap.org ) at 2023-11-10 23:33 EST
NSE: Loaded 46 scripts for scanning.
Initiating Ping Scan at 23:33
Scanning 10.10.207.243 [4 ports]
Completed Ping Scan at 23:33, 0.11s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 23:33
Completed Parallel DNS resolution of 1 host. at 23:33, 0.00s elapsed
Initiating SYN Stealth Scan at 23:33
Scanning 10.10.207.243 (10.10.207.243) [65535 ports]
Discovered open port 80/tcp on 10.10.207.243
Discovered open port 22/tcp on 10.10.207.243
SYN Stealth Scan Timing: About 42.52% done; ETC: 23:34 (0:00:42 remaining)
Discovered open port 25565/tcp on 10.10.207.243
Completed SYN Stealth Scan at 23:34, 67.57s elapsed (65535 total ports)
Initiating Service scan at 23:34
Scanning 3 services on 10.10.207.243 (10.10.207.243)
Completed Service scan at 23:34, 6.20s elapsed (3 services on 1 host)
NSE: Script scanning 10.10.207.243.
Initiating NSE at 23:34
Completed NSE at 23:34, 0.43s elapsed
Initiating NSE at 23:34
Completed NSE at 23:34, 0.38s elapsed
Nmap scan report for 10.10.207.243 (10.10.207.243)
Host is up (0.094s latency).
Not shown: 65532 closed tcp ports (reset)
PORT      STATE SERVICE   VERSION
22/tcp    open  ssh       OpenSSH 7.6p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
80/tcp    open  http      Apache httpd 2.4.29 ((Ubuntu))
25565/tcp open  minecraft Minecraft 1.7.2 (Protocol: 127, Message: ck00r lcCyberCraftedr ck00rrck00r e-TryHackMe-r  ck00r, Users: 0/1)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 74.94 seconds
           Raw packets sent: 66803 (2.939MB) | Rcvd: 66600 (2.664MB)
```

Göründüğü gibi 3 tane açık port var 
> Cevap:3

2.
En yüksek portta hangi servis çalışıyor?

>Cevap: minecraft

3.
Herhangi bir alt alan adı (subdomain) var mı? (Alfabetik sırayla)













































