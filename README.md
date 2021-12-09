# H2ServerTechnology

### Serverteknologi opgave af Victor, Michael, Pierre - 21hf43dat2pV - 10/12/2021

##### Table of Contents  
1. [Opgavebeskrivelse](#Opgavebeskrivelse)  
   - [Server Opsætning](#Server-Opsætning)  
   - [Beskriv / Forklar](#Beskriv-/-Forklar)  
2. [Netværksdiagram](#Netværksdiagram)  
3. [Interface og IP-adresse tabel](#Interface-og-IP-adresse-tabel)  
4. [Hardware og software specifikationer](#Hardware-og-software-specifikationer)  
   - [Hardware Specifikationer](#Hardware-Specifikationer)  
     - [Server](#Server)  
     - [Router](#Router)  
     - [Klient Computer](#Klient-Computer)   
 
5. [Installeret server roller](#Installeret-server-roller)
6. [Bruger Tabel](#Bruger-tabel)  
7. [Sikkerheds Grupper](#Sikkerheds-Grupper)  
8. [Konfigurationsvalg af fysisk server](#Konfigurationsvalg-af-fysisk-server)  
   - [Server konfigurationer](#Server-konfigurationer)  
   - [Active Directory konfigurationer](#Active-Directory-konfigurationer)  
   - [DHCP konfigurationer](#DHCP-konfigurationer)  
   - [WSUS konfiguration](#WSUS-konfiguration)  
   - [DNS konfiguration](#DNS-konfiguration)  
9. [Sekundær DNS server](#Sekundær-DNS-server)  
10. [Forklaring af DDNS-NetBIOS-WINS-LLMNR](#Forklaring-af-DDNS-NetBIOS-WINS-LLMNR)  
    - [DDNS](#DDNS)  
    - [NetBIOS](#NetBIOS)  
    - [WINS](#WINS)  
    - [LLMNR](#LLMNR)  
11. [Forklar: Cloud-baseret serverdrift](#Forklar-Cloud-baseret-serverdrift)
    - [Fordele](#Fordele)
    - [Ulemper](#Ulemper)
12. [Konklusion](#Konklusion)  
13. [Henvisninger](#Henvisninger)  
14. [Bilag](#Bilag)

## Opgavebeskrivelse
### Server opsætning
Der skal opsættes en server og en klient på et lokalt netværk. Følgende betingelser skal være opfyldt:  
- Microsoft Server 2019 eller nyere.
- Domæne (servertek.local), som styres at en enkelt Domain Controller (DC1).
- Active Directory (Domain Controller)
- DHCP serverrolle
- DNS serverrolle
- ISS serverrolle
- Folder redirect for hver bruger på et delt drev
- Mindst tre afdelinger (OU’s) med mindst to brugere pr. afdeling.
- Automatisk backup af alle brugerdata.
- Printserver med print-til-PDF. Kun tilgængelig i normal arbejdstid. Ansatte skal have højere prioritet end gæstebrugere.
- Alle opdateringer styres af WSUS og udføres på et passende tidspunkt.
- En dummy hjemmeside med firmalogo og -navn opsat via ISS

### Beskriv / Forklar
- Beskriv hvordan man opsætter en sekundær DNS server (DevDNS). Den skal indeholde en subdomain zone (dev.servertek.local). Beskriv hvordan man opretter en DNS delegering til -subdomain zonen på en primære DNS server.
- Beskriv hvordan man tilgår begge servere via Remote Desktop fra LAN.
- Beskriv begrebet IPSec og hvordan man laver en opkobling over WAN til LAN via en L2TP over IPSec VPN forbindelse opsat med remote access rollen.
- Beskriv hvordan udviklingsafdelingen kan få backend adgang til dummy hjemmesiden via FTP over SSL.
- Beskriv konfigurationsvalg af server.
- Forklar begreberne DDNS, NETBIOS, WINS, og LLMNR.
- Forklar begrebet cloudbaseret serverdrift, uddyb fordele og ulemper.

## Netværksdiagram
Skærmbillede fra Packet Tracer…  
<details><summary>Networks Diagram</summary>
   <p>
      
![NetværksDiagram](https://github.com/WackyMoose/H2ServerTechnology/blob/main/NetworksDiagram.png)

</p>
</details>
      
## Interface og IP-adresse tabel
<details><summary>Adresse tabel</summary>
<p>
  
| Enhed           | Port                  | IPv4          | MAC               |
| --------------- | --------------------- | -----------   | ----------------- |
| Server          | NIC2                  | 192.168.1.2   | 50-9A-4C-91-D5-1E |
| Klient          | 1 / default           | dynamisk      | 00-23-7D-00-2C-CC |
| Router          | WAN (Default gateway) | 192.168.1.1   | 58-EF-68-55-3E-C5 |
| Router          | LAN (Server)          | 192.168.1.1   | 58-EF-68-55-3E-C4 |
| Router          | LAN (Client)          | 192.168.1.1   | 58-EF-68-55-3E-C4 |

</p>
</details>

## Hardware og software specifikationer
### Hardware Specifikationer
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

## Installeret server roller

Kommando brugt til at fremkalde tabellen:
```Powershell
Get-WindowsFeature | Where-Object { $_.installState -eq “Installed” } | Format-Table Name,InstallState | More | Out-File -FilePath P:\ServerRoles.txt
```
<details><summary>Server Roller</summary>
   <p>
      
| Name                           | Install State                   |
| ------------------------------ | ------------------------------- |
| AD-Domain-Services             | Installed                       |
| DHCP                           | Installed                       |
| DNS                            | Installed                       |
| FileAndStorage-Services        | Installed                       |
| File-Services                  | Installed                       |
| FS-FileServer                  | Installed                       |
| FS-Resource-Manager            | Installed                       |
| Storage-Services               | Installed                       |
| Print-Services                 | Installed                       |
| Print-Server                   | Installed                       |
| Print-Internet                 | Installed                       |
| Web-Server                     | Installed                       |
| Web-WebServer                  | Installed                       |
| Web-Common-Http                | Installed                       |
| Web-Default-Doc                | Installed                       |
| Web-Dir-Browsing               | Installed                       |
| Web-Http-Errors                | Installed                       |
| Web-Static-Content             | Installed                       |
| Web-Http-Redirect              | Installed                       |
| Web-Health                     | Installed                       |
| Web-Http-Logging               | Installed                       |
| Web-Log-Libraries              | Installed                       |
| Web-Request-Monitor            | Installed                       |
| Web-Http-Tracing               | Installed                       |
| Web-Performance                | Installed                       |
| Web-Stat-Compression           | Installed                       |
| Web-Dyn-Compression            | Installed                       |
| Web-Security                   | Installed                       |
| Web-Filtering                  | Installed                       | 
| Web-Basic-Auth                 | Installed                       |
| Web-Windows-Auth               | Installed                       |
| Web-App-Dev                    | Installed                       |
| Web-Net-Ext45                  | Installed                       |
| Web-ASP                        | Installed                       |
| Web-ASP-Net45                  | Installed                       |
| Web-ISAPI-Ext                  | Installed                       |
| Web-ISAPI-Filter               | Installed                       |
| Web-Mgmt-Tools                 | Installed                       |
| Web-Mgmt-Console               | Installed                       |
| Web-Mgmt-Compat                | Installed                       |
| Web-Metabase                   | Installed                       |
| UpdateServices                 | Installed                       |
| UpdateServices-WidDB           | Installed                       |
| UpdateServices-Services        | Installed                       |
| NET-Framework-45-Features      | Installed                       |
| NET-Framework-45-Core          | Installed                       |
| NET-Framework-45-ASPNET        | Installed                       |
| NET-WCF-Services45             | Installed                       |
| NET-WCF-HTTP-Activation45      | Installed                       |
| NET-WCF-TCP-PortSharing45      | Installed                       |
| GPMC                           | Installed                       |
| Windows-Defender               | Installed                       |
| RSAT                           | Installed                       |
| RSAT-Role-Tools                | Installed                       |
| RSAT-AD-Tools                  | Installed                       |
| RSAT-AD-PowerShell             | Installed                       |
| RSAT-ADDS                      | Installed                       |
| RSAT-AD-AdminCenter            | Installed                       |
| RSAT-ADDS-Tools                | Installed                       |
| UpdateServices-RSAT            | Installed                       |
| UpdateServices-API             | Installed                       |
| UpdateServices-UI              | Installed                       |
| RSAT-DHCP                      | Installed                       |
| RSAT-DNS-Server                | Installed                       |
| RSAT-File-Services             | Installed                       |
| RSAT-FSRM-Mgmt                 | Installed                       |
| RSAT-Print-Services            | Installed                       |
| System-DataArchiver            | Installed                       |
| Windows-Internal-Database      | Installed                       |
| PowerShellRoot                 | Installed                       |
| PowerShell                     | Installed                       |
| WAS                            | Installed                       |
| WAS-Process-Model              | Installed                       |
| WAS-Config-APIs                | Installed                       |
| WoW64-Support                  | Installed                       |
| XPS-Viewer                     | Installed                       |
      
   </p>
</details>

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
### Server konfigurationer
Vi har ændret navnet på serveren til MooseServer, fordi vi ville have et Moose tema.
Vi har givet serveren en statisk IP adresse, som er 192.168.1.2.

### Active Directory konfigurationer
Vores domæne i vores Active Directory hedder servertek.local. 
Directory Services Restore Mode password (DSRM): DataIT2021!
Vi har konfigureret en række gruppepolitikker for folder redirection og netværksdrev til hver afdeling, 

### DHCP konfigurationer
Vi har oprettet et scope med navnet servertek med start IP 192.168.1.1 og slut IP 192.168.1.254. Grunden til at vi ikke har scope fra 192.168.1.0 til 192.168.1.255, er fordi at .0 er vores netværks adresse, og .255 er broadcast adressen i vores scope. Vi har oprettet en eksklusion fra 192.168.1 til 192.168.10. Vi har valgt dette scope, så der er plads til nye servere (192.168.1.3/4/5 osv.), og så der er plads i scopet til at ansætte nye medarbejdere. Lease time er sat til 8 timer, da det er en hel arbejdsdag.

I en rigtig virksomhed ville man ikke vælge denne løsning. I stedet ville man lægge servere, administration og hver afdeling i sit eget V-LAN/subnet.
Scopet udsender information om IP adressen til default gateway og DNS server, samt DNS Domain name. 

### WSUS konfiguration

### DNS konfiguration
Forwarders er sat til at være 8.8.8.8 (Googles DNS) og 1.1.1.1, 

## Sekundær DNS server
“Beskriv hvorledes man opsættes yderligere én server, der skal fungere som sekundær DNS-server. Serveren kaldes DevDNS. Den sekundære DNS-server skal indeholde en subdomainzone der kaldes dev.servertek.local, ligeledes beskrives hvorledes man opretter en DNS-delegering til dev.servertek.local på den primære domain-server.”

Den sekundære server sættes op med Windows Server 2022, og der installeres DNS server som på den primære server. Serveren navngives DevDNS.

På den primære server, kan man i DNS manager højreklikke på DNS rod-ikonet og vælge Connect to DNS server… og indtaste navnet dev.servertek.local. Så er den sekundære DNS server tilføjet stifinder panelet.

På den primære server skal DNS zone transfer slås til: I DNS manager, højreklik på det primære domæne (servertek.local) og vælge Zone transfer fanen. Sæt flueben ved Allow zone transfer og vælg Only to the following server, hvorefter den statiske IP adresse til den sekundære server indtastes.

På den sekundære server, I DNS manager højreklikkes på mappen DevDns/Forward lookup zones. Vælg New zone og Secondary zone, hvorefter navnet på den nye zone indtastes (dev.servertek.local). Derefter angives master DNS serveren, hvis IP indtastes. Derefter højreklik DevDns/Forward lookup og vælg Transfer from master, hvorefter der hentes en read-only kopi af alle forward adresser fra den primære DNS server.

## Forklaring af DDNS-NetBIOS-WINS-LLMNR
### DDNS
Dynamic Domain Name System (DDNS) er en dynamisk DNS der selv opdaterer DNSen ved at lytte efter IP adresse ændringer, derefter går den selv ind og opdatere DNSen.

### NetBIOS
Network Basic Input/Output System (NetBIOS) er en API der hjælper computer med at kommunikere over LAN, den tilbyder nogle services til Bl.a. name service.

### WINS
Windows Internet Name Service (WINS) er en service der hjalp windows med at oversætte NetBIOS domæne navne til IP-adresser, da NetBIOS stadig kørte på NetBEUI protokollen, i modsætning til DNS der køre på TCP/IP protokolen, den er senere hen blevet udskiftet med DNS efter Microsoft har ændret i den så NetBIOS kan bruge TCP/IP protokollen.

### LLMNR
Link-Local Multicast Name Resolution (LLMNR), er en protokol der er baseret på DNS packet format, dette gør det muligt for både IPv4 og IPv6 at oversætte navne til IP adresser uden en DNS server.

## Forklar: Cloud-baseret serverdrift
I dag findes der en hastigt voksende mængde af cloudbaserede services, der kan bruges som alternativ til en fysisk, on-premises server. Der findes i dag mange cloud services, der tilbyder en bred vifte af produkter. F.eks. er det ikke længere almindeligt at have en fysisk mail-server stående hjemme eller på virksomheden, fordi den service i dag ligger i skyen.  Kort forklaret, så flytter man alle serverens funktioner ud på en virtuel server, der ligger i skyen. Alt det tekniske bliver ordnet og vedligeholdt af cloud servicens ansatte teknikere.
Eksempler på populære cloud-serviceudbydere kunne være Amazon Web Services, Microsoft Azure og Google Cloud, men der findes mange flere, og antallet af service-udbydere vokser hastigt.

### Fordele  

### Ulemper  

## Konklusion
Vi havde en del problemer med opsætning af Microtic Hex routeren og endte med at bytte den til en Linksys e900. Vi havde en snak med SKP folkene, der fortalte at de fik rigtig mange klager over Microtik routerne.

## Henvisninger
Sekundær DNS server  
https://www.youtube.com/watch?v=LmpuiiQ_GS4  
https://www.youtube.com/watch?v=A66835rpTvY  

## Bilag  

