---
publish: true
created: 2026-09-11T12:16:55.937Z
modified: 2026-09-19T14:28:45.218Z
---

# Ignition

Plataforma: HackTheBox
OS: Linux
Level: Very Easy
Status: Done
Complete: Yes
Created time: 5 de enero de 2025 14:01
IP: 10.129.1.27

## Recopilación de información

<aside>
💡

</aside>

### **Escaneo de puertos**

Comenzamos con un escaneo para identificar que puertos están abiertos.

---

```bash
❯ sudo nmap -p- --open --min-rate 5000 -sS -n -Pn -vvv 10.129.1.27 -oG allports

PORT   STATE SERVICE REASON
80/tcp open  http    syn-ack ttl 63
```

### **Enumeración de servicios**

Una vez listado los puertos accesibles, procederemos a realizar la enumeración de servicios para su posterior identificación de vulnerabilidades.

---

```bash
❯ sudo nmap -p80 -sCV 10.129.1.27 -oN targeed

PORT   STATE SERVICE VERSION
80/tcp open  http    nginx 1.14.2
|_http-title: Did not follow redirect to http://ignition.htb/
|_http-server-header: nginx/1.14.2
```

- **Identificación de vulnerabilidades**
  - 80/tcp open  http    nginx 1.14.2

**Enumeracion web**

Añadimos el host iginition.htb

```bash
sudo nano /etc/hosts
```

Visitamos el sitio

![image.png](Attachments/images/HackTheBox/image.png)

Scripts nmap

```bash
❯ sudo nmap --script=http-enum -sV -p80 10.129.1.27

PORT   STATE SERVICE VERSION
80/tcp open  http    nginx 1.14.2
|_http-server-header: nginx/1.14.2
| http-enum: 
|   /setup/: Potentially interesting folder
|_  /soap/: Potentially interesting folder

```

```bash
whatweb 10.129.1.27

http://10.129.1.27 [302 Found] Cookies[PHPSESSID], Country[RESERVED][ZZ], HTTPServer[nginx/1.14.2], HttpOnly[PHPSESSID], IP[10.129.1.27], RedirectLocation[http://ignition.htb/], UncommonHeaders[content-security-policy-report-only,x-content-type-options], X-Frame-Options[SAMEORIGIN], X-XSS-Protection[1; mode=block], nginx[1.14.2]
http://ignition.htb/ [200 OK] Cookies[PHPSESSID], Country[RESERVED][ZZ], HTML5, HTTPServer[nginx/1.14.2], HttpOnly[PHPSESSID], IP[10.129.1.27], Magento, Script[text&#x2F;javascript,text/javascript,text/x-magento-init], Title[Home page], UncommonHeaders[content-security-policy-report-only,x-content-type-options], X-Frame-Options[SAMEORIGIN], X-XSS-Protection[1; mode=block], nginx[1.14.2]
```

Fuzzing

```bash
❯ gobuster dir -u http://ignition.htb/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x php,txt,html

Starting gobuster in directory enumeration mode
===============================================================
/index.php            (Status: 200) [Size: 25815]
/contact              (Status: 200) [Size: 28673]
/home                 (Status: 200) [Size: 25802]
/media                (Status: 301) [Size: 185] [--> http://ignition.htb/media/]
/0                    (Status: 200) [Size: 25803]
/catalog              (Status: 302) [Size: 0] [--> http://ignition.htb/]
/admin                (Status: 200) [Size: 7092]
/static               (Status: 301) [Size: 185] [--> http://ignition.htb/static/]
/Home                 (Status: 301) [Size: 0] [--> http://ignition.htb/home]
/cms                  (Status: 200) [Size: 25817]
/checkout             (Status: 302) [Size: 0] [--> http://ignition.htb/checkout/cart/]
/robots.txt           (Status: 200) [Size: 1]
/robots               (Status: 200) [Size: 1]
/setup                (Status: 301) [Size: 185] [--> http://ignition.htb/setup/]
/wishlist             (Status: 302) [Size: 0] [--> http://ignition.htb/customer/account/login/referer/aHR0cDovL2lnbml0aW9uLmh0Yi93aXNobGlzdA%2C%2C/]
/soap                 (Status: 200) [Size: 391]
/rest                 (Status: 400) [Size: 52]
/errors               (Status: 301) [Size: 185] [--> http://ignition.htb/errors/]
/opt                  (Status: 301) [Size: 185] [--> http://ignition.htb/opt/]
```

Enumeramos /admin

![image.png](Attachments/images/HackTheBox/image 1.png)

En la pagina /setup encontramos la versión:

Magento Version dev-2.4-develop

Usamos un script en python para buscar la contraseña de admin del panel de login:

```bash
❯ python magentoBf.py http://ignition.htb/admin -u admin -w /usr/share/seclists/Passwords/xato-net-10-million-passwords-10000.txt

[V] To Check: admin: qwerty123          
Ctrl-c was pressed. Do you really want to exit? y/n 

```

User: admin

Password: qwerty123

Nota: Las pistas de HTBox decian que se podia hacer una busqueda de los passwords mas usados en 2024. Buscandolo en google encontramos (qwerty123)

![image.png](Attachments/images/HackTheBox/image 2.png)

## Explotación

<aside>
💡 No hay explotación ni escalada de privilegios

</aside>

## Conclusión

<aside>
💡 Maquina fácil. Tener presente leer todos los paneles (dashboard)  y home pages, ya que no he visto la flag del dashboard y he perdido mucho tiempo buscando maneras de acceder.

</aside>
