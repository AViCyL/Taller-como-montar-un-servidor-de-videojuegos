# Como-montar-un-servidor-de-videojuegos
Taller parte de la jornada dos del evento ¨Las mujeres en el videojuego¨

## Paso 0 - Prerequisitos

Antes de nada necesitamos un servidor en el que correr nuestros sevidores de juegos, hay muchas opciones disponibles en el mercado pero lo más adecuado para nuestro uso casero del servidor es alquilar un VPS (Servidor privado virtual). Este tipo de servidor consiste en una virtualizacion dentro de uno de los servidores del datacenter de la empresa que oferta el servicio, basicamente nos ofercen aceso a una fración de los recursos de la máquina para nuestro uso particular. 

Yo personalmente utilizo los VPS Value de [OVH](https://www.ovhcloud.com/es-es/vps/?_gl=1*gttbzb*_gcl_aw*R0NMLjE2MjE0MzA4ODYuQ2owS0NRanc3cEtGQmhEVUFSSXNBRlVvTURiQnJqTF8xWmNsSThNZXFkQnplYTR1Qmc0U0FzRkJDLVluSnlhVXpyblJqYzJkRVBGaFRaMGFBbGN1RUFMd193Y0I.) ya que esta empresa tiene datacenters en Francia, los cuales son muy ventajosos para mi por su localización geográfica.

Una vez tengamos contratado el servicio con un provedor este nos instalara una distribucion GNU/Linux y nos facilitara el usuario y contrase;a de un usuario root.

## Paso 1 - Conectate via shh

Para poder acceder a nuestro servidor desde nuestro ordenador hemos de abrir nuestra consola de comandos. En windows pulsa command("tecla Windows") + R y escribe cmd en la ventana que se muestra. Si tu OS es una distribucion linux pulsa control + alt + t. Si utilizas MacOS puedes buscar la terminal dentro de tus aplicaciones.

Deberias ver algo similiar a esto:

<img src="https://upload.wikimedia.org/wikipedia/commons/b/b3/Command_Prompt_on_Windows_10_RTM.png"/>

## Extra - Descargar archivos desde el servidor

```bash
scp root@51.83.43.17:/root/MinecraftTubarra/world/* C:\Users\ddomi\Downloads
```
