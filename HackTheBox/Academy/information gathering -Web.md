# INFORMATION GATHERING - WEB EDITION
-----
###### Collecte passive sur le domaine :

    whois <domainurl>
    whatweb <ip> --no-errors
    robots.txt

* [Netcraft](https://sitereport.netcraft.com)
* [Way Back Machine](https://web.archive.org/)
    ```bash
    #installation
    go install github.com/tomnomnom/waybackurls@latest
    # utilisation
    waybackurls -dates <url site> > waybackurls.txt
    ```
-----
###### Collecte passive sur le dns :

* **NSLOOKUP**
    ```bash
    nslookup @<nameserver/IP>
    # subdomain 
     nslookup -type=any -query=AXFR <serverName> <IP>  # grace a nslookup -NS
    ```
    * **-query=PTR** : PTR Records for an IP Address
    * **-query=mx** : for mx, server names...
    * **-NS** : nameserver
    * **-query=AXFR** : trouver subdomains

* **DIG**
    ```bash
    dig  <nameserver> @<IP> -query=A 
    # subdomain
    dig @<IP> axfr inlanefreight.htb
    # subdomain specific
    dig @<IP> axfr <subdomain> <subdomain2>
    ```
    * **-query=A** : A Records for a Subdomain
    * **mx**  : for mx, server names...
------
###### Collecte passive sur les sous-domaines :

* **VirusTotal**
* https://censys.io
* https://crt.sh
* https://hackertarget.com/zone-transfer/

```bash
export TARGET="<ma target>"
export PORT="<mon port>"
# automatisé
curl -s "https://crt.sh/?q=${TARGET}&output=json" | jq -r '.[] | "\(.name_value)\n\(.common_name)"' | sort -u > "${TARGET}_crt.sh.txt"
# performance manuel :
openssl s_client -ign_eof 2>/dev/null <<<$'HEAD / HTTP/1.0\r\n\r' -connect "${TARGET}:${PORT}" | openssl x509 -noout -text -in - | grep 'DNS' | sed -e 's|DNS:|\n|g' -e 's|^\*.*||g' | tr -d ',' | sort -u

gobuster dns -q -r "${NS}" -d "${TARGET}" -w "${WORDLIST}" -p ./patterns.txt -o "gobuster_${TARGET}.txt"
# NS ( name server ) WORLIST ( seclist pattern ) TARGET étant des variables bash

ffuf #vhost
```
###### Technologies & headers
_tool_

```bash
# use wappalyzer extension or :
whatweb -a3 <link or localvhostlink>
# si on utilise un vhost random.link.com, on doit le renseigner dans etc/hosts
# ex: 127.11.12.14 random.link.com etc.link.com
```

_headers_

```bash
# header info
curl -I "http://${TARGET}"
```
_VHOST fuzzing_
```bash
# get all the page
curl -s <link(http://10.10....)> -H "Host: <AddressName(google.com)>"

# Seclist vhosts : SecLists/Discovery/DNS/namelist.txt

# fuzzing de Vhost avec dictionnaire ./vhosts
cat ./vhosts | while read vhost;do echo "\n********\nFUZZING: ${vhost}\n********";curl -s -I http://192.168.10.10 -H "HOST: ${vhost}.randomtarget.com" | grep "Content-Length: ";done
```

-------
###### Tools
* [The Harvester](https://github.com/laramies/theHarvester)

###### outils de theharvester utils :
![outils utils](../../Ressources/IMG/0-info-gat-web-01.png) 
```bash
# creer un ficheir sourcestxt qui contient :
baidu
bufferoverun
crtsh
hackertarget
otx
projecdiscovery
rapiddns
sublist3r
threatcrowd
trello
urlscan
vhost
virustotal
zoomeye
```

###### utilisation

```bash
export TARGET="<le nom de domaine>"
cat sources.txt | while read source; do theHarvester -d "${TARGET}" -b $source -f "${source}_${TARGET}";done

# extraction d'information
cat *.json | jq -r '.hosts[]' 2>/dev/null | cut -d':' -f 1 | sort -u > "${TARGET}_theHarvester.txt"
```

