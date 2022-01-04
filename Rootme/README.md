![](https://miro.medium.com/max/2000/1*esghPi4R2cEjX5LstPmxHg.png)

___

### Reto completo 

[https://www.youtube.com/watch?v=8UbjrRyW3Tc&ab_channel=Z3r0_302](https://www.youtube.com/watch?v=8UbjrRyW3Tc&ab_channel=Z3r0_302)

___

### Answers
Para visualizar los puertos abiertos, usaremos nmap para el descubrimiento de servicios y sus respectivas versiones.

`$ sudo nmap -sS --min-rate 5000 10.10.81.126 -v -n -Pn --open -p- -oN scann`

`-sS: es para enviar un paquete Syn`

`--min-rate [number]: establece un determinado tiempo de respuesta`

`-v: verbosidad`

`-n: no resuelve el DNS`

`-Pn: no realiza un sondeo de ping`

`--open: indica solo los puertos abiertos`

`-p-: escanea todos los puertos del 1 al 65536 `

`-oN: guarda el resultadao en un archivo de texto`
	
	Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times may be slower.
	Starting Nmap 7.92 ( https://nmap.org ) at 2022-01-03 19:19 CST
	Initiating SYN Stealth Scan at 19:19
	Scanning 10.10.81.126 [65535 ports]
	Discovered open port 80/tcp on 10.10.81.126
	Discovered open port 22/tcp on 10.10.81.126
	Completed SYN Stealth Scan at 19:20, 22.36s elapsed (65535 total ports)
	Nmap scan report for 10.10.81.126
	Host is up (0.28s latency).
	Not shown: 50013 closed tcp ports (reset), 15520 filtered tcp ports (no-response)
	Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
	PORT   STATE SERVICE
	22/tcp open  ssh
	80/tcp open  http

	Read data files from: /usr/bin/../share/nmap
	Nmap done: 1 IP address (1 host up) scanned in 22.55 seconds
			   Raw packets sent: 109809 (4.832MB) | Rcvd: 55985 (2.241MB)
			   
##### Scan the machine, how many ports are open?

`2`

Visualizamos el puerto 80 abierto, ahora significa que esta alojando algun sitio web, para verificar nos vamos al navegador y pegamos la ip.

##### What version of Apache is running?

`2.4.29`

`Command execute: nmap -sS -sV -p 80 10.10.81.126`

##### What service is running on port 22?

`SSH`

`Command execute: nmap -sS -sV -p 22 10.10.81.126`

##### What is the hidden directory?

`/panel/`

`Command execute: gobuster dir -u http://10.10.81.126/ -w /usr/share/wordlists/dirb/common.txt 2>/dev/null `

	
	===============================================================
	Gobuster v3.1.0
	by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
	===============================================================
	[+] Url:                     http://10.10.81.126/
	[+] Method:                  GET
	[+] Threads:                 10
	[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
	[+] Negative Status codes:   404
	[+] User Agent:              gobuster/3.1.0
	[+] Timeout:                 10s
	===============================================================
	2022/01/03 19:21:06 Starting gobuster in directory enumeration mode
	===============================================================
	/.htaccess            (Status: 403) [Size: 277]
	/.hta                 (Status: 403) [Size: 277]
	/.htpasswd            (Status: 403) [Size: 277]
	/css                  (Status: 301) [Size: 310] [--> http://10.10.81.126/css/]
	/index.php            (Status: 200) [Size: 616]                               
	/js                   (Status: 301) [Size: 309] [--> http://10.10.81.126/js/] 
	/panel                (Status: 301) [Size: 312] [--> http://10.10.81.126/panel/]
	/server-status        (Status: 403) [Size: 277]                                 
	/uploads              (Status: 301) [Size: 314] [--> http://10.10.81.126/uploads/]
	===============================================================
	2022/01/03 19:22:34 Finished
	===============================================================

##### user.txt

`THM{y0u_g0t_a_sh3ll}`

`Command execute: find / -type f -name "user.txt" 2>/dev/null`

#####  Search for files with SUID permission, which file is weird? 

`/usr/bin/python`

`Command execute: find / -perm -4000 2>/dev/null`

##### root.txt

`THM{pr1v1l3g3_3sc4l4t10n}`

`Command execute: find / -type f -name "root.txt" 2>/dev/null`

