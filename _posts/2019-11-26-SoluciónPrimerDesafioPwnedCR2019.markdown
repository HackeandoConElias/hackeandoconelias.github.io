---
layout: post
title:  "Solución al primer desafío del PwnedCR 2019"
date:   2019-11-26 08:58:36 +0530
---

El próximo 7 de Diciembre en Costa Rica vamos a tener la segunda edición del Pwned CR 2019, un evento enfocado en seguridad informatica y ethical hacking creado por la comunidad para la comunidad.

En la pagina del evento los organizadores nos dejarón un pequeño desafio en la [pagina del evento](https://pwnedcr.rocks/#/home). ¡Happy Hacking!

Analísis del sitio Web
------------------------

En la página podemos ver información del evento: Horarios de las charlas, talleres y zonas que estarán el día del evento. Pero además hay algo que llama la atención, en tendencias podemos ver algunos vínculos que parecen ser hex.

![Screenshot de la bandera a simple vista.](/assets/images/pwnedCR2019-Challenge1-Tendencias.png)

Podemos convertirlo a ASCII utilizando cualquier tipo de herramienta, en este caso usamos [rapidtables.com](https://www.rapidtables.com/convert/number/hex-to-ascii.html) y obtenemos lo siguiente:

\# 0x46696e64 => Find

\# 0x536f6d657468696e67 => Something

\# 0x48657265 => Here

\# 0x53656564 => Seed

\# 0x50776e6564 => Pwned

\# 0x4861636b72626f79 => Hackrboy

Otro elemento que llama la atención es la barra de búsqueda que esta justo encima de las tendencias, y ya que la pista en el HEX dice que hay algo aquí vamos a darle un inspect:

```bash
<input aria-label="Search" class="form-control borde-buscador mr-sm-2 ng-valid ng-touched ng-dirty" placeholder="Buscar" type="search" ng-reflect-model="">
```
Cada vez que se ingresa algo en el cuadro de búsqueda, cambia el contenido dentro del ng-reflect-model, lo que nos da una pista de que lo que ingresemos en el cuadro de busqueda puede ayudar con el desafio.

En la pista anterior habla de un **Seed** lo cual podría significar el uso de algún random. Podemos probar una a una las palabras obtenidas de los HEX en el cuadro de búsqueda.

En mi caso al escribir la palabra "Seed" se activó el trigger que descarga un archivo llamado Hahaudidit.ini, luego de varias pruebas podemos notar que la palabra que activa la descarga es aleatoria.

A simple vista no hay nada más que ver en el sitio, por lo que pasamos a los developer tools del explorador en este caso Google Chrome.

En el index del sitio podemos encontrar dos tiras en binario, las cuales podemos convertir a ASCII utilizando una herramienta en linea como [binaryhexconverter.com](https://www.binaryhexconverter.com/binary-to-ascii-text-converter) y obtenemos:

01100011 00110000 01110010 01101001 00110000 01010101 01110011 01000010 00110000 01111001 00111111 => c0ri0UsB0y?

01110000 00100000 01110111 00100000 01101110 0010000000100000 01110000 00100000 01110111 00100000 01101110 => p w n  p w n

De momento no tenemos alguna utilidad para esta pista pero, luego de seguro será útil.

Una vez descargado el archivo podemos empezar a jugar con el.



Al ejecutar el comando **file** en el archivo podemos ver que es un .rar por lo que podemos proceder a descomprimirlo usando el comando **unrar** y dentro encontramos otro archivo Hahaudidit esta vez sin ninguna extensión.

Al ejecutar **file** sobre este podemos ver que es otro rar, y al intentar descomprimir el archivo usando **unrar** encontramos dentro un archivo llamado c0ri0UsB0y esta vez protegido por una contraseña. El nombre del archivo contenido en el .rar ya lo vimos antes en una de las tiras de binario que convertimos a ASCII anteriormente por lo que procedemos a probar estas como contraseña para la extracción.

¡Vualá! Al usar p w n  p w n como contraseña logramos extraer el contenido que resulta ser otro archivo .rar c0ri0UsB0y...

Al proceder a descomprimirlo usando **unrar** encontramos que dentro tiene un archivo llamado URCURIOUS.txt protegido por una contraseña, como ya usamos una de las tiras de binario convertidas a ASCII se procede a utilizar la que no usamos anteriormente: c0ri0UsB0y?

Y así logramos extraer el archivo URCURIOUS.txt que al ver lo que tiene dentro usando **cat** vemos que contiene una tira de binario que al convertirla a ASCII obtenemos:

01010010 01000101 01000001 01000100 01011001 01000110 01001111 01010010 01000011 01010100 01000110 00111111 => READYFORCTF?

Obteniendo así la bandera final del desafío.

Y si que estamos listos para el CTF el día 7 de diciembre en el PwnedCR 2019. Como pudimos ver este desafio preparado por [Asterion](https://twitter.com/BarrantesK24) fue un calentamiento perfecto para prepararse para el evento.

Este desafió lo resolvi junto con mis amigos [Andrés](https://twitter.com/EndlazeSG) y [Diego](https://twitter.com/diesveca).