
# # CISCO - Salted Password


## Énoncé

```
#### Énoncé

L’administrateur réseau de votre entreprise a oublié ses mots de passes d’administration des routeurs. Il dispose cependant d’une sauvegarde de sa startup-config. À l’aide de celle-ci, retrouvez ses mots de passes !

Le flag est le mot de passe enable et le mot de passe administrateur concaténés.

Bon courage !
```

```
{!
version 15.1
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname R1
!
enable secret 5 $1$mERr$A419.HL58lq743wXS4kSM1
!
ip cef
no ipv6 cef
!
username administrator secret 5 $1$mERr$yhf7f2RnC74CxKANvoekD.
!
license udi pid CISCO2911/K9 sn FTX1524V4VG-
!
no ip domain-lookup
!
spanning-tree mode pvst
!
interface GigabitEthernet0/0
 ip address 10.0.0.254 255.255.255.0
 no ip proxy-arp
 duplex auto
 speed auto
!
interface GigabitEthernet0/1
 ip address 11.0.0.1 255.255.255.252
 no ip proxy-arp
 duplex auto
 speed auto
!
interface GigabitEthernet0/2
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface Vlan1
 no ip address
 shutdown
!
router bgp 1
 bgp router-id 1.1.1.1
 bgp log-neighbor-changes
 no synchronization
 neighbor 11.0.0.2 remote-as 2
 network 10.0.0.0 mask 255.255.255.0
!
ip classless
!
ip flow-export version 9
!
no cdp run
!
line con 0
 login local
!
line aux 0
!
line vty 0 4
 login
!
!
!
}
```
## Solution

```
enable secret 5 $1$mERr$A419.HL58lq743wXS4kSM1
username administrator secret 5 $1$mERr$yhf7f2RnC74CxKANvoekD.


```