---
publish: true
created: 2026-09-11T12:16:55.919Z
modified: 2026-09-19T14:28:43.689Z
---

# Chocolate

Plataforma: Dockerlabs
OS: Linux
Level: Easy
Status: Done
Complete: Yes
EJPT: yes
Created time: 5 de diciembre de 2024 21:03

Reconeixement

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv  -n 172.17.0.2 -oG allports
```

![image.png](Attachments/images/DockerLabs/image.png)

Mirem codi font i apareix;

![image.png](Attachments/images/DockerLabs/image 1.png)

Entrem a :

<http://172.17.0.2/nibbleblog/>

![image.png](Attachments/images/DockerLabs/image 2.png)

Trobem panell d'autenticació:

![image.png](Attachments/images/DockerLabs/image 3.png)

Credencials per defecte : admin/admin
Trobem versió :

![image.png](Attachments/images/DockerLabs/image 4.png)

Busquem exploit
Provem a metasploit

![image.png](Attachments/images/DockerLabs/image 5.png)

Configurem parametres:

Usuari: admin
Pass: admin
RHOST: 172.17.0.2

Falla
Veiem que al scrip utilitza la ruta del Plugin "My\_image"
Instale el plugin a la web
executem exploit

Entrem i tractem tty:

![image.png](Attachments/images/DockerLabs/image 6.png)

Som usuari www-data

```bash
sudo -l :
```

![image.png](Attachments/images/DockerLabs/image 7.png)

L'usuari "chocolate" pot utilitzar php

Busquem a GTOBins i torbem:

![image.png](Attachments/images/DockerLabs/image 8.png)

Per poder fer-ho amb l'usuari chocolate i que no demani password:

![image.png](Attachments/images/DockerLabs/image 9.png)

Som usuari chocolate

Veiem amb ps -faux que corre un script php com a root

![image.png](Attachments/images/DockerLabs/image 10.png)

/opt/script.php

Modifiquem el fitxer :

echo '<?php exec("chmod u+s /bin/bash"); ?>' > /opt/script.php

Comprovem que ha canviat la bash :

![image.png](Attachments/images/DockerLabs/image 11.png)

amb el permis sudoer

![image.png](Attachments/images/DockerLabs/image 12.png)

Ja amb la bash modificada fem

bash -p
root

![image.png](Attachments/images/DockerLabs/image 13.png)
