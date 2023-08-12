# SQL BASICS

----
* MySql/MariaDB default port : 3306
```bash
# installation 
sudo apt install mysql-server -y
# config 
cat /etc/mysql/mysql.conf.d/mysqld.cnf | grep -v "#" | sed -r '/^\s*$/d'

#connect to db
mysql -u <Username> -p
-h <RemoteHost> -P <Port>

# create db
CREATE DATABASE <dbName>;
#  show db
SHOW DATABASES;
# select db
USE <dbName>;
# show table 
SHOW TABLES;
# show descriotion
DESCRIBE <tableName>;
```

```sql
# creation
INSERT INTO table_name VALUES(...);
CREATE TABLE table_name(....);

# selection
SELECT * FROM table_name;
SELECT element_name1, element_name2 from table_name;
# order selection
ORDER BY <ColumnName> <DESC | ASC>
LIMIT <Number>;
LIKE '<TO_MATCH>%'; # match tous les mots qui continnement la suite de caractère
LIKE '<_>'; # _ match n characters
# suppression
DROP TABLE table_name;

# alter table 
ALTER TABLE table_name ADD newColumn_name TYPEDESCRIPTION;

# rename
ALTER TABLE table_name RENAME COLUMN OLD_col_Name TO New_col_name;

# drop column
ALTER TABLE table_name DROP Column_name;

# update table 
UPDATE table_name SET column1=newvalue1, column2=newvalue2, ... WHERE <condition>;

# operators
AND | && 
OR |  ||
NOT | !=
```
----