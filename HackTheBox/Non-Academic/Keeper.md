---
tags:
  - HTB
  - Machine
  - Box
  - Easy
---
# Keeper - Easy

### Objectif : 
Se connecter à la machine et trouver les flags de user.txt et root.txt

### Information :

AddressIP : **10.10.11.227**

### Process : 

##### Récolte d'information : 

###### Scan du réseau : 

```bash
# premier scan
nmap 10.10.11.227 -F
```

###### résultat du premier scan

![nmap keeper](../../Ressources/IMG/HTB/Machines/Keeper/Machine-Keeper-02.png)

###### connexion au site 

![[Machine-Keeper-01.png]]
![](../../Ressources/IMG/HTB/Machines/Keeper/Machine-Keeper-03.png)


![](../../Ressources/IMG/HTB/Machines/Keeper/Machine-Keeper-04.png)

![](../../Ressources/IMG/HTB/Machines/Keeper/Machine-Keeper-05.png)

- chercher les users

![](../../Ressources/IMG/HTB/Machines/Keeper/Machine-Keeper-06.png)

on a deux utilisateurs : 

![](../../Ressources/IMG/HTB/Machines/Keeper/Machine-Keeper-07.png)
selectionnons le premier : 

![](../../Ressources/IMG/HTB/Machines/Keeper/Machine-Keeper-08.png)

###### connexion via ssh

```bash
ssh lnorgaard@10.10.11.227
# password : Welcome2023!
```

###### User flag 

![](../../Ressources/IMG/HTB/Machines/Keeper/Machine-Keeper-09.png)

###### Exfiltrer le Fichier zip

![](../../Ressources/IMG/HTB/Machines/Keeper/Machine-Keeper-10.png)
###### connaître le contenu du dump

![](../../Ressources/IMG/HTB/Machines/Keeper/Machine-Keeper-11.png)
###### Ouvrir la db Keepass

après avoir cherché le mot sur  google translate, on peut récupérer les vrai lettres

![](../../Ressources/IMG/HTB/Machines/Keeper/Machine-Keeper-11.png)
###### Se connecter en tant que root et avoir le flag root
![](../../Ressources/IMG/HTB/Machines/Keeper/Machine-Keeper-12.png)

- copier le code dans un fichier ppk puis le convertir en fichier pem

```bash
sudo vim rsa_key.ppk
[sudo] password for kali: 
┌──(kali㉿kali)-[/media/sf_linu/keepass-dump-masterkey]
└─$ puttygen rsa_key.ppk -O private-openssh -o rootkey.pem

┌──(kali㉿kali)-[/media/sf_linu/keepass-dump-masterkey]
└─$ ssh -i rootkey.pem  root@10.10.11.227
Welcome to Ubuntu 22.04.3 LTS (GNU/Linux 5.15.0-78-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage
Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

You have new mail.
Last login: Sat Nov 11 07:03:16 2023 from 10.10.16.18
root@keeper:~# ls
root.txt  RT30000.zip  SQL
root@keeper:~# cat root.txt 
9efe54f32d7ba678b04083b130d69026
```
