# Como-montar-un-servidor-de-videojuegos
Taller parte de la jornada dos del evento ¨Las mujeres en el videojuego¨.

## Paso 0 - Prerequisitos

Antes de nada necesitamos un servidor en el que correr nuestros sevidores de juegos, hay muchas opciones disponibles en el mercado pero lo más adecuado para nuestro uso casero del servidor es alquilar un VPS (Servidor privado virtual). Este tipo de servidor consiste en una virtualizacion dentro de uno de los servidores del datacenter de la empresa que oferta el servicio, básicamente nos ofrecen acceso a una fracción de los recursos de la máquina para nuestro uso particular.

Yo personalmente utilizo los VPS Value de [OVH](https://www.ovhcloud.com/es-es/vps/?_gl=1*gttbzb*_gcl_aw*R0NMLjE2MjE0MzA4ODYuQ2owS0NRanc3cEtGQmhEVUFSSXNBRlVvTURiQnJqTF8xWmNsSThNZXFkQnplYTR1Qmc0U0FzRkJDLVluSnlhVXpyblJqYzJkRVBGaFRaMGFBbGN1RUFMd193Y0I.) ya que esta empresa tiene datacenters en Francia, los cuales son muy ventajosos para mí por su localización geográfica.

Una vez tengamos contratado el servicio con un proveedor este nos instalará una distribución GNU/Linux y nos facilitará el usuario y contraseña de un usuario root.

## Paso 1 - Conectate via shh

Para poder acceder a nuestro servidor desde nuestro ordenador hemos de abrir nuestra consola de comandos. En windows pulsa `command("tecla Windows") + R` y escribe `cmd` en la ventana que se muestra. Si tu OS es una distribución linux pulsa `control + alt + t`. Si utilizas MacOS puedes buscar la terminal dentro del launchpad pero la manera más sencilla es utilizar el launchpad `cmd + espacio` y buscar terminal.

Deberías ver algo similiar a esto:

<img src="https://upload.wikimedia.org/wikipedia/commons/b/b3/Command_Prompt_on_Windows_10_RTM.png"/>

La consola es una interfaz de comunicación con el ordenador únicamente basado en texto. A día de hoy muchos de los programas que incorporan nuestros sistemas operativos funcionan solamente mediante el uso de esta interfaz.

Ahora vamos a conectarnos vía SSH a la terminal de nuestro servidor. SSH es un programa y un protocolo de conexión remoto que permite, entre otras cosas, ejecutar programas remotamente. Para lograr esto simplemente tenemos que escribir el siguiente comando en la terminal de nuestro ordenador.

```bash
ssh nombre_de_usuario@ip_del_servidor
```

Cuando ejecutemos esto nuestro servidor nos pedirá la contraseña para acceder con ese nonbre de usuario y una vez la introduzcamos ya tendremos total acceso al ordenador.

## Paso 2 - Actualiza tus paquetes

Antes de hacer nada más sería conveniente actualizar los paquetes. Para hacer eso en primer lugar invocamos el siguiente comando para actualizar la lista de paquetes disponibles:

```bash
apt update
```
Ahora que nuestro servidor ya sabe cuales son los programas que puede actualizar/instalar invocamos el siguiente comando para que lo actualice:

```bash
apt upgrade
```

Esto es algo que deberíais hacer de vez en cuando para evitar que nadie acceda a vuestro servidor a través de un error público en algún programa.

## Paso 3 - Conseguir el servidor de minecraft

Normalmente si quisieramos descarganos el servidor de minecraft para ejecutarlo en nuestro ordenador simplemente lo descargaríamos de la [página de Mojang](https://www.minecraft.net/es-es/download/server). Pero claro nuestro servidor no dispone de un navegador web ni nada similar para que nosotros podamos clicar en el enlace de descarga. Por suerte tenemos un programa llamado wget que nos permite descargar archivos desde la terminal: 

```bash
wget https://launcher.mojang.com/v1/objects/1b557e7b033b583cd9f66746b7a9ab1ec1673ced/server.jar
```
Ahora si invocamos `ls` para listar nuestros archivos deberíamos de conseguir algo como esto:

```bash
root@vps718866:~# ls
server.jar
```

Hemos descargado server.jar en la ruta en la que estábamos, pero ya que este archivo va a generar muchos otros archivos donde almacenar la información del mundo y los jugadores será mejor que creemos su propia carpeta.

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
No obstante el servidor que nos facilita Mojang no es un programa que podamos ejecutar sin más como un exe en Windows. Esto es un jar y necesita que se ejecute con Java. Sea pues, instalemos java. Este comando es algo que puede cambiar dependiendo de la distribución que estés usando, si no estás usando ubuntu deberías googlear como.

```bash
 apt install default-jre
```
Ahora para comprobar que todo funciona correctamente ejecuta:

```bash
java -version
```

Deberías conseguir algo similar a esto

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
Simplemente nuestro servidor nos está pidiendo que aceptemos la EULA del servidor para poder usarlo, para ello tendremos que modificar el archivo eula.txt que acaba de creear. Para hacer esto utilizaremos un editor de texto de nuesta terminal llamado nano:

```bash
nano eula.txt
```
<img width="949" alt="imagen" src="https://user-images.githubusercontent.com/44611837/118965218-b89d8380-b968-11eb-99bc-224b53375cf6.png">

Simplemente con las flechas moveremos el puntero hasta la última línea para cambiar el valor de false a true. Y pulsaremos `control + O` para guardar y `control + X` para cerrar el editor.

Ahora si volvemos a ejecutar el mismo comando deberíamos de poder entrar en el servidor.

```bash
java -Xmx1024M -Xms1024M -jar server.jar nogui 
```

No obstante aunque funcione si cerramos el terminal se cerrara también el servidor.

## Paso 5 - Mantener el servidor corriendo cuando cierro el cmd

Como probablemente te habrás dado cuenta  de que si cierras el cmd también lo hace el servidor, esto sucede porque al cerrar la cmd cierras la sesión en el servidor y todos los procesos que estaban corriendo ahí se cierran a su vez a consecuencia de esto. 

Para solucionar esto vamos a utilizar un programa llamado GNUScreen. Este programa es un multiplexador de terminales, básicamente está pensado para permitir al usuario utilizar diferentes sesiones utilizando la misma interfaz. Lo interesante es que las terminales que abramos y archivemos con screen son persistentes, lo que quiere decir que sus procesos van a seguir funcionando aunque terminemos nuestra sesión ssh.

Entonces lo primero que tendremos que hacer es crear una nueva sesión con screen, puedes hacerlo sin necesidad de darle un nombre, pero es conveniente para poderla localizar comodamente en un futuro.

```bash
screen -S minecraft_session
```

Corre el server.

```bash
java -Xmx1024M -Xms1024M -jar server.jar nogui 
```

Ahora para cerrar la sesión podemos pulsar `Ctrl+a d` y cerrar nuestra cmd con tranquilidad.

## Extra 1 - Recuperar la session de screen

Para recuperar la última sesión que archivaste en screen puedes ejecutar simplemente:

```bash 
screen -r
```

Si la sesión a la que quieres acceder no es la última simplemente lista las sesiones disponibles:

```bash 
screen -ls
```

Y ya puedes recuperar la sesión que quieras indicando el id de la sesión:

```bash 
screen -r minecraft-session
```

## Extra 2 - Descargar archivos desde el servidor

```bash
scp root@51.83.43.17:/root/MinecraftTubarra/world/* C:\Users\ddomi\Downloads
```
