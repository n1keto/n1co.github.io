---
title: Curso Mastermind 1
published: true
---

* * *

<br/>

## CURSO DE MASTERMIND PRIMERA PARTE: Conceptos básicos.

### 1. Permisos en Linux.

drwxr-xr-x n1k3t0 n1k3t0 90 B Fri Nov 19 13:00:34 2021  fondos

d -> Directorio

rwx rwx rwx
 P   G   O
r -> read
w -> write
x -> f:executable | d: atravesar (cd directorio)

<br>

<br/>
### 2. Explotación de permisos SUID.

Si hago, siendo root, un **which find | xargs ls -l** me muestra los permisos que tengo sobre dicho comando.
No tiene permisos SUID por lo que hacemos **chmod 4755 /usr/bin/find**. Compruebo los permisos ahora y sale esto:
`-rwsr-xr-x 1 root root 311008 jan  9  2021 /usr/bin/find`
Visito la página _https://gtfobins.github.io_ que ayuda mucho para explotar y escalar privilegios.
Para el comando find, puedo ser root sin proporcionar contraseña con el comando `find . -exec /bin/sh -p \; -quit`

Hay muchos más comandos de los que podemos escalar privilegios.

<br>

<br/>
### 3. Explotación de tareas Cron.

<br>

<br/>

### 4. Explotación de un PATH Hijacking frente a un binario SUID.
