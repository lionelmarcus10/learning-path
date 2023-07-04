# Ffuf - WEB ENUMERATION

```bash
# toujours ajouter -v -c
# ne pas oublier d'ajouter le sous domaine que tu fuzz dans /etc/hosts

# augmenter le nombre de tentative
ffuf -t 200 

# url for directory -c (couleur) -v (afficher)
ffuf -w <dico> -u <URL/FUZZ>

# url for file extension
ffuf -w <dico> -u <URL/direcroy/[PageName]FUZZ>

# specifier l'extension ( -e ) en cas d'utilisation du recursif (-recursion):
ffuf -w wordlist.txt:FUZZ -u link.com/FUZZ -recursion -e .php .html

# vhost and subdomain
ffuf -w wordlist.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb' -fs <Size>

# parameter fuzzing get
ffuf -w wordlist.txt:FUZZ -u <link>?FUZZ=<key | website special string >

# parameter fuzzing post
ffuf -w wordlist.txt:FUZZ -u <link> -X POST -d 'FUZZ=< key | WebsiteSpecificElement>' -H 'Content-Type: application/x-www-form-urlencoded' -fs <Size>

# pour le fuzzing de valeur, on fait le fuzzing post mais avec 
-d <Element>=FUZZ # avec un dico étant des nombres de 0 à Infini

```

```bash
# post  with curl 
curl <link> -X POST -d '<ELEMENTTOPOST>=<key>' -H 'Content-Type: application/x-www-form-urlencoded'
```
## Ffuf

| **Command**   | **Description**   |
| --------------|-------------------|
| `ffuf -h` | ffuf help |
| `ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ` | Directory Fuzzing |
| `ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/indexFUZZ` | Extension Fuzzing |
| `ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/blog/FUZZ.php` | Page Fuzzing |
| `ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v` | Recursive Fuzzing |
| `ffuf -w wordlist.txt:FUZZ -u https://FUZZ.hackthebox.eu/` | Sub-domain Fuzzing |
| `ffuf -w wordlist.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb' -fs 900 ou xxx` | VHost Fuzzing |
| `ffuf -w wordlist.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php?FUZZ=key -fs xxx` | Parameter Fuzzing - GET |
| `ffuf -w wordlist.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx` | Parameter Fuzzing - POST |
| `ffuf -w ids.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx` | Value Fuzzing |  

# Wordlists ( Parrot HTB)

| **Command**   | **Description**   |
| --------------|-------------------|
| `/opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt` | Directory/Page Wordlist |
| `/opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt` | Extensions Wordlist |
| `/opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt` | Domain Wordlist |
| `/opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt` | Parameters Wordlist |

# Misc

| **Command**   | **Description**   |
| --------------|-------------------|
| `sudo sh -c 'echo "SERVER_IP  academy.htb" >> /etc/hosts'` | Add DNS entry |
| `for i in $(seq 1 1000); do echo $i >> ids.txt; done` | Create Sequence Wordlist |
| `curl http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=key' -H 'Content-Type: application/x-www-form-urlencoded'` | curl w/ POST |