# Enumeration : Web et Réseaux

>Enumeration is collecting as much information as possible. The more information we have, the easier it will be for us to find vectors of attack.

-----
###### whatweb
```bash
# enumeration passive
whatweb --norerror <ip>
```
----
###### dirbuster et dirb
----
###### gobuster
```bash
# enumerate directories
gobuster dir -u <link> -w <localLinkDico>
# dns enumeration
gobuster dns -d <link> -w <dico>
```
----
###### zaproxy (zap)
* Similaire à burp suit  
* Dispose de fuzzer et scanner et proxy 
* option interessante : Zap Processor <=
    * selectionner un element dans le proxy
    * option fuzzer
    * add payload 
    * add processor ( transforme/converitl'élement ex: md5 avant de le fuzz comme intruder burp )
* **ZAP scanner**
    ```bash
    # une fois dans l'historique :  Attack Spider 
    ou
    #spider start dans le navigateur 
    # passive scan :  cliquer sur les alerts
    # active scan : click active scan buttun dans le navigateur
    # visualisation : ZAP HUD
    # rapport generation : Report>Generate HTML Report
    ```
-----
###### Burp Suite :
- intruder 
    * pour faire du fuzzing 

    ```bash
    # payload processing 
    # skip payload ...
    ^\..*$
    # ce code permet de skip le payload qui ont des extensions .html .... dans le cas où on veut eviter de les utiliser dans le dico
    ```
- Scanner : Pro-feature
    ```bash
    # 1 - click droit + add to scope 
    # 2 - après le scope
    # 3 - click droit +  passive scan
    # 3 - active scan <= click droir + Do active scan
    # 4 - dashboard => new scan
    # 4 - scan configuration => new => select from library

    # result => View detail => Issue activity 

    # report : Target>Site map, right-click on our target, and select Issue>Report issues for this host
    ```
----
###### [_Ffuf_](../Tools/Ffuf.md)
