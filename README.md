# H2ServerTechnology

### Serverteknologi opgave af Victor, Michael, Pierre - 21hf43dat2pV - 10/12/2021

##### Table of Contents  
[Indledning](#Indledning)  
[Netværksdiagram](#Netværksdiagram)  
[Interface og IP-adresse tabel](#Interface-og-IP-adresse-tabel)  
[Hardware og software specifikationer](#Hardware-og-software-specifikationer)  
[Bruger Tabel](#Bruger-tabel)  
[Sikkerheds Grupper](#Sikkerheds-Grupper)  
[Konfigurationsvalg af fysisk server](#Konfigurationsvalg-af-fysisk-server)  
[Sekundær DNS server](#Sekundær-DNS-server)  
[Forklaring af DDNS-NetBIOS-WINS-LLMNR](#Forklaring-af-DDNS-NetBIOS-WINS-LLMNR)
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
<details><summary>Bruger Tabel</summary>
<p>
  
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

</p>
</details>

## Sikkerheds Grupper

<details><summary>Sikkerheds Grupper</summary>
<p>
  
| OU                             | Navn                            |
| ------------------------------ | ------------------------------- |
| servertek/Users                | FolderRedirectionUsers          |
| servertek/Users/Administration | Administration                  |
| servertek/Users/Designer       | Designer                        |
| servertek/Users/Developer      | Developer                       |
| servertek/Users/DevOps         | DevOps                          |

</p>
</details>

## Konfigurationsvalg af fysisk server
Vi har ændret navnet på serveren til MooseServer, fordi vi ville have et Moose tema :)

Directory Services Restore Mode password (DSRM): DataIT2021!

## Sekundær DNS server
“Beskriv hvorledes man opsættes yderligere én server, der skal fungere som sekundær DNS-server. Serveren kaldes DevDNS. Den sekundære DNS-server skal indeholde en subdomainzone der kaldes dev.servertek.local, ligeledes beskrives hvorledes man opretter en DNS-delegering til dev.servertek.local på den primære domain-server.”

Den sekundære server sættes op med Windows Server 2022, og der installeres DNS server som på den primære server. Serveren navngives DevDNS.

På den primære server skal DNS zone transfer slås til: I DNS manager, højreklik på det primære domæne (servertek.local) og vælge Zone transfer fanen. Sæt flueben ved Allow zone transfer og vælg Only to the following server, hvorefter den statiske IP adresse til den sekundære server indtastes.

## Forklaring af DDNS-NetBIOS-WINS-LLMNR
### DDNS
Dynamic Domain Name System (DDNS) er en dynamisk DNS der selv opdaterer DNSen ved at lytte efter IP adresse ændringer, derefter går den selv ind og opdatere DNSen.

### NetBIOS
Network Basic Input/Output System (NetBIOS) er en API der hjælper computer med at kommunikere over LAN, den tilbyder nogle services til Bl.a. name service.

### WINS
Windows Internet Name Service (WINS)

### LLMNR
Link-Local Multicast Name Resolution (LLMNR), er en protokol der er baseret på DNS packet format, dette gør det muligt for både IPv4 og IPv6 at oversætte navne til IP adresser uden en DNS server.

## Konklusion
Vi havde en del problemer med opsætning af Microtic Hex routeren og endte med at bytte den til en Linksys e900. Vi havde en snak med SKP folkene, der fortalte at de fik rigtig mange klager over Microtik routerne.
