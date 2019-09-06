---
layout: post
title:  "Solución al primer desafio del Google's Begginers Quest CTF"
date:   2019-05-09 21:03:36 +0530
---

**"Enter Space-Time Coordinates"** es el nombre del primer desafío, en donde el contexto es que estamos en una nave espacial sentados al frente del coordinador de viaje.

Luego de descargar el archivo a simple vista se puede ver que es un archivo ZIP, al abrirlo se encuentran dos archivos **log.txt** y **rand2**.

Analísis de los Archivos
------------------------

Para el analísis de los archivo usamos inicialmente utilizamos el comando `file`, el cuál permite obtener informacón basica de un archivo.

```bash
root@kali:~/Desktop/GoogleCTF/1stChallenge# file log.txt 
log.txt: ASCII text

root@kali:~/Desktop/GoogleCTF/1stChallenge# file rand2 
rand2: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=0208fc60863053462fb733436cef1ed23cb6c78f, not stripped
```
Este analísis nos muestra que log.txt es un archivo de texto plano que aparentemente contiene una salida de rand2, el cuál es un archivo x86-64 ELF el cual puede ser ejecutado.

Usualmente con el analísis basico me parece interesante usar el comando `cat` para dar un vistazo al contenido de los archivos, en este caso podemos ver la bandera a simple vista luego de ejecutar `cat rand2`

![Screenshot de la bandera a simple vista.](/assets/images/googleCTFC1.png)

Esta manera de obtener la bandera es un poco vulgar. Vamos a intentar maquillarla un poco.

El camino rapido
----------------

De antemano se sabe que las banderas tienen el formato **CTF{bandera}**, con lo cual usando el una combinación del comando `strings` para revisar el contenido del binario del archivo ejecutable y usando `grep` buscamos CTF para buscar la bandera.

```bash
root@kali:~/Desktop/GoogleCTF/1stChallenge# strings rand2 | grep CTF
Arrived at the flag. Congrats, your flag is: CTF{welcome_to_googlectf}
```

obteniendo así la bandera.

Cabe recalcar que esta es una de las formas en la que se puede resolver este desafio, ya que analizando el binario se puede llegar a la manera "legitima" de obtener la bandera.