## Giriş

Hoşgeldiniz! Bu yazımda Tryhackme’de bulunan <a href="https://tryhackme.com/room/breakit">RootMe</a> makinesinin çözümünü paylaşacağım.

# Bölüm 1 - Reconnaissance

1.
```
Makineyi tarayın, kaç tane port açık?
```
Öncelikle nmap taraması başlatalım.
```
┌──(root@r3tr0)-[/home/kali/Desktop/ctf/RootMe]
└─# nmap -T5 -p- -v -sV 10.10.155.83     
```

![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/RootMe/images/a1.png?raw=true)

>Göründüğü gibi 22 ve 80 portu açık. Cevap: 2

2.
```
Çalışan Apache versiyonu nedir?
```
>2.4.29

3.
```
22 portunda çalışan servis nedir?
```
> ssh

4.
```
Gizli dizin nedir?
```
Gobuster çalıştırarak gizli dizini bulalım.

```
┌──(root㉿r3tr0)-[/home/kali/Desktop/ctf/RootMe]
└─# gobuster dir --url http://10.10.178.135 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x txt,php,html -t 60
```
![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/RootMe/images/a2.png?raw=true)

> /panel/


# Bölüm 2 - Getting a Shell

1.
```
Bir form yükleyin ve ters shell alın ve bayrağı bulun.
```
Önce bulduğumuz gizli dizine gidelim.

![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/RootMe/images/a3.png?raw=true)

