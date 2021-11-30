---
title: Primeras Maquinas HTB
published: true
---

* * *

<br/>

### PRIMEROS PASOS . . .

Primer día que termino unas máquinas de **HTB**. La parte de credenciales es bastante sencilla, siempre es logearse con root y sin contraseña.
Se centra más en conocer los servicios más comunes, analisis de puertos y saber que te encuentras.
Me encontré con vulnerabilidades en telnet, ssh, ftp, smb, y sql. A veces la gente pone cualquier contraseña y suponen que nunca va a pasar nada.

<br>
<br/>

### HERRAMIENTAS ÚTILES:


**Ping**: mitica para saber si conectamos con otra máquina. El ttl nos dice si la máquina _victima_ es Linux(~64) o Windows(~128).

**Nmap**: para rastrear los puertos. Una de las más utilizadas sin duda. Tiene miles de paramétros. Yo he usados estos: `nmap -p- --open -T5 -v -n {IP}` o `nmap -sC -sV -n {IP}`.

**whatweb**: Nos da información sobre la página web. Esta muy interesante: `whatweb http://{IP} -v`

**wfuzz**: herramienta de fuzzing para webs. Un tipo de fuerza bruta para sacar directorios: `wfuzz -c --hc=404 -t 200 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt http://{IP}`

**gobuster**: parecido a wfuzz. `gobuster dir --url http://{IP} --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`

