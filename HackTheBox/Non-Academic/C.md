# CozyHosting

## 

```bash
sudo vim /etc/hosts
# ajouter à la dernière ligne
10.129.59.194 cozyhosting.htb 
```

## Network Scan
- Lancer un scan nmap
    ```bash
    nmap -sV -sC 10.129.59.194 -F --script banner
    ```
    - image

