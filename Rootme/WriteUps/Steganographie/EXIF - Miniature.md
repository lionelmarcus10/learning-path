

# EXIF - Miniature


## Énoncé

```
Retrouver le mot de passe caché dans cette image au format JPG.
```


## Solution
```bash
wget http://challenge01.root-me.org/steganographie/ch10/ch10.jpg

exiftool ch10.jpg
```

on obtient un petit message en bas du résultat : 

```text
Thumbnail Image: (Binary data 41506 bytes, use -b option to extract)
```

```
 exiftool -b  -ThumbnailImage ch10.jpg  > 01.jpg
 exiftool -b  -ThumbnailImage 01.jpg  > 02.jpg
```

- En lisant le second la deuxième vignette, on retrouve le flag qui est : B33r1sG00d!