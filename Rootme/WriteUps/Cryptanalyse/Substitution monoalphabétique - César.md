
 # Substitution monoalphabétique - César

## Énoncé

```
tm bcsv qolfp
f'dmvd xuhm exl tgak
hlrkiv sydg hxm
qiswzzwf qrf oqdueqe
dpae resd wndo
liva bu vgtokx sjzk
hmb rqch fqwbg
fmmft seront sntsdr pmsecq

Nous avons intercepté le messager de l’empereur. Il transmettait un message codé à son fils. Cela pourrait être un ordre de guerre ! A vous de le déchiffrer au plus vite.  
Pour valider, il faudra entrer la concaténation des premières lettres de chaque ligne suivie de la concaténation des dernières lettres de chaque ligne (exemple avec le code : tfhqdlhfpkmeokgq).

```
## Solution

- En utilisant un chffrement cesar sur chaque mot, on remarque qu'il correspond à un mot en français.
- J'ai donc fait un script python pour effectuer un decalage sur chaque mot en fonction de sa position dans la phrase 

```python
def chiffrement_cesar(message, decalage):
    resultat = ""
    for char in message:
        # Vérifier si le caractère est une lettre
        if char.isalpha():
            # Déterminer si le caractère est majuscule ou minuscule
            majuscule = char.isupper()
            # Appliquer le décalage et s'assurer que le résultat reste une lettre
            code_ascii = ord(char)
            nouvelle_valeur = (code_ascii - ord('A' if majuscule else 'a') + decalage) % 26
            nouveau_char = chr(nouvelle_valeur + ord('A' if majuscule else 'a'))
            resultat += nouveau_char
        else:
            # Si le caractère n'est pas une lettre, le laisser inchangé
            resultat += char

    return resultat

# mettre la phrase avec des délimiteurs
x ="-t-m bcsv qolf+p+ -f-'dmvd xuhm exl tga+k+ -h-lrkiv sydg hx+m+ -q-iswzzwf qrf oqdueq+e+ -d-pae resd wnd+o+ -l-iva bu vgtokx sjz+k+ -h-mb rqch fqwb+g+ -f-mmft seront sntsdr pmsec+q+"

y = x.replace("'"," ").split(" ")
k = ""
for i in range(len(y)):
    for ele in y[i]:
        k += chiffrement_cesar(ele,i+1) + ""
    k+=" "
print(k)
# ajouter les lettres de début de ligne
j = ""
for i in range(1,len(k)-2):
    if(k[i-1] == "-" and k[i+1] == '-'):
        j += k[i]

# ajouter les lettres de fin de ligne
h = ""
for i in range(len(k)-2,1,-1):
    if(k[i-1] == "+" and k[i+1] == '+'):
        h = k[i] + h
print(j+h)
    


    
# un deux trois j irai dans les bois quatre cinq six cueillir des cerises sept huit neuf dans un panier neuf dix onze douze elles seront toutes rouges 
# ujqcsddessxsffes

```

- Trouvons la correspondance entre les lettres
```
ujqcsddeseffsxss
```