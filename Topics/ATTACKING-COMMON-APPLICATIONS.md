# ATTACKING COMMON APPLICATIONS  

## Information Gathering : Discovery and Enumeration/

#### Screenshot report tool
```bash
# intall eyewitness
sudo apt install eyewitness
eyewitness -h

eyewitness --web -x <NmapDiscoveryOutputXMLFile> -d inlanefreight_eyewitness

# get aquatone for installation
 wget https://github.com/michenriksen/aquatone/releases/download/v1.7.0/aquatone_linux_amd64_1.7.0.zip
# unzip
unzip aquatone_linux_amd64_1.7.0.zip 
# usage
cat <NmapDiscoveryOutputXMLFile> | ./aquatone -nmap
```

###### Methodologie
```bash
#1 scope file 
<ScopeList>
<Main Adress.com>
<Main IP>

#2 lancer scan sur tout le scope
nmap -p- --open -oA <resultName> -iL<ScopeFile>

#3 lancer le scan sur la main IP
nmap -p- <MainIP> -sV

#4 utiliser eyewitness | aquatone

#5  interpretation : report page of aquatone |  eyewitness
```

## Content Management Systems (CMS) Attack

### <h3 style='text-align: center'>Wordpress</h3>

###### Methodologie
```bash
# Manual automation
#0 identify wordpress 
curl -s http://<Link> | grep WordPress 
# identify theme
| grep themes

#1 browse /robots.txt

#2 visit /wp-admin 

#3 confirm presence of /wp-content | wp-content/themes 

#4 identify connection page and try to log manually ( root admin ...)


# Automated ennumeration 
# install wpscan
sudo gem install wpscan
# scan website
sudo wpscan --url http://<Link> --enumerate --api-token <WPVulnDBApi>
# number of thread : -t <Number>

# authentification attack bruteforce
sudo wpscan --password-attack xmlrpc -t 20 -U <Username> -P <PasswordDictionnary> --url http://<WebsiteLink>
```

### <h3 style='text-align: center'>Joomla</h3>