---
tags:
  - BTLO
  - Easy
  - JWT
  - Challenge
---


## Scénario 

```
You’re a senior cyber security engineer and during your shift, we have intercepted/noticed a high privilege actions from unknown source that could be identified as malicious. We have got you the ticket that made these actions.  
You are the one who created the secret for these tickets. Please fix this and submit the low privilege ticket so we can make sure that you deserve this position.  
Here is the ticket:  
  
**eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJmbGFnIjoiQlRMe180X0V5ZXN9IiwiaWF0Ijo5MDAwMDAwMCwibmFtZSI6IkdyZWF0RXhwIiwiYWRtaW4iOnRydWV9.jbkZHll_W17BOALT95JQ17glHBj9nY-oWhT1uiahtv8**
```


#1) Can you identify the name of the token? (Format: String) _(2 points)_

```
JWT
```

#2) What is the structure of this token? (Format: Section.Section.Section) _(2 points)_

`Header.Payload.Signature`

#3) What is the hint you found from this token? (Format: String) _(2 points)_

**en décodant le jwt , on peut obtenir dans la partie payload :** `_4_Eyes`

#4) What is the Secret? (Format: String) _(2 points)_

- Method 1 
```bash
hashcat -a 3 eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJmbGFnIjoiQlRMe180X0V5ZXN9IiwiaWF0Ijo5MDAwMDAwMCwibmFtZSI6IkdyZWF0RXhwIiwiYWRtaW4iOnRydWV9.jbkZHll_W17BOALT95JQ17glHBj9nY-oWhT1uiahtv8 -m 16500 ?a?a?a?a
```
Method 2
```bash
npm install --global jwt-cracker

jwt-cracker -t <token> [-a <alphabet>] [--max <maxLength>] [-d <dictionaryFilePath>] [-f]

#execution
# creation dictionnary string
keyboard_chars="abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890\`\~\!\@\#\$\%\^\&\*\(\)-_=+\[\{\]\}\\\|\;\:\'\",<.>/? "
# lancement du script
jwt-cracker -t eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJmbGFnIjoiQlRMe180X0V5ZXN9IiwiaWF0Ijo5MDAwMDAwMCwibmFtZSI6IkdyZWF0RXhwIiwiYWRtaW4iOnRydWV9.jbkZHll_W17BOALT95JQ17glHBj9nY-oWhT1uiahtv8  -a "$keyboard_chars" --max 7
```

secret : `bT!0`

#5) Can you generate a new verified signature ticket with a low privilege? (Format: String.String.String) _(2 points)_

*mettre admin à false*

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhZG1pbiI6ZmFsc2V9.hWEyQRQHppCYDwbRt3fpbPVVI3e-0z60WudOSD7h5v4
```


