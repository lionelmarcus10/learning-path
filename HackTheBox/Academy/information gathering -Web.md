# INFORMATION GATHERING - WEB EDITION

###### Collecte passive sur le domaine :

    whois <domainurl>

* [Netcraft](https://sitereport.netcraft.com)
* [Way Back Machine](https://web.archive.org/)
    ```bash
    #installation
    go install github.com/tomnomnom/waybackurls@latest
    # utilisation
    waybackurls -dates <url site> > waybackurls.txt
    ```

###### Collecte passive sur le dns :

* **NSLOOKUP**
    ```bash
    nslookup @<nameserver/IP>
    ```
    * **-query=PTR** : PTR Records for an IP Address
    * **-query=mx** : for mx, server names...

* **DIG**
    ```bash
    dig  <nameserver> @<IP> -query=A 
    ```
    * **-query=A** : A Records for a Subdomain
    * **mx**  : for mx, server names...

###### Collecte passive sur les sous-domaines :
* **VirusTotal**
* https://censys.io
* https://crt.sh

```bash
export TARGET="<ma target>"
export PORT="<mon port>"
# automatisé
curl -s "https://crt.sh/?q=${TARGET}&output=json" | jq -r '.[] | "\(.name_value)\n\(.common_name)"' | sort -u > "${TARGET}_crt.sh.txt"
# performance manuel :
openssl s_client -ign_eof 2>/dev/null <<<$'HEAD / HTTP/1.0\r\n\r' -connect "${TARGET}:${PORT}" | openssl x509 -noout -text -in - | grep 'DNS' | sed -e 's|DNS:|\n|g' -e 's|^\*.*||g' | tr -d ',' | sort -u
```
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