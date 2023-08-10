# Footprinting

## Principes d'énumération

### Questions 
* What can we see?
* What reasons can we have for seeing it?
* What image does what we see create for us?
* What do we gain from it?
* How can we use it?
* What can we not see?
* What reasons can there be that we do not see?
* What image results for us from what we do not see?

### Principes

| **PRINCIPES** |
|---|
|There is more than meets the eye. Consider all points of view.|
|Distinguish between what we see and what we do not see.|
|There are always ways to gain more information. Understand the target.|



**`Another advantage of these principles is that we can see from the practical tasks that we do not lack penetration testing abilities but technical understanding when we suddenly do not know how to proceed because our core task is not to exploit the machines but to find how they can be exploited.`**


**Enumeration Layers**
![Layer of Enumeration](../../Ressources/IMG/0-FOOTPRINT-LAYERS.png)

**Enumeration Process**
![Enumeration Process](../../Ressources/IMG/0-FOOTPRINT-ENUMERATION.png)

---
## Enumeration sur l'infrastructure
### Domain Information
  
  1.  Verifier le certificat ssl et utiliser crt.sh

    ```bash
    curl -s https://crt.sh/\?q\=<Target.com>\&output\=json | jq .
    # filtrer par subdomain 
    <Command> | jq . | grep name | cut -d":" -f2 | grep -v "CN=" | cut -d'"' -f2 | awk '{gsub(/\\n/,"\n");}1;' | sort -u
    ```

  2. Company Hosted Servers
  ```bash
  for i in $(cat <subdomainlistFileName>);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f1,4;done
  ```
  3. Use Shodan
  ```bash
    for i in $(cat <subdomainlistFileName>);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f4 >> ip-addresses.txt;done
    for i in $(cat ip-addresses.txt);do shodan host $i;done
  ```
  4. use Dig

  5. [Information Gathering module](./information%20gathering%20-Web.md)

  6. use Google Dorks `inurl` and `intext`

  7. use domain.glass and https://buckets.grayhatwarfare.com/ to find leak or weakness
---

## Host Based Enumeration

### FTP : File Trasfert Protocol

**FTP** 
- Control channel : Port 21
- data transmission channel : Port 20

**TFTP :  Trivial File Trasfert Protocol**

- command 

    | Commands |	Description |
    | ---- | ---- |
    | `connect`	| Sets the remote host, and optionally the port, for file transfers. | 
    |`get`	| Transfers a file or set of files from the remote host to the local host. | 
    |`put`	| Transfers a file or set of files from the local host onto the remote host. | 
    |`quit`	| Exits vtftp. | 
    |`status` |	Shows the current status of tftp, including the current transfer mode (ascii or binary), connection status, time-out value, and so on. | 
    |`verbose` |	Turns verbose mode, which displays additional information during file transfer, on or off. | 

- TFTP does not have directory listing functionality

- initialisation of ftp service 
    ```bash
    # dependance installation
    sudo apt install vsftpd
    # fichier de configuration
    cat /etc/vsftpd.conf | grep -v "#"
    # liste des utilisateurs
    cat /etc/ftpusers
    ```
- Autres commandes 
    ```bash
    # listing recursif
    ls -R
    # telecharger tous les fichiers disponible
    wget -m --no-passive ftp://<anonymous>:<password>@10.129.14.136/

    # service interaction 
    nc -nv <Tae=rget-IP> 21
    telnet <Tae=rget-IP> 21
    openssl s_client -connect <Tae=rget-IP>:21 -starttls ftp
    ``` 

- Explication config
    
    | Commands |	Description |
    | ---- | ---- |
    | `listen=NO` |Run from inetd or as a standalone daemon? |
    | `listen_ipv6=YES` |	Listen on IPv6 ? |
    | `anonymous_enable=NO` |	Enable Anonymous access? |
    | `local_enable=YES` |	Allow local users to login? |
    | `dirmessage_enable=YES` |	Display active directory messages when users go into certain directories? |
    | `use_localtime=YES`  | Use local time? |
    | `xferlog_enable=YES` |	Activate logging of uploads/downloads? |
    | `connect_from_port_20=YES` |	Connect from port 20? |
    | `secure_chroot_dir=/var/run/vsftpd/empty` |	Name of an empty directory |
    | `pam_service_name=vsftpd` |	This string is the name of the PAM service vsftpd will use. |
    | `rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem` |	The last three options specify the location of the RSA certificate to use for SSL encrypted connections. |
    | `rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key`	 |
    | `ssl_enable=NO`	 |
    | `    anonymous_enable=YES` |	Allowing anonymous login? |
    | `anon_upload_enable=YES` |	Allowing anonymous to upload files? |
    | `anon_mkdir_write_enable=YES` |	Allowing anonymous to create new directories? |
    | `no_anon_password=YES` |	Do not ask anonymous for password? |
    | `anon_root=/home/username/ftp` |	Directory for anonymous. |
    | `write_enable=YES` |	Allow the usage of FTP commands: STOR, DELE, RNFR, RNTO, MKD, RMD, APPE, and SITE? |
    | `dirmessage_enable=YES` |	Show a message when they first enter a new directory? |
    | `chown_uploads=YES` |	Change ownership of anonymously uploaded files? |
    | `chown_username=username` |	User who is given ownership of anonymously uploaded files. |
    | `local_enable=YES` |	Enable local users to login? |
    | `chroot_local_user=YES` |	Place local users into their home directory? |
    | `chroot_list_enable=YES` |	Use a list of local users that will be placed in their home directory? |
    | `hide_ids=YES` |	All user and group information in directory listings will be displayed as "ftp". |
    | `ls_recurse_enable=YES` |	Allows the use of recurse listings |

**Ressources**
 - [Liste des commandes ftp](https://www.smartfile.com/blog/the-ultimate-ftp-commands-list/)
 - [FTP Server return doc ](https://en.wikipedia.org/wiki/List_of_FTP_server_return_codes)
### SMB : Server Message Block

- client-server protocol that regulates access to files and entire directories and other network resources such as printers, routers, or interfaces released for the network.
- Samba server Netbios connection Port: 137, 138, 139 
- CIFS connection port : 445 

```bash
# samba settings
cat /etc/samba/smb.conf | grep -v "#\|\;"
# connect to a share
smbclient -N -L //<Target-IP>
smbclient -N -L //<Target-IP>/<Share-DirName>
# restart 
sudo systemctl restart smbd
```

| Setting |	Description |
| ---- | ---- |
| `[sharename]` |	The name of the network share. |
| `workgroup = WORKGROUP/DOMAIN	Workgroup` | that will appear when clients query. |
| `path = /path/here/` |	The directory to which user is to be given access. |
| `server string = STRING` |	The string that will show up when a connection is initiated. |
| `unix password sync = yes` |	Synchronize the UNIX password with the SMB password? |
| `usershare allow guests = yes` |	Allow non-authenticated users to access defined shared? |
| `map to guest = bad user` |	What to do when a user login request doesn't match a valid UNIX user? |
| `browseable = yes` |	Should this share be shown in the list of available shares? Allow listing available shares in the current share? |
| `guest ok = yes` |	Allow connecting to the service without using a password? |
| `read only = yes` |	Allow users to read files only? Forbid the creation and modification of files? |
| `create mask = 0700` |	What permissions need to be set for newly created files?	 |
| `writable = yes`|	Allow users to create and modify files? |
| `enable privileges = yes`|	Honor privileges assigned to specific SID? |
| `directory mask = 0777` |	What permissions must be assigned to the newly created directories? |
| `logon script = script.sh` |	What script needs to be executed on the user's login? |
| `magic script = script.sh` |	Which script should be executed when the script gets closed? |
| `magic output = script.out` |


**Ressources**