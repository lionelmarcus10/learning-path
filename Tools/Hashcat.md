# Hashcat

`Toujours utiliser hashid pour reconnaître le type de hash` 
---

--- 
```bash
# optimisation mode
hashcat -O
hashcat -m <ModeNumber>
```
---

### Encryption

```bash
# hash en md5
 echo -n "<Fichier ou Pass>" | md5sum
```

```python
from pwn import xor

# encrypt
xor("<PassToEncrypt>", "<Secret>")
```

### Identification des hashes

```bash
pip install hashid

# Detecter un hash
hashid '<HASH>'

# Un exemple
hashcat --example-hashes | less
```

| **Mode** (`-a`)  | **Description**   |
| --------------|-------------------|
|  `0` | Straight
|  `1` | Combination
|  `3` | Brute-force
|  `6` | Hybrid Wordlist + Mask
|  `7` | Hybrid Mask + Wordlist


---
## Hashcat Attack

### Basic attack 
```bash
# format
hashcat -a 0 -m <hash type> <hash file> <wordlist>
```

### Combinartion mode attack

**`The combination attack modes take in two wordlists as input and create combinations from them`**

```bash
# exemple of combination format
hashcat -a 1 -m <hash type> <hash file> <wordlist1> <wordlist2>
```

### Mask attack

| **Placeholder**  | **Meaning**   |
| --------------|-------------------|
|`?l`|	lower-case ASCII letters (a-z)|
|`?u`|	upper-case ASCII letters (A-Z)|
|`?d`|	digits (0-9)|
|`?h`|	0123456789abcdef|
|`?H`|	0123456789ABCDEF|
|`?s`|	special characters («space»!"#$%&'()*+,-./:;<=>?@[]^_`{|
|`?a`|	?l?u?d?s|
|`?b`|	0x00 - 0xff|

```bash
# Mask attack format
hashcat -a 3 -m <HashType> <HASH -1 01 '<PASSKNOWN>PLACEHOLDER== MASK'
```

### Hybrid ( Mask and Other ) 
```bash
# Hashcat - Hybrid Attack using Wordlists
hashcat -a 6 -m 0 <Hybrid_hash> <Wordlist> '<MASK>'

# if prefix is known
# Hashcat - Hybrid Attack using Wordlists with Masks
hashcat -a 7 -m 0 <hybrid_hash_prefix> -1 01 '<MASK>' <Wordlist>
```

### Rule attack

```bash
hashcat -m <typeHash> -a <Mode> <HASH> <wordlistURL> -r rules file=<RuleFile> | -r <RuleFile>
```

---
# Wordlists

###### crunch : creation de dictinnaire custom 
```bash
# "-t" specify pattern 
# "@" lower case characters
# "," upper case characters
# "%" numbers
# "^" symbols.
# -d amount of repetition
# exemple de pattern 'ILFREIGHT201%@@@@'
crunch <minimum length> <maximum length> <charset> -t <pattern> -o <output file>
```

###### Cupp : Social engeneering password generator 
```bash
python3 cupp.py -i
```
###### kwprocessor : Wordlist by keyword
```bash
git clone https://github.com/hashcat/kwprocessor
cd kwprocessor
make

# ex : 
kwp -s 1 basechars/full.base keymaps/en-us.keymap  routes/2-to-10-max-3-direction-changes.route
```
###### Princeprocessor : Prince Algorithm
* The program takes in a wordlist and creates chains of words taken from this wordlist
```bash
# installation
wget https://github.com/hashcat/princeprocessor/releases/download/v0.22/princeprocessor-0.22.7z
7z x princeprocessor-0.22.7z
cd princeprocessor-0.22
./pp64.bin -h

# find number of combination
./pp64.bin --keyspace < words

# Forming Wordlist
./pp64.bin -o wordlist.txt < words

# Password Length Limits
--pw-min=<Number> --pw-max=<Number>
# repetition
--elem-cnt-min=3
```

###### CeWL 
* It spiders and scrapes a website and creates a list of the words that are present

```bash
cewl -d <depth to spider> -m <minimum word length> -w <output wordlist> <url of website>

# email extraction : -e
```

###### Hashcat-utils :  maskprocessor
* create wordlists using a given mask
```bash
# exemple
/mp64.bin Welcome?s
```
---

## Ressources 

#### Dictionnaires 

- Rockyou 
- Seclists
    * Rockyou in seclists : `/opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt` 
- [ CrackStation's Password Cracking Dictionary](https://crackstation.net/crackstation-wordlist-password-cracking-dictionary.htm)

#### Rules

- [OneRuleToRuleThemAll](https://github.com/NotSoSecure/password_cracking_rules/blob/master/OneRuleToRuleThemAll.rule)

##### Other repository

- [Cupp](https://github.com/Mebus/cupp)
- [ maskprocessor](https://github.com/hashcat/maskprocessor)