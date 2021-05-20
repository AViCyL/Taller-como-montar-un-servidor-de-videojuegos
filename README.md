# Como-montar-un-servidor-de-videojuegos
Taller parte de la jornada dos del evento ¨Las mujeres en el videojuego¨

## Paso 0 - Prerequisitos

Antes de nada necesitamos un servidor en el que correr nuestros sevidores de juegos, hay muchas opciones disponibles en el mercado pero lo más adecuado para nuestro uso casero del servidor es alquilar un VPS (Servidor privado virtual). Este tipo de servidor consiste en una virtualizacion dentro de uno de los servidores del datacenter de la empresa que oferta el servicio, basicamente nos ofercen aceso a una fración de los recursos de la máquina para nuestro uso particular. 

Yo personalmente utilizo los VPS Value de [OVH](https://www.ovhcloud.com/es-es/vps/?_gl=1*gttbzb*_gcl_aw*R0NMLjE2MjE0MzA4ODYuQ2owS0NRanc3cEtGQmhEVUFSSXNBRlVvTURiQnJqTF8xWmNsSThNZXFkQnplYTR1Qmc0U0FzRkJDLVluSnlhVXpyblJqYzJkRVBGaFRaMGFBbGN1RUFMd193Y0I.) ya que esta empresa tiene datacenters en Francia, los cuales son muy ventajosos para mi por su localización geográfica.

Una vez tengamos contratado el servicio con un provedor este nos instalara una distribucion GNU/Linux y nos facilitara el usuario y contrase;a de un usuario root.

## Paso 1 - Conectate via shh

Para poder acceder a nuestro servidor desde nuestro ordenador hemos de abrir nuestra consola de comandos. En windows pulsa command("tecla Windows") + R y escribe cmd en la ventana que se muestra. Si tu OS es una distribucion linux pulsa control + alt + t. Si utilizas MacOS puedes buscar la terminal dentro del launchpad pero la manera mas sencilla es utilizar el launchpad (cmd + espacio) y buscar terminal.

Deberias ver algo similiar a esto:

<img src="https://upload.wikimedia.org/wikipedia/commons/b/b3/Command_Prompt_on_Windows_10_RTM.png"/>

La consola es una interfad de comunicación con el ordenador unicamente basado en texto. A dia de hoy muchos de los programas que incorporan nuestros sistemas operativos funcionan solamente mediante el uso de esta interfaz.

Ahora vamos a conectarnos via SSH a la terminal de nuestro servidor. SSH es un programa y un protocolo de conexión remoto que permite, entre otras cosas, ejecutar programas remotamente. Para lograr esto simplemente tenemos que escribir el siguiente comando en la terminal de nuestro ordenador.

```bash
ssh nombre_de_usuario@ip_del_servidor
```

Cuando ejecutemos esto nuestro servidor nos pedirá la contraseña para acceder con ese nonbre de usuario y una vez la introduzcamos ya tendremos total acceso al ordenador.

## Paso 2 - Actualiza tus paquetes

Antes de hacer nada más sería conveniente actualizar los paquetes. Para hacer eso en primer lugar invocamos el siguiente comando para actualizar la lista de paquetes disponibles:

```bash
apt update
```
Ahora que nuestro servidor ya sabe cuales son los programas que puede actualizar invocamos el siguiente comando para que lo actualice:

```bash
apt upgrade
```

Esto es algo que deberíais hacer de vez en cuando para evitar que nadie acceda a vuestro servidor atraves de un error publico en algun programa.

## Paso 3 - Conseguir el servidor de minecraft

Normalmente si quisieramos descarganos el servidor de minecraft para ejecutarlo en nuestro ordenador simplemente lo descargaríamos de la [página de Mojang](https://www.minecraft.net/es-es/download/server). Pero claro nuestro servidor no dispone de un navegador web ni nada similar parq que nosotros podamos clicar en el enlace de descarga. Por suerte tenemos un programa llamado wget que nos permite descargar archivos desde la terminal: 

```bash
wget https://launcher.mojang.com/v1/objects/1b557e7b033b583cd9f66746b7a9ab1ec1673ced/server.jar
```
Ahora si invocamos ls para listar nuestros archivos deberiamos de conseguir algo como esto:

```bash
root@vps718866:~# ls
server.jar
```

Hemos descargado server.jar en la ruta en la que estabamos, pero ya que este archivo va a generar muchos otros archivos donde almacenar la informacion del mundo y los jugadores sera mejor que creemos su propia carpeta.

```bash
mdkir minecaft-server
```
Y lo movamos dentro:
```bash
 mv server.jar minecraft-server/
```
## Paso 4 - Lanzar el servidor

Antes de nada debemos entrar en la carpeta en la que estamos guardando el servidor:

```bash
 cd minecraft-server
```
No obstante el servidor que nos facilita Mojang no es un programa que podamos ejecutar sin más como un exe en Windows. Esto es un jar y necesita que se ejecute con Java. Sea pues, instalemos java. Este comando es algo que puede cambiar dependiendo de la distribucion que estes usando, si no estas usando ubuntu deberías googlear como.

```bash
 apt install default-jre
```
Ahora para comprobar que todo funciona correctamente ejecuta:

```bash
java -version
```

Deberias conseguir algo similar a esto

```bash
openjdk version "11.0.11" 2021-04-20
OpenJDK Runtime Environment (build 11.0.11+9-Ubuntu-0ubuntu2.20.04)
OpenJDK 64-Bit Server VM (build 11.0.11+9-Ubuntu-0ubuntu2.20.04, mixed mode, sharing)
```

Y ya por fin podemos arrancar el servidor:
```bash
java -Xmx1024M -Xms1024M -jar server.jar nogui 
```
La primera vez que arranquemos el servidor nos va a aparecer algo similar a esto:
```bash
[10:37:19] [main/ERROR]: Failed to load properties from file: server.properties
[10:37:20] [main/WARN]: Failed to load eula.txt
[10:37:20] [main/INFO]: You need to agree to the EULA in order to run the server. Go to eula.txt for more info.
```
Simplemente nuestro servidor nos esta pidiendo que aceptemos la EULA del servidor para poder usarlo, para ellos entremos que modificar el archivo eula.txt que acaba de creear, para hacer esto utilizaremos un editor de texto de nuesta terminal llamado nano:

```bash
nano eula.txt
```
<img width="949" alt="imagen" src="https://user-images.githubusercontent.com/44611837/118965218-b89d8380-b968-11eb-99bc-224b53375cf6.png">

Simplemente con las flechas moveremos el puntero hasta la ultima línea para cambiar el valor de false a true. Y pulsaremos control + O para guardar y control + X para cerrar el editor.

Ahora si volvemos a ejecutar el mismo comando deberiamos de poder entrar en el servidor.

```bash
java -Xmx1024M -Xms1024M -jar server.jar nogui 
```

No obstante aunque funcione si cerramos el terminal se cerrara tambien el servidor.

## Extra - Descargar archivos desde el servidor

```bash
scp root@51.83.43.17:/root/MinecraftTubarra/world/* C:\Users\ddomi\Downloads
```
