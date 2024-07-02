# Web Application Penetration Testing

## <ins>I - Recap</ins>:

### <ins>1 - Passive Information Gathering</ins>

- [What is information gathering](https://medium.com/@neharidha5/information-gathering-e88f5b4f8286)
- [What is information gathering article 2](https://www.recordedfuture.com/threat-intelligence-101/intelligence-sources-collection/information-gathering)
- [Information gathering 1](https://www.youtube.com/watch?v=BWaGnsRirtU&ab_channel=HackerSploit)
- [How to find more vulnerabilities using the OWASP WSTG!](https://www.youtube.com/watch?v=52w4JXRYzhY&ab_channel=Hacksplained)
- [(WSTG) to improve your web application security](https://medium.com/@cuncis/how-to-use-owasp-web-security-testing-guide-wstg-to-improve-your-web-application-security-94e8b17d589d)

- [How to Use the WHOIS Command to Lookup Ip and Domain Name Information](https://www.youtube.com/watch?v=U-VG_E4kxWI&ab_channel=TonyTeachesTech)
- [DNS enumeration and zone transfers](https://www.youtube.com/watch?v=EpfHhEGz35I&ab_channel=HackerSploit)
- [Reconnaissance Part 1](https://www.youtube.com/watch?v=4hfO8NZgbc4&ab_channel=CyberSecurityRanger)

- [Netcraft (article)](https://medium.com/@pentesterclubpvtltd/find-the-companys-domains-and-sub-domains-using-netcraft-9033ab06b276)
- [Netcraft (video)](https://www.youtube.com/watch?v=PdOP6ZosaJU&ab_channel=CyberSecWithDesire)
- [DNS explained step by step](https://www.youtube.com/playlist?list=PL_vyuxE-AO-DD94NKcCqd4iqwy5ah_pwq)
- [Passive DNS enumeration with dnsdumpster](https://www.youtube.com/watch?v=o0P7CxGAPHY&ab_channel=BestChannel)
- [Passive DNS enumeration with dnsrecon](https://www.youtube.com/watch?v=JMKgfxtxVrk&ab_channel=ProfessorK)
- [How Attackers Can Misuse Sitemaps](https://medium.com/@adimenia/how-attackers-can-misuse-sitemaps-to-enumerate-users-and-discover-sensitive-information-361a5065857a)
- [Web app fingerprinting](https://www.youtube.com/watch?v=BDsRy9EzBVg&ab_channel=NullByte)

### <ins>2 - Active Information Gathering</ins>

- [Crawling with burp suite (8min)](https://www.youtube.com/watch?v=mI7fj5fdsB0&ab_channel=Hacksplained)
- [Crawling with burp suite (quiet video: 3min)](https://www.youtube.com/watch?v=6xbhPkGl7Qg&ab_channel=PentesterAcademyTV)
- [Spider (automatic crawl) with OWASP ZAP](https://www.youtube.com/watch?v=6hWWXIduFSg&ab_channel=PentesterAcademyTV)
- [Fingerprinting with nmap](https://www.youtube.com/watch?v=AKWUllE9MOg&ab_channel=HackerSploit)
- [Fingerprinting with msfconsole](https://www.youtube.com/watch?v=adzahJ0vmDE&ab_channel=HackerSploit)
- [Directory enumeration (web fuzzing tools)](https://medium.com/@ucihamadara/directory-enumeration-1ba91012dcf8#:~:text=Directory%20Enumeration%20is%20a%20technique,%2C%20dirbuster%2C%20gobuster%20and%20dirsearch.)
- [Enumeration with Gobuster](https://patchthenet.com/blog/using-gobuster-to-find-hidden-web-content/)
- [Gobuster in video](https://www.youtube.com/watch?v=QTAKmINxKZA&ab_channel=SystemExploited)

### <ins>3 - XSS</ins>

- [Reflected XSS explained](https://www.youtube.com/watch?v=T0vxdqvW9fU&ab_channel=AndrewHoffman)
- [CVE-2018-9034 XSS WordPress Relevanssi Plugin (4.0.4) ](https://www.exploit-db.com/exploits/44366)
- [Stored XSS explained](https://www.youtube.com/watch?v=ifTsiU45-7A&ab_channel=AndrewHoffman)
- [exploit MyBB Plugin Downloads 2.0.3](https://www.exploit-db.com/exploits/44400)
- [DOM XSS explained](https://www.youtube.com/watch?v=_3Wgx1FabIo&ab_channel=AndrewHoffman)
- [All XSS types explained with Hackersploit](https://www.youtube.com/watch?v=SHmQ3sQFeLE&ab_channel=HackerSploit)
- [Learn XSS with port swigger](https://portswigger.net/web-security/cross-site-scripting#what-is-cross-site-scripting-xss)

### <ins>3 - SQL Injection</ins>

[SQL injection with OWASP ZAP](https://www.youtube.com/watch?v=S2GggY6tcZs&ab_channel=TheTestTherapist)
[SQL injection with OWASP ZAP (Hackersploit)](https://www.youtube.com/watch?v=XaAcRpXl9nA&ab_channel=Pasukan08Prabowo)

### <ins>Ressources</ins>

- [OWASP Top 10 checklist ](https://github.com/tanprathan/OWASP-Testing-Checklist)
- [Web footprinting msfconsole modules (documentation)](https://www.offsec.com/metasploit-unleashed/scanner-http-auxiliary-modules/)

## <ins>II - Notes</ins>:

###### OWASP Web Security Testing Guide:

- How to use WSTG
  - Donwload the last WSTG document
  - Download OWASP checklist
  - Open the documents and follow steps to conduct the pentest phase you want

###### Others

```
# general
https://sitereport.netcraft.com/
# whois
whois <IP|url>
# host
host <url|ip>
```

```bash
# dns enumeration
dnsrecon -d <domainName>
# alternative
dnsdumpster.com
```

- Meta files

  ```bash
  # webserver meta files
  /sitemap.xml
  # robots
  /robots.txt
  ```

  - Open files specified on theses files in order to get more informations

- Fingerprinting

  ```bash
  # addons in firefox
  BuiltWith
  Wappalizer

  # cmd
  whatweb <Url>
  ```

- Crawling and spidering

  ```bash
  # Burp
  1.active the proxy on your browser and navigate on pages.
  2. After, go to target (burp suite) > and browse sub sections

  # OWASP ZAP
  1. Active proxy and navigate
  2. See on the left side the website sitemap
  3. Then you could go on tool > spider > (you config the spider) > (lunch the attack)

  ```

- Fingerprinting the app

  ```bash
  # NMAP
  nmap -F <ip> --script banner -sV
  # nmap web script: --script http-enum

  # MSFCONSOLE
  # http version and server( web footprinting)
  use auxiliary/scanner/http/http_version
  # directory
  use auxiliary/scanner/http/dir_scanner
  # file
  use auxiliary/scanner/http/files_dir
  # robots
  use auxiliary/scanner/http/robots_txt
  # config params and lunch the auxiliary

  # WGET
  wget <URL>

  # CURL
  curl <URL>

  # lynx
  lynx <IP>
  # browsh
  browsh <IP>
  # web enumeration/fuzzing with dirb
  dirb <URL> <DicoLink (directory) | usr/share/metasploit-framework/data/wordlists/directory.txt>
  ```

- Web server scanning with Nikto

  ```bash
  # host => --host | -h
  nikto -h <IP>
  # export file
  nikto -h <IP> -o <file.html> -Format htm
  ```

- Go buster for file discovery

  ```bash
  gobuster dir -u <URL/> -w <DicoFilePath>
  ```

- Fingerprinting automation with amass

  ```bash
  # domain enum ( passive )
  amass <enum| intel | ....> -d <domainName> -passive -src -ip -dir <outputDir>
  # active -active

  # report
  amass viz -dir <AmassInputResultPath> -d3

  ```

- XSS
  - Wordpress
    ```bash
    wpscan  --api-token <WPVulnDBApi>
    # wordpress scan
    wpscan --url <URL> --enumerate p --plugins-detection <mode (aggressive)>
    ```
  - MyBB
    ```bash
    git clone https://github.com/0xB9/MyBBscan
    pip install huepy
    ./scan.py
    ```
- SQLI

  ```bash

  ```
