# H2ServerTechnology

##### Table of Contents  
[Indledning](#Indledning)  
[Netværksdiagram](#Netværksdiagram)  
[Interface og IP-adresse tabel](#Interface-og-IP-adresse-tabel)  
[Hardware og software specifikationer](#Hardware-og-software-specifikationer)  
[Bruger Tabel](#Bruger-tabel)  
[Konfigurationsvalg af fysisk server](#Konfigurationsvalg-af-fysisk-server)  
[Konklusion](#Konklusion)

## Indledning
Vi skal opsætte en et netværk, hvor det er en server og en klient på netværket. Serveren skal have nogle roller;
Active Directory (Domain Controller)
DHCP server
DNS server
IIS server

Domain Controlleren skal have et domæne der hedder servertek.local.

## Netværksdiagram
Skærmbillede fra Packet Tracer…

## Interface og IP-adresse tabel
======

## Hardware og software specifikationer
### Server
OS: Windows Server 2022
Enhed: MooseServer
CPU: Intel(R) Xeon(R) CPU E3-1220 v6 @ 3.00Ghz
RAM: 16GB
Storage: 228GB SSD, 1TB HDD
### Router
Enhed: LINKSYS E900
### Klient computer
OS: Windows 10 Pro (21H1, build: 19043.928)
Enhed: Moose01
CPU: Intel(R) Core™ 2 Duo CPU T9400 @ 2.53Ghz
RAM: 4GB
Storage: 128GB

## Bruger Tabel
| Enhed           | OU                              | Brugernavn    | Adgangskode |
| --------------- | ------------------------------- | -----------   | ----------- |
| MooseServer     | Users/                          | Administrator | DataIT2021! |
| Moose01         | centered                        | PC-01         | DataIT2021! |
| AD konto        | servertek/Users/DevOps/         | Ops1          | DataIT2021! |
| AD konto        | servertek/Users/DevOps/         | Ops2          | DataIT2021! |
| AD konto        | servertek/Users/Developers/     | Dev1          | DataIT2021! |
| AD konto        | servertek/Users/Developers/     | Dev2          | DataIT2021! |
| AD konto        | servertek/Users/Designer/       | Des1          | DataIT2021! |
| AD konto        | servertek/Users/Designer/       | Des2          | DataIT2021! |
| AD konto        | servertek/Users/Administration/ | admin         | DataIT2021! |

## Konfigurationsvalg af fysisk server
Vi har ændret navnet på serveren til MooseServer, fordi vi ville have et Moose tema :)

Directory Services Restore Mode password (DSRM): DataIT2021!

## Konklusion
Vi havde en del problemer med opsætning af Microtic Hex routeren og endte med at bytte den til en Linksys e900. Vi havde en snak med SKP folkene, der fortalte at de fik rigtig mange klager over Microtik routerne.
