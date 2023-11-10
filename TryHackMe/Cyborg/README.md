## Giriş

Hoşgeldiniz! Bu yazımda Tryhackme’de bulunan <a href="https://tryhackme.com/room/cyborgt8">Cyborg</a> makinesinin çözümünü paylaşacağım.

Şifrelenmiş arşivleri, kaynak kodu analizini ve daha fazlasını içeren bir ctf.

Başlayalım.

# Görev 1 Compromise the System 

Her zamanki gibi nmap taramasıyla başlıyoruz.

>nmap -A -T5 -sV -p- $ip

nmap çıktısı şu şekilde:
```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 dbb270f307ac32003f81b8d03a89f365 (RSA)
|   256 68e6852f69655be7c6312c8e4167d7ba (ECDSA)
|_  256 562c7992ca23c3914935fadd697ccaab (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

1.
Makineyi tarayın. Kaç port açık?
>Cevap:2

2.
22 portunda hangi servis çalışıyor?
>Cevap:ssh

3.
80 portunda hangi servis çalışıyor?
>Cevap:http

4.
user.txt bayrağını bulunuz.

Hedefin websitesine gidelim.

![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/Cyborg/images/a1.png?raw=true)

Karşımıza Apache2 Ubuntu Default Page geldi. Bu sayfadan bişey çıkaramayız.

Gizli dizinleri görmek için gobuster çalıştıralım.

>gobuster dir -u http://$ip -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

```
Gobuster v3.5
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.218.26
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.5
[+] Timeout:                 10s
===============================================================
2023/03/27 16:14:52 Starting gobuster in directory enumeration mode
===============================================================
/admin                (Status: 301) [Size: 312] [--> http://10.10.218.26/admin/]
/etc                  (Status: 301) [Size: 310] [--> http://10.10.218.26/etc/]
Progress: 220326 / 220561 (99.89%)
===============================================================
2023/03/27 16:27:14 Finished
===============================================================
```

Gobuster çıktısı bu şekilde. admin ve etc dizinini inceleyelim.

![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/Cyborg/images/a2.png?raw=true)

Buradaki kullanıcı isimlerini not edelim: Josh, Adam ve Alex. İleride işimize yarayabilirler. Başka neler işimize yarayabilir? Alex, mesajında bazı işleri mahvettiğini söylüyor. Web sitesini daha güvenli hale getirmek için ne yapacağını bilmediği için beklemeye karar vermiş ve yapılandırma dosyalarının herkesin erişebildiğinden bahsediyor. Ayrıca, “music_archive” adlı bir arşivinden bahsediyor.

Yani, web sitesi pek güvenli değil. Yapılandırma dosyasına erişebiliriz ve “music_archive” adlı arşivi de onun için önemli görünüyor.

Web sitesini keşfetmeye devam edelim.

“Archive-Download” kısmında bize “archive.tar” dosyasını indiriyor.

Dosyayı incelemeden once /etc alt dizin adresimizide bir ziyaret edelim.


![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/Cyborg/images/a3.png?raw=true)



