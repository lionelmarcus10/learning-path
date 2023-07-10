# CROSS-SITE SCRIPTING (XSS)  

###### 3 types d'injection xss :
|**Type**|**Description**|
|---|---|
|Stored (Persistent) XSS|The most critical type of XSS, which occurs when user input is stored on the back-end database and then displayed upon retrieval (e.g., posts or comments)|
|Reflected (Non-Persistent) XSS|Occurs when user input is displayed on the page after being processed by the backend server, but without being stored (e.g., search result or error message)|
|DOM-based XSS|Another Non-Persistent XSS type that occurs when user input is directly shown in the browser and is completely processed on the client-side, without reaching the back-end server (e.g., through client-side HTTP parameters or anchor tags)|

## Bases

###### Stored (Persistent) XSS
* elle envoi l'input au backend 
###### Reflected XSS:
* elle se fait souvent ou parfois dans les URL des requettes , ex :
    ```URL
    http://159.65.81.48:30264/index.php?task=%27%3Cscript%3Ealert(document.cookie)%3C/script%3E%27
    ```
* elle envoi l'input au backend 
* non persistant 
###### DOM XSS
* exécuté en front
* non persistant
```html
<img src="" onerror=alert(window.origin)>
```

###### Vulnérability scanner 
* Burp Suite
* Zap
* Nessus


###### Tools de découverte

* XSS Strike
    ```bash
    git clone https://github.com/s0md3v/XSStrike.git
    cd XSStrike
    pip install -r requirements.txt
    python xsstrike.py

    python xsstrike.py -u "http://SERVER_IP:PORT/index.php?<Champ>=<Value>"
    ```
* Brute XSS

* XSSer

## XSS Attack
###### Defzcing attacks

###### Phishing Attack
```javascript

```

## Ressources
##### Payloads
* [Payload all the things](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS%20Injection/README.md)
* [Payload Box](https://github.com/payloadbox/xss-payload-list)
