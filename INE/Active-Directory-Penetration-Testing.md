# Active Directory Penetration Testing

## <ins>I - Recap</ins>:

### <ins>1 - Active Directory Primer</ins>

- [Introduction to active directory](https://www.youtube.com/watch?v=GfqsFtmJQg0&ab_channel=ServerAcademy)
- [Active directory groups](https://www.youtube.com/watch?v=nJPxub2dnSQ&ab_channel=Netwrix)
- [Active directory security groups](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-groups)
- [Intro to Active Directory Organizations Units](https://www.youtube.com/watch?v=WSFKm8bMh14&ab_channel=BlueTechPanda)
- [Active Directory Organizations Units explained](https://www.youtube.com/watch?v=zosB3bPyYkA&ab_channel=Make8Real2017)
- [Active Directory Organizations Units explained 2](https://www.youtube.com/watch?v=hOTj5rFxptU&t=38s&ab_channel=BurningIceTech)
- [Introduction to Active Directory Authentication](https://www.youtube.com/watch?v=0IRFgzD1MBA&ab_channel=JumpCloud)
- [Kerberos vs LDAP](https://www.youtube.com/watch?v=Xjpi8xYqPcY&ab_channel=JumpCloud)
- [Kerberos Explained](https://youtube.com/watch?v=5N242XcKAsM&ab_channel=DestinationCertification)
- [Understanding Active Directory Domains, Trees, and Forest](https://www.youtube.com/watch?v=7xOUsirYLYU&ab_channel=JohnChristopher)
- [Active Directory Domain, Trees, Forest and Trust](https://www.youtube.com/watch?v=Whh3kPS0FdA&ab_channel=ITFreeTraining)
- [Active directory quizz](https://quizlet.com/509473672/system-administration-and-it-infrastructure-services-week-4-directory-services-flash-cards/)
- [Intro to AD recap](https://rootdse.org/posts/active-directory-basics-1/)

### <ins>2 - Active Directory Penetration Testing</ins>

#### A- Methodology

- ![AD Methodology](https://camo.githubusercontent.com/e86663235b4690432fc71048a0c53929ac2768171e31f45069a143b89d17b0c3/68747470733a2f2f692e696d6775722e636f6d2f414d5a394d4d352e6a706567)
- [AD github cheat-sheet](https://github.com/S1ckB0y1337/Active-Directory-Exploitation-Cheat-Sheet)
- [AD Orange cyberdefense mind maps](https://github.com/Orange-Cyberdefense/ocd-mindmaps?tab=readme-ov-file)

#### B - AD Enumeration

- [AD remote password spray](https://www.youtube.com/watch?v=L7qr1uBZ_I0&ab_channel=InfiniteLogins)
- [AD Password spray 2](https://www.netwrix.com/password_spraying_tutorial_defense.html)
- [JohnHammond AD Enumeration automated with BloodHound on Kali](https://www.youtube.com/watch?v=yp8fw72oQvY&ab_channel=JohnHammond)
- [Hackersploit AD Enumeration automated with BloodHound](https://www.youtube.com/watch?v=sGO4F23Xik4&ab_channel=HackerSploit)
- [Hackersploit AD Enumeration with PowerView](https://www.youtube.com/watch?v=n3Ow_LKanMo&ab_channel=HackerSploit)
- [AD powerview enumeration blog](https://nored0x.github.io/red-teaming/active-directory-domain-enumeration-part-1/)

#### C - AD Privilege Escalation

- []()
- []()
- []()
- []()
- []()
- []()

## Notes

```powershell
# laptop name
hostname
# current user (whoami)
net user
# with domain
net user domain
# get ad users
Get-ADUser -Filter *
# enum org
Get-ADOrganizationalUnit -Filter *
# enum users
Get-DomainUser | Select-Object -ExpandProperty cn | Out-file user.txt


# /Tools/Scripts


## spray password
#### step 1
powershell -ep bypass
. .\PowerView.ps1
git clone https://github.com/dafthack/DomainPasswordSpray
#### Step 2
. .\DomainPasswordSpray.ps1
Invoke-DomainPasswordSpray -UserList <UserFilePath.txt> -Password <Password> -Verbose



## Automated  Enumeration with bloodhound in windows
## load alias
powershell -ep bypass
cd  .\Bloodhound\ressources\app\collectors
. .\SharpHound.ps1
# Perform enumeration => result zip file
Invoke-BloodHound -collectionMethod All


# Enumeration with PowerView
powershell -ep bypass
. .\PowerView.ps1
# current user and all infos
whoami /all
# current user privileges
whoami /priv
# current user group
whoami /groups
# local user that has admin rights
Find-LocalAdminAccess
# List of local users
Get-LocalUser
# policy settings
net accounts
# local groups
Get-LocalGroup
# local group members
Get-LocalGroupMember <MemberName>
# network config and infos mapped
Get-NetIPConfiguration | ft InterfaceAlias,InterfaceDescription,IPv4Address
Get-DnsClientServerAddress -AddressFamily IPv4
### domain
Get-Domain
### Parent Domain
Get-Domain -Domain <ParentDomainName>
### Domain SID
Get-DomainSID
### Domain Policiy
(Get-DomainPolicy)."SystemAccess"
(Get-DomainPolicy)."kerberospolicy"
### Domain Controller
Get-DomainController -Domain <DomainName>
### Domain User
Get-DomainUser  | select samaccountname, objectsid # -Identity <samaccountname> -Properties <PropertyName,...> | Formatlist
### Computers on network
Get-NetComputer # -Domain <DomainName>
# Domain Group
Get-NetGroup '<samaccountname>'
# Domain group members
Get-NetGroupMember '<samaccountname>'
# Domain user all groups membership
Get-NetGroup -UserName "<Username>"
# domain shares accessed by current user
Find-DomainShare -ComputerName <ComputerName> -verbose
# active shares on localhost
Get-NetShare

# domain Group policy
Get-NetGPO
# domain Organisation Units
Get-NetOU
# domain trusts
Get-NetDomainTrust
# domain forest current -other
Get-NetForest -Forest <otherForestName>
# map trust of forest
Get-NetForestTrust -Forest <TrustDomain>
# all domain in current forest -other
Get-NetForestDomain -Forest <otherForestName>
# enum all trust
Get-DomainTrustMapping
# List all access control list
Get-ObjectAcl -SamAccountName "<samaccountname>" -ResolveGUIDs # -Identity <samaccountname>
#nteresting ACEs
Find-InterestingDomainAcl -ResolveGUIDs
#  specific Active Directory right associated with the specified object
Get-ObjectAcl -SamAccountName WAYNE_GIBBS -ResolveGUIDs
# Identify user accounts with non-null Service Principal Name (SPN)
Get-NetUser -SPN | select samaccountname, serviceprincipalname
# Find AS-REP roastable accounts
Get-NetUser -PreauthNotRequired | select samaccountname, useraccountcontrol

```

## Ressources

- [Windows pentest tools](https://github.com/S3cur3Th1sSh1t/Pentest-Tools)
- [AD pentest gitbook](https://www.thehacker.recipes/ad/movement/kerberos/delegations/constrained)
- [AD attack Defense](https://github.com/infosecn1nja/AD-Attack-Defense)
- [AD github cheat-sheet](https://github.com/S1ckB0y1337/Active-Directory-Exploitation-Cheat-Sheet)
- [AD Orange cyberdefense mind maps](https://github.com/Orange-Cyberdefense/ocd-mindmaps?tab=readme-ov-file)
- [Password Spray script (DomainPasswordSpray.ps1)](https://github.com/dafthack/DomainPasswordSpray)
- [AD Gitbook pentest](https://www.ired.team/offensive-security-experiments/active-directory-kerberos-abuse/active-directory-password-spraying)

- [BloodHound](https://github.com/BloodHoundAD/BloodHound)
- [BloodHound usage](https://bloodhound.readthedocs.io/en/latest/index.html) -[Bad Blood (for analyse / blue team)](https://github.com/davidprowe/BadBlood)
- [AD powerview enumeration](https://nored0x.github.io/red-teaming/active-directory-domain-enumeration-part-1/)
- [Payload all the things AD attacks](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Active%20Directory%20Attack.md)
