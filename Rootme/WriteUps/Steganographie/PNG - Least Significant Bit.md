

# PNG - Least Significant Bit


## Énoncé 


## Solution

```bash
wget http://challenge01.root-me.org/steganographie/ch9/ch9.png
```

- Après avoir cherché des outils de lsb sur internet, on tombe sur :
  
```bash
pip install tinyscript

wget https://gist.githubusercontent.com/dhondta/d2151c82dcd9a610a7380df1c6a0272c/raw/stegolsb.py && chmod +x stegolsb.py && sudo mv stegolsb.py /usr/bin/stegolsb

stegolsb -v bruteforce ch9.png
```

