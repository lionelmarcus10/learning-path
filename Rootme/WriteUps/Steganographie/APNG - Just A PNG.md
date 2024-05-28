
# APNG - Just A PNG


## Énoncé

```
Votre collègue blagueur vous met au défi de retrouver le message caché dans cette animation.
```
## Solution

```bash
wget http://challenge01.root-me.org/steganographie/ch21/ch21.apng
```

- Essayons de décomposer le apng en utilisant : 
	- apngdis - deconstruct APNG file into a sequence of PNG frames

```
sudo apt install apngdis

apngdis ch21.apng
```

- Après avoir extrait toutes les frames, on remarque des fichier txt pour le delay

```
delay=70/10
delay=76/10
delay=65/10
delay=71/10
delay=58/10
delay=80/10
delay=51/10
delay=80/10
delay=111/10
delay=70/10
delay=82/10
delay=111/10
delay=71/10
```
- en utilisant des régex, on garde le text entre "=" et "/"
- puis on fait un petit script python pour le convertir en string
```python
x = "70 76 65 71 58 80 51 80 111 70 82 111 71"

print("".join(([chr(int(i)) for i in x.split(" ")]))
```

 - on obtient:  `FLAG:P3PoFRoG`