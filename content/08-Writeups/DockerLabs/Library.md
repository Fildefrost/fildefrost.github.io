---
publish: true
created: 2026-09-11T12:16:55.920Z
modified: 2026-09-19T14:28:43.773Z
---

# Library

Plataforma: Dockerlabs
OS: Linux
Level: Easy
Status: Done
Complete: Yes
EJPT: yes
Created time: 5 de diciembre de 2024 21:23

## Reconeixement

### NMAP

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv -Pn -n 172.17.0.2 -oG allports
```

![image.png](Attachments/images/DockerLabs/image.png)

```bash
sudo nmap -p22,80 -sCV 172.17.0.2 -oN targeted
```

![image.png](Attachments/images/DockerLabs/image 1.png)

Fem fuzzing a la web:

Trobem :

![image.png](Attachments/images/DockerLabs/image 2.png)

Index.php : JIFGHDS87GYDFIGD

Trobem

/javascrip/jquery/jquery/

Trobem un script que no sembla que serveixi per res

Provem força bruta amb el possible login de la web index.php

```bash
hydra -L /usr/share/wordlists/rockyou.txt -p JIFGHDS87GYDFIGD 172.17.0.2 ssh -vV
```

Pass: carlos

![image.png](Attachments/images/DockerLabs/image 3.png)

Conectem per ssh :

Usuari: carlos
Password: JIFGHDS87GYDFIGD

![image.png](Attachments/images/DockerLabs/image 4.png)

### Explotacio

```bash
sudo -l
```

![image.png](Attachments/images/DockerLabs/image 5.png)

Veiem que crida al script :

/opt/script.py

<https://exploit-notes.hdks.org/exploit/linux/privilege-escalation/python-privilege-escalation/>

Veiem que podem eliminar el script i crear un nou amb la ordre de cridar a una bash:

![image.png](Attachments/images/DockerLabs/image 6.png)

Script original:

![image.png](Attachments/images/DockerLabs/image 7.png)

creem el [script.py](http://script.py/)

![image.png](Attachments/images/DockerLabs/image 8.png)

i executem el scrip amb sudo

![image.png](Attachments/images/DockerLabs/image 9.png)
