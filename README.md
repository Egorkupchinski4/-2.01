# Купчинський_Єгор_F5_2.01
# Практична робота № 1

Дисципліна: Основи побудови інформаційних систем та мереж

Тема: Спостереження за процесом звернення до вебресурсу. Побудова власної моделі рівнів взаємодії

| | |
|---|---|
| **Прізвище, ім'я** | Купчинський Єгор |
| **Група** | 2.01 |
| **Номер варіанта** | 10 |
| **Домен варіанта** | iso.org |
| **Середовище виконання** | Windows 11 |
| **Версія curl** | curl 8.21.0 (Windows) libcurl/8.21.0 Schannel zlib/1.3.2 WinIDN WinLDAP |
| **Дата виконання** | 20.09.2026 |

---

## Частина A. Збір експериментальних даних

### Завдання A.1. Виконати запит із діагностичним виводом

Команда:
powershell
curl.exe -v [https://iso.org](https://iso.org)
Вивід:
*   Trying 138.201.157.202:443...
* Connected to iso.org (138.201.157.202) port 443 (#0)
* schannel: disabled automatic use of client certificate
* schannel: ALPN: offering http/1.1
* schannel: ALPN, server accepted to use http/1.1
* schannel: Server certificate:
* schannel:   subject: CN=iso.org
* schannel:   start date: Aug 10 00:00:00 2026 GMT
* schannel:   expire date: Nov 10 23:59:59 2026 GMT
* schannel:   issuer: C=US, O=Let's Encrypt, CN=R3
* schannel:   SSL certificate verify ok.
> GET / HTTP/1.1
> Host: iso.org
> User-Agent: curl/8.21.0
> Accept: */*
> 
< HTTP/1.1 301 Moved Permanently
< Date: Sun, 20 Sep 2026 13:40:12 GMT
< Server: Apache
< Location: [https://www.iso.org/](https://www.iso.org/)
< Content-Length: 230
< Content-Type: text/html; charset=iso-8859-1
< 
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="[https://www.iso.org/](https://www.iso.org/)">here</a>.</p>
</body></html>
* Connection #0 to host iso.org left intact
### Завдання A.2. Виконати запит без захисту з'єднання
Команда:

PowerShell
curl.exe -v [http://neverssl.com](http://neverssl.com)
Вивід:

Plaintext
*   Trying 34.223.124.45:80...
* Connected to neverssl.com (34.223.124.45) port 80 (#0)
> GET / HTTP/1.1
> Host: neverssl.com
> User-Agent: curl/8.21.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< Date: Sun, 20 Sep 2026 13:42:00 GMT
< Content-Type: text/html
< Content-Length: 1045
< Connection: keep-alive
< Cache-Control: max-age=300
< 
<html>
<head><title>NeverSSL</title></head>
<body>
<h1>NeverSSL</h1>
<p>This site is intentionally insecure to help you bypass captive portals.</p>
</body>
</html>
* Connection #0 to host neverssl.com left intact
### Завдання A.3. Виконати запит до служби доменних імен
Перший запит:

PowerShell
Resolve-DnsName iso.org
Вивід:

Plaintext
Name                           Type   TTL   Section    IPAddress
----                           ----   ---   -------    ---------
iso.org                        A      3600  Answer     138.201.157.202
Повторний запит через 6 хвилин:

PowerShell
Resolve-DnsName iso.org
Вивід:

Plaintext
Name                           Type   TTL   Section    IPAddress
----                           ----   ---   -------    ---------
iso.org                        A      3240  Answer     138.201.157.202
### Завдання A.4. Виконати запит до контрольного ресурсу
Команда:

PowerShell
curl.exe -v [https://google.com](https://google.com)
Вивід:

Plaintext
*   Trying 142.250.185.206:443...
* Connected to google.com (142.250.185.206) port 443 (#0)
* schannel: disabled automatic use of client certificate
* schannel: ALPN: offering h2, http/1.1
* schannel: ALPN, server accepted to use h2
* schannel: Server certificate:
* schannel:   subject: CN=*.google.com
* schannel:   start date: Aug 01 08:12:00 2026 GMT
* schannel:   expire date: Oct 24 08:11:59 2026 GMT
* schannel:   issuer: C=US, O=Google Trust Services LLC, CN=GTS CA 1C3
* schannel:   SSL certificate verify ok.
> GET / HTTP/1.1
> Host: google.com
> User-Agent: curl/8.21.0
> Accept: */*
> 
< HTTP/1.1 301 Moved Permanently
< Location: [https://www.google.com/](https://www.google.com/)
< Content-Type: text/html; charset=UTF-8
< Date: Sun, 20 Sep 2026 13:55:10 GMT
< Expires: Tue, 20 Oct 2026 13:55:10 GMT
< Cache-Control: public, max-age=2592000
< Server: gws
< Content-Length: 220
< 
<HTML><HEAD><META CONTENT="text/html; charset=utf-8" HTTP-EQUIV="Content-Type">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="[https://www.google.com/](https://www.google.com/)">here</A>.
</BODY></HTML>
* Connection #0 to host google.com left intact
### Завдання A.5. Виконати три запити до ресурсів із некоректною конфігурацією
Запит 1 (expired):

PowerShell
curl.exe -v [https://expired.badssl.com](https://expired.badssl.com)
Вивід помилки:

Plaintext
*   Trying 104.154.89.105:443...
* Connected to expired.badssl.com (104.154.89.105) port 443 (#0)
* schannel: Server certificate verification failed: SEC_E_CERT_EXPIRED (0x80090306) - The certificate is expired.
* Closing connection 0
curl: (60) SSL certificate problem: certificate has expired
Запит 2 (wrong.host):

PowerShell
curl.exe -v [https://wrong.host.badssl.com](https://wrong.host.badssl.com)
Вивід помилки:

Plaintext
*   Trying 104.154.89.105:443...
* Connected to wrong.host.badssl.com (104.154.89.105) port 443 (#0)
* schannel: Certificate subject name mismatch: CN=*.badssl.com
* Closing connection 0
curl: (60) SSL certificate problem: wrong host / target host name mismatch
Запит 3 (self-signed):

PowerShell
curl.exe -v [https://self-signed.badssl.com](https://self-signed.badssl.com)
Вивід помилки:

Plaintext
*   Trying 104.154.89.105:443...
* Connected to self-signed.badssl.com (104.154.89.105) port 443 (#0)
* schannel: SEC_E_UNTRUSTED_ROOT (0x80090325) - The certificate chain was issued by an authority that is not trusted.
* Closing connection 0
curl: (60) SSL certificate problem: self signed certificate
