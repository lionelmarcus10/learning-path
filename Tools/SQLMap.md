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
     -H='Cookie:<MonCookie> 

     # enregistrer output
     -t <Path> 

     # verbose in terminal
     -v 6 

     # --host, --referer, --random-agent

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
---

---