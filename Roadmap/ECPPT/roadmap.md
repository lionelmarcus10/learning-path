# ECPPT Roadmap

## <ins> I - Resource Development & Initial Access</ins>

### <ins>1 - PowerShell for Pentesters</ins>

#### Youtube based cours + notes and labs

###### Courses

- [Personal notes (recap)](../../INE/Powershell-for-pentester.md#recap)

###### Labs

> Use **Powershell empire** for these labs

- [THM Empire (recommanded)](https://tryhackme.com/r/room/rppsempire)
- [HTB Blue (recommanded)](https://www.hackthebox.com/machines/blue)
- [HTB Optimum (recommanded)](https://www.hackthebox.com/machines/Optimum)
- [THM Wreath](https://tryhackme.com/r/room/wreath)
- [HTB Access](https://www.hackthebox.com/machines/access)

###### Write ups

- [HTB Blue Ippsec video (important)](https://www.youtube.com/watch?v=YRsfX6DW10E&ab_channel=IppSec)
- [HTB Access Ippsec video (recommanded)](https://www.youtube.com/watch?v=Rr6Oxrj2IjU)
- [HTB Optimum Ippsec video (recommanded)](https://www.youtube.com/watch?v=kWTnVBIpNsE&ab_channel=IppSec)
- Others available on medium and youtube

#### More deeper ( facultatif )

- [Empire module creation](https://www.youtube.com/watch?v=6l4ZIKwzW8U&ab_channel=IppSec)
- [Powersploit ( powershell module for pentest)](https://github.com/PowerShellMafia/PowerSploit/)

- [Havoc C2](https://www.youtube.com/watch?v=ErPKP4Ms28s&ab_channel=JohnHammond)
- [Havoc C2 bypass detection](https://www.youtube.com/watch?v=fQR65pkC8Us&ab_channel=ElevateCyber)
- [HackTrick Powershell](https://book.hacktricks.xyz/windows-hardening/basic-powershell-for-pentesters)
- [Github powershell scripts](https://github.com/Whitecat18/Powershell-Scripts-for-Hackers-and-Pentesters)

### <ins>2 - Client-Side Attacks</ins>

#### Cours and labs

###### Courses

- [Free courses + notes](../../INE/Client-Side-Attacks.md)

###### Labs

- [THM Passive recon](https://tryhackme.com/r/room/passiverecon)
- [OWASP mutillidae II (Perform directory crawling with burp/owasp zap)](https://github.com/webpwnized/mutillidae)
- [Apache recon basics (not free): could find alternative ](https://attackdefense.com/challengedetails?cid=538)
- [OWASP mutillidae II (Perform directory enumeration with Gobuster)](https://github.com/webpwnized/mutillidae)

#### Blog and article ( facultatif but recommanded )

#### More deeper ( facultatif )

- [Pretexting github repo](https://github.com/L4bF0x/PhishingPretexts)

## <ins>II - Web Application Attacks</ins>

### <ins>3 - Web Application Penetration Testing

</ins>

#### Courses and labs

###### Courses

- [Free courses + notes](../../INE/Web-Application-Penetration-Testing.md)

###### Labs

- [THM Content Discovery](https://tryhackme.com/r/room/contentdiscovery)
- [HTB Knife (use nikto)](https://www.hackthebox.com/machines/knife)
- [THM WGEL (use gobuster / ffuf / dirbuster)](https://tryhackme.com/r/room/wgelctf)
- [THM XSS (recommanded)](https://tryhackme.com/r/room/axss)
- [THM SQLi](https://tryhackme.com/r/room/sqlinjectionlm)
- [Port Swigger XSS ( do apprentice level ) labs ](https://portswigger.net/web-security/all-labs#cross-site-scripting)
- [Port Swigger SQLi ( do apprentice and practitionner level ) labs](https://portswigger.net/web-security/all-labs#sql-injection)
- [AttackAndDefense RCE via MySql (optional)](https://attackdefense.com/challengedetails?cid=1910)

###### Labs write-up

#### More deeper:

## <ins>IV - Exploit Development</ins>

### <ins>5 - System Security & x86 Assembly Fundamentals</ins>

#### Courses and labs

###### Courses

- [Free courses + notes](../../INE/System-Security-&-x86-Assembly-Fundamentals.md)

###### Labs

- [x86 Architecture Overview](https://tryhackme.com/r/room/x8664arch)
- [Enhance coding skills by reverse engeneering ( more advaced than the ecppt course)](https://www.youtube.com/watch?v=1d-6Hv1c39c)
- Write, compile and run a hello world program in asm

### <ins>6 - Exploit Development: Buffer Overflows (SEH)</ins>

#### Cours and labs

###### Courses

- [Free courses + notes](../../INE/Buffer-Overflow-SEH.md)

###### Labs

- [Easy chat 3.1: (Necessary)!](https://easy-chat-server.software.informer.com/3.1/)
- [Vulnserver ( TRUN part ) : (Necessary)!](https://github.com/stephenbradshaw/vulnserver)
- [THM Gatekeeper](https://tryhackme.com/r/room/gatekeeper)
- [Vulnserver ( GMON part ) => SEH type Egghunter](https://github.com/stephenbradshaw/vulnserver)

- R 3.4.4 Windows 10 system

  - [Vulnerable app](https://www.exploit-db.com/apps/a642a3de7b5c2602180e73f4c04b4fbd-R-3.4.4-win.exe)

- Easy File Sharing Web Server 7.2 windows
  - [Vulnerable app](https://www.exploit-db.com/apps/60f3ff1f3cd34dec80fba130ea481f31-efssetup.exe)

###### Labs write-up

- [SEH TRUN write up John Hammond (excellent)](https://www.youtube.com/watch?v=yJF0YPd8lDw&ab_channel=JohnHammond)
- [Easy chat write up article](https://www.onsecurity.io/blog/buffer-overflow-easy-chat-server-31/) OR [Easy chat (writeup in youtube)](https://www.youtube.com/watch?v=d_SUycNesDU&ab_channel=VirajDissanayake)

- R 3.4.4 on a 32-bit Windows 10 system

  - [Gitbook article](https://www.ired.team/offensive-security/code-injection-process-injection/binary-exploitation/seh-based-buffer-overflow)
  - [Exploit db](https://www.exploit-db.com/exploits/47122)

- Easy File Sharing Web Server 7.2 windows

  - [Article](https://scriptkidd1e.wordpress.com/2017/01/07/seh-overflow-egg-hunter-in-1-go/)
  - [Exploit db](https://www.exploit-db.com/exploits/40178/)

- [Vulnrver SEH BOF and other BOF write-up ( highly recommanded)](https://zflemingg1.gitbook.io/undergrad-tutorials/walkthroughs-osce/introduction)

- [Vulnserver(GMON)](https://m0chan.github.io/2019/08/21/Win32-Buffer-Overflow-SEH.html#examples)

#### More deeper:

###### General and stack based BOF tuto

- [Buffer overflow with flipthebit ](https://youtube.com/playlist?list=PLdVIvW2RPTRxNdJeBZRcdt1JQJlmQlQMU)

- [Learn BOF with cybermentor](https://www.youtube.com/playlist?list=PLLKT__MCUeix3O0DPbmuaRuR_4Hxo4m3G)

- [Learn BOF with cryptocat](https://www.youtube.com/playlist?list=PLHUKi1UlEgOIc07Rfk2Jgb5fZbxDPec94)

- [BOF step by step with immunity debugger](https://www.cobalt.io/blog/pentester-guide-to-exploiting-buffer-overflow-vulnerabilities)

## <ins>IV - Red Teaming</ins>

### <ins>9 - Active Directory Penetration Testing</ins>

#### Courses and labs

###### Courses

- [Active Directory Penetration Testing notes](../../INE/Active-Directory-Penetration-Testing.md)

###### Labs

- [THM Active Directory Basics](https://tryhackme.com/r/room/winadbasics)
- [THM Postexploit](https://tryhackme.com/r/room/postexploit)
- [Active directory GOAD (more deeper than the program)]()

## Preparation:

### For myself

- Do all labs mentioned in labs sections
