---
publish: true
created: 2026-09-11T12:16:55.946Z
modified: 2026-09-11T12:24:56.370Z
---

# Preignition

Plataforma: HackTheBox
OS: Linux
Level: Very Easy
Status: Done
Complete: Yes
Created time: 4 de enero de 2025 15:38
IP: 10.129.77.25

## Recopilación de información

<aside>
💡 Maquina linux del Trier 0 HTB

</aside>

### **Escaneo de puertos**

Comenzamos con un escaneo para identificar que puertos están abiertos.

---

```bash
❯ sudo nmap -p- --open --min-rate 5000 -sS -n -Pn -vvv 10.129.77.25 -oG allports

PORT   STATE SERVICE REASON
80/tcp open  http    syn-ack ttl 63

```

### **Enumeración de servicios**

Una vez listado los puertos accesibles, procederemos a realizar la enumeración de servicios para su posterior identificación de vulnerabilidades.

---

```bash
❯ sudo nmap -p80 -sCV 10.129.77.25 -oN targeted
```

- **Identificación de vulnerabilidades**
  - 80/tcp open  http    nginx 1.14.2

    ```bash
    PORT   STATE SERVICE VERSION
    80/tcp open  http    nginx 1.14.2
    |_http-title: Welcome to nginx!
    |_http-server-header: nginx/1.14.2
    ```

Enumeramos la web

```bash
whatweb 10.129.77.25

whatweb 10.129.77.25
http://10.129.77.25 [200 OK] Country[RESERVED][ZZ], HTML5, HTTPServer[nginx/1.14.2], IP[10.129.77.25], Title[Welcome to nginx!], nginx[1.14.2]
```

![image.png](/images/HackTheBox/image.png)

Fuzzing

```bash
❯ gobuster dir -u http://10.129.77.25/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt -x php

===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/admin.php            (Status: 200) [Size: 999]

❯ sudo nmap --script=http-enum -sV -p80 10.129.77.25
PORT   STATE SERVICE VERSION
80/tcp open  http    nginx 1.14.2
|_http-server-header: nginx/1.14.2
| http-enum: 
|_  /admin.php: Possible admin folder
```

Result: admin.php

![image.png](/images/HackTheBox/image%201.png)

Probamos con credenciales por defecto

Admin / admin

![image.png](/images/HackTheBox/image%202.png)

Flag: 6483bee07c1c1d57f14e5b0717503c73

## Explotación

<aside>
💡 No hay explotacion

</aside>

###

## Conclusión

<aside>
💡 En esta sección, debes proporcionar un resumen de la máquina para cuando tengas que volver a ella, puedas saber conocer de forma rápida de que se trataba

</aside>

Maquina muy facil , sin escalada de privilegios
