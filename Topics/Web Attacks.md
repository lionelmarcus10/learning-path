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
        - It can be referenced in an XML document with **&** (e.g. &company;)
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


##### XXE Prevention


