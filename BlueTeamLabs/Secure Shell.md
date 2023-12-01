---
tags:
  - SOC-Tier-2
  - Log-Analysis
  - SSH
---

## Scénario

```
Hey! We had a SSH service on a system and noticed unusual change in size of the log file. Don’t panic, it was the new IT guys’ daughter who said she was able to break into the system. I had given her permission to test some of these services. I am giving you the log file, can you solve the following queries?
```

## Challenge Submission

### Is it an internal or external attack, what is the attacker IP? _(5 points)_


```
cat sshlog.log | grep -e  "user .* from"
```
on peut donc  remarquer les multiples tentatives avec son addresse IP qui semble externe

```
# réponse
external:192.168.1.17
```

### How many valid accounts did the attacker find, and what are the usernames? _(5 points)_

```
cat sshlog.log | grep -e  "Accepted password for .* from"


# on a comme résultat 
7176 2021-04-30 00:53:25.023 Accepted password for sophia from 192.168.1.17 port 41990 ssh2
7300 2021-04-30 01:01:11.699 Accepted password for sophia from 192.168.1.17 port 42364 ssh2

```

Après m'être renseigné sur des forums, j'ai compris que les logs SSH pour un login réussi était Accepted password for "UserName"

```
# réponse
1:sophia
```


#### How many times did the attacker login to these accounts? _(5 points)_

```
# en s'appuyant sur le résultat précédant, on a  comme réponse
2
```

#### When was the first request from the attacker recorded? _(5 points)_

*Methode 1*
```
# chercher l'addresse ip dans les logs et avoir sa première occurence 
192.168.1.17

# soit on effectue un ctrl + f dans le fichier 
# soit on lit le fichier et on tombe sur la ligne 64

``` 

*Methode 2*

```
cat sshlog.log | grep -e  "from 192.168.1.17" | head -1
```

*Réponse*
```
# réponse
2021-04-29 23:52:25.989
```

#### What is the log level for the log file? _(5 points)_

> A **log level** or **log severity** is a piece of information telling how important a given log message is. It is a simple, yet very powerful way of distinguishing log events from each other.

> différents niveau de log level : DEBUG, INFO, WARNING, ERROR, CRITICAL

on remarque le plus de log level présent est débug

```
# réponse
debug
```

#### Where is the log file located in Windows? _(5 points)_

*chercher sur google * 
[lien vers le forum](https://superuser.com/questions/1635361/starting-openssh-server-in-windows-with-debug-messages-enabled-d)
```
# réponse 
C:\_ProgramData_\_ssh_\_logs_\_sshd_._log_
```
