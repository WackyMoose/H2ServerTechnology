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
   - [Software Specifikationer](#Software-Specifikationer)  
5. [Installeret server roller](#Installeret-server-roller)
6. [Bruger Tabel](#Bruger-tabel)  
7. [Sikkerheds Grupper](#Sikkerheds-Grupper)  
8. [Konfigurationsvalg af fysisk server](#Konfigurationsvalg-af-fysisk-server)  
   - [Server konfiguration](#Server-konfiguration)  
   - [Active Directory konfiguration](#Active-Directory-konfiguration)  
   - [DHCP konfiguration](#DHCP-konfiguration)  
   - [DNS konfiguration](#DNS-konfiguration)  
   - [File Server Resource Manager Konfiguration](#File-Server-Resource-Manager-Konfiguration)
   - [WSUS konfiguration](#WSUS-konfiguration)  
9. [Beskriv: Sekundær DNS server](#Beskriv-Sekundær-DNS-server)  
10. [Beskriv: Remote Desktop adgang til serverne](#Beskriv-Remote-Desktop-adgang-til-serverne)  
11. [Beskriv: VPN opsætning til hjemmearbejdsplads](#Beskriv-VPN-opsætning-til-hjemmearbejdsplads)  
12. [Beskriv: Adgang til website med FTP over SSL](#Beskriv-Adgang-til-website-med-FTP-over-SSL)
13. [Forklaring af DDNS-NetBIOS-WINS-LLMNR](#Forklaring-af-DDNS-NetBIOS-WINS-LLMNR)  
    - [DDNS](#DDNS)  
    - [NetBIOS](#NetBIOS)  
    - [WINS](#WINS)  
    - [LLMNR](#LLMNR)  
14. [Forklar: Cloud-baseret serverdrift](#Forklar-Cloud-baseret-serverdrift)
    - [Infrastructure as a service (IaaS)](#Infrastructure-as-a-service-IaaS)
    - [Platform as a service (PaaS) / Serverless](#Platform-as-a-service-PaaS--Serverless)
    - [Software as a service (SaaS)  ](#Software-as-a-service-SaaS)
    - [Fordele](#Fordele)
    - [Ulemper](#Ulemper)
15. [Konklusion](#Konklusion)
    - [Server](#Server)  
    - [Netværk](#Netværk)  
    - [WSUS](#WSUS)
16. [Henvisninger](#Henvisninger)  
17. [Bilag](#Bilag)
    - [Opsætning af Router](#Opsætning-af-Router)
    - [Opretning af SSL certifikat](#Opretning-af-SSL-certifikat)
    - [Opsætning af FTP server med SSL](#Opsætning-af-FTP-server-med-SSL)

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
<details><summary>Netværks Diagram</summary>
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

### Software specifikationer  
Windows Server 2022 (21H2, build: xxxx.xxx)  
Windows 10 Pro (21H1, build: 19043.928)  
SQL Server Management  

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
### Server konfiguration
Vi har ændret navnet på serveren til MooseServer, fordi vi ville have et Moose tema.
Vi har givet serveren en statisk IP adresse, som er 192.168.1.2.

Partitioner (UserFolder, Backup, Public, Data)…
Ud fra vores 1TB HDD, vi havde i serveren, har vi delt den op i 4 partitioner;
Public (P:)
Data (D:)
Backup (B:)
UserFolders$ (U:)
Public og UserFolders$ er begge shared drives.
 
Public drevet er et drev det bliver sendt ud til klienterne via en gruppepolitik, og er et fælles drev for alle brugerne.

UserFolders$ drevet er det drev vi har sat Folder Redirection op til at køre på. Det er det drev, hvor alle klienternes mapper ligger i.

Data drevet er det drev, som indeholder WSUS mappen, alle afdelings mapperne, som er shared mapper, hvor det kun er afdelingen der har adgang til mappen, som er sat med NTFS rettigheder.

### Active Directory konfiguration
Vores domæne i vores Active Directory hedder servertek.local. 
Directory Services Restore Mode password (DSRM): DataIT2021!
Vi har konfigureret en række gruppepolitikker for folder redirection og netværksdrev til hver afdeling, 

### DHCP konfiguration
Vi har oprettet et scope med navnet servertek med start IP 192.168.1.1 og slut IP 192.168.1.254. Grunden til at vi ikke har scope fra 192.168.1.0 til 192.168.1.255, er fordi at .0 er vores netværks adresse, og .255 er broadcast adressen i vores scope. Vi har oprettet en eksklusion fra 192.168.1 til 192.168.10. Vi har valgt dette scope, så der er plads til nye servere (192.168.1.3/4/5 osv.), og så der er plads i scopet til at ansætte nye medarbejdere. Lease time er sat til 8 timer, da det er en hel arbejdsdag.

I en rigtig virksomhed ville man ikke vælge denne løsning. I stedet ville man lægge servere, administration og hver afdeling i sit eget V-LAN/subnet.
Scopet udsender information om IP adressen til default gateway og DNS server, samt DNS Domain name. 

### DNS konfiguration
Forwarders er sat til at være 8.8.8.8 (Googles DNS) og 1.1.1.1, 

### File Server Resource Manager konfiguration  
File Server Resource Manager er en server rolle, der gør sådan at man kan lave Disk Quota’er på delte mappe. En Disk Quota er en lille service, der bestemmer hvor meget plads der må ligge i den delte mappe, men deler. 
Vi har lavet 4 afdelinger, som hver har deres egne netværksdrev, som brugerne får via en GPO når de logger ind. 
Følgende afdelinger, som har et netværksdrev:  
Administration  
Developer  
Designer  
DevOps  

Vi har lavet en Disk Quota template, som hedder DepartmentsQuotas, hvor der er blevet sat en max limit på 20GB i mappen. Det ville sige at mappen ikke kan indeholde mere end 20GB

### WSUS konfiguration  

## Beskriv: Sekundær DNS server
“Beskriv hvorledes man opsættes yderligere én server, der skal fungere som sekundær DNS-server. Serveren kaldes DevDNS. Den sekundære DNS-server skal indeholde en subdomain zone der kaldes dev.servertek.local, ligeledes beskrives hvorledes man opretter en DNS-delegering til dev.servertek.local på den primære domain-server.”

Den sekundære server sættes op med Windows Server 2022, og der installeres DNS server som på den primære server. Serveren navngives DevDNS.

På den primære server, kan man i DNS manager højreklikke på DNS rod-ikonet og vælge Connect to DNS server… og indtaste navnet dev.servertek.local. Så er den sekundære DNS server tilføjet stifinder panelet.

På den primære server skal DNS zone transfer slås til: I DNS manager, højreklik på det primære domæne (servertek.local) og vælge Zone transfer fanen. Sæt flueben ved Allow zone transfer og vælg Only to the following server, hvorefter den statiske IP adresse til den sekundære server indtastes.

På den sekundære server, I DNS manager højreklikkes på mappen DevDns/Forward lookup zones. Vælg New zone og Secondary zone, hvorefter navnet på den nye zone indtastes (dev.servertek.local). Derefter angives master DNS serveren, hvis IP indtastes. Derefter højreklik DevDNS/Forward lookup og vælg Transfer from master, hvorefter der hentes en read-only kopi af alle forward adresser fra den primære DNS server.

## Beskriv: Remote Desktop adgang til serverne
Inde på Server Manager, under “Local Server”, skal man enable den der hedder “Remote Desktop”. 
Den knap åbner et nyt vindue, hvor der er en sektion der hedder “Remote Desktop”, du kan vælge at slå det til. Som standard står den til “Don’t allow remote connections to this computer”, hvilket betyder at man ikke kan forbinde sig til den med Remote Desktop.

Tryk på “Allow remote connections to this computer”, for at slå det til. Derefter skal du vælge hvilke brugere/grupper der skal have adgang til at kunne tilgå serveren via Remote Desktop. Derinde ville man normalt lave en gruppe som alle brugerne, der skal have adgang til Remote Desktop, ligger i. 

Derefter kan man teste det ved at gå på klientens computer, og åbne “Remote Desktop Connection“. I det vindue, skal man skrive IP adressen ind på den server man gerne ville tilgå. Her kan man gør det nemt for ens medarbejdere, ved at lave et domænenavn til IP adressen, sådan at man ikke behøver at skulle huske på IP adressen hver gang.

## Beskriv: VPN opsætning til hjemmearbejdsplads

## Beskriv: Adgang til website med FTP over SSL
Først skal der genereres et certifikat. I IIS manager, klik server certificate ikonet og vælg Create self-signed certificate… i fanen til højre. I fanen Specify Friendly Name, skriv navnet på certifikatet og vælg Web Hosting i drop down menuen1.

Installer serverrollen Web Server (IIS) / FTP Server. I IIS manager, højreklik på websitet og vælg Add FTP publishing… Under fanen Binding and SSL settings, klik radioknappen Require SSL og vælg SSL Certifikatet.

Under Authentication, vælg basic. Under Authorization, vælg Specified roles or user groups, og vælg Developer gruppen. Giv Developer read / write permission. Klik Finish.

På klienten kan medlemmer af Developer gruppen nu tilgå mappen med websitets filer ved at skrive ftp://192.168.1.2/ i file explorer.

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
I dag findes der en hastigt voksende mængde af cloudbaserede serviceudbydere, der tilbyder en bred vifte af produkter. F.eks. er det ikke længere almindeligt at have en fysisk mail-server stående hjemme eller på virksomheden, fordi den service i dag ligger i skyen som en cloud service. Eksempler på cloud-serviceudbydere kunne være Amazon Web Services, Microsoft Azure og Google Cloud, men der findes mange flere, og antallet af serviceudbydere vokser hastigt.

Cloud services falder i følgende hovedkategorier:
 
### Infrastructure as a service (IaaS)  
Tidligere var virksomheder nødt til at opsætte fysiske servere og klienter i et lokalt netværk af routere og switches, hvilket medførte udgifter til plads, strøm, nettrafik og løbende vedligeholdelse og skalering. 
I dag vælger mange at bruge cloudbaseret serverdrift eller IaaS i stedet, hvilket vil sige, at hele virksomhedens infrastruktur ligger tilgængeligt virtuelt i skyen. Brugeren skal ikke forholde sig til tekniske problemer, skalering eller vedligehold, det klarer cloudservicens medarbejdere.
### Platform as a service (PaaS) / Serverless
PaaS tilbyder kunderne en web-platform, hvor de hurtigt kan komme i gang med at udvikle software, websites eller apps uden at skulle bekymre sig om opsætning og konfigurering af udviklingsmiljøet. 
Kunden kan behandle store filer og benytte CPU- og RAM-krævende software på en lille laptop, da alt det tunge computing arbejde foregår i skyen. Service- udbyderen sørger for opsætning og skalering af hardware, så kunden kan fokusere på at udvikle.
### Software as a service (SaaS)  
Tidligere installerede man software lokalt på PC’en via et fysisk medie som f.eks. en diskette eller CD-ROM, som man havde købt i en butik. 
I dag er det mere almindeligt at benytte SaaS, dvs. at man abonnerer på adgang til softwaren og betaler pr. brug eller via et månedligt abonnement. 
Softwaren hentes over nettet og installeres og opdateres automatisk. 
Eksempler på software, der tidligere skulle installeres fysisk men som nu er SaaS kunne være Adobe Photoshop, der i dag er en del af Creative Cloud, eller platformen Steam, hvor man kan købe og installere spil over nettet, der tidligere blev solgt på DVD-ROM.
### Fordele  
Man er hurtigt i gang. Det kræver kun en subscription og nogle museklik at opsætte en virtuel cloud infrastruktur, som det ville tage lang tid at opsætte fysisk.

Fleksibel skalering. Det er nemt at skalere sin infrastruktur op (flere CPU kerner, mere RAM, større båndbredde osv.) eller ud (flere virtuelle maskiner).

Ingen hardware. Det kan være meget dyrt og tidskrævende at opsætte og køre et serverrum med køling, og det er dyrt at ansætte IT folk til at vedligeholde serveren. Alt det undgår man ved at vælge en IaaS service.

Nem adgang. Da servicen ligger i skyen kan den tilgås overalt i verden.

### Ulemper  
Når man først har valgt en cloud service, kan det være meget besværligt og tidskrævende at flytte til en anden serviceudbyder.

Tab af kontrol over data. Når man bruger en cloud service, vil data sandsynligvis blive transporteret på tværs af landegrænser og kontinenter, hvor der gælder forskellig lovgivning omkring datasikkerhed. Derfor kan det blive svært at sikre, at kundernes data ikke tilgås eller sælges til tredjepart.

## Konklusion
### Server
### Netværk
Vi havde en del problemer med opsætning af Microtic Hex routeren og endte med at bytte den til en Linksys e900. Vi havde en snak med SKP folkene, der fortalte at de fik rigtig mange klager over Microtik routerne.
### WSUS
Vi har arbejdet intens på at få WSUS til at virke, men efter rigtig mange forsøg og uden held, er det ikke lykkedes os at få det til at fungere. Vi fik 
## Henvisninger
Sekundær DNS server
https://www.youtube.com/watch?v=LmpuiiQ_GS4
https://www.youtube.com/watch?v=A66835rpTvY

Table of Server Roles
https://sid-500.com/2017/07/18/windows-server-list-all-installed-roles-and-features-using-powershell/ 

FTP over SSL

## Bilag  
### Opsætning af Router  
<details><summary>Router login interface, koden er admin.</summary>
   <p>
      
![Router login interface](https://github.com/WackyMoose/H2ServerTechnology/blob/main/LoginInterface.png)

</p>
</details>  
<details><summary>Her kan man se hvilken MAC adresse WAN interfacet har, samt IP adresse.</summary>
   <p>
      
![Router login interface](https://github.com/WackyMoose/H2ServerTechnology/blob/main/MACWan.png)

</p>
</details> 
<details><summary>Her kan man se hvilken MAC adresse LAN interfaces har, samt default gateway adressen. 
   Samt man kan se at DHCP serveren på routeren er disabled</summary>
   <p>
      
![Router login interface](https://github.com/WackyMoose/H2ServerTechnology/blob/main/MACLan.png)

</p>
</details> 
<details><summary>Her kan man se at NAT er enabled på vores router.</summary>
   <p>
      
![Router login interface](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Nat.png)

</p>
</details> 

### Opretning af SSL certifikat    
<details><summary>Her trykker man på “Server Certificates”.</summary>
   <p>
      
![Router login interface](https://github.com/WackyMoose/H2ServerTechnology/blob/main/ServerCertificates.png)

</p>
</details>  
<details><summary>Tryk på “Create Self-Signed Certificate…” ude i højre side.</summary>
   <p>
      
![Router login interface](https://github.com/WackyMoose/H2ServerTechnology/blob/main/SelfSignedCertificate.png)

</p>
</details> 
<details><summary>Her skal man give den et navn, samt vælge hvad type certifikat det skal være.</summary>
   <p>
      
![Router login interface](https://github.com/WackyMoose/H2ServerTechnology/blob/main/NameCertificate.png)

</p>
</details> 
<details><summary>Her kan man se at “servertek” certifikatet er blevet oprettet.</summary>
   <p>
      
![Router login interface](https://github.com/WackyMoose/H2ServerTechnology/blob/main/ServerTek.png)

</p>
</details> 

### Opsætning af FTP server med SSL  
<details><summary>FTP rollen installere gennem Server Manager -> Manage -> Add Roles and Features. Der ligger den under Server Roles og under Web Server (IIS).</summary>
   <p>
      
![Router login interface](https://github.com/WackyMoose/H2ServerTechnology/blob/main/ServerManager.png)

</p>
</details>  
<details><summary>På den side du gerne ville have FTP serveren, skal du højreklikke og tryk på “Add FTP Publishing…”.</summary>
   <p>
      
![Router login interface](https://github.com/WackyMoose/H2ServerTechnology/blob/main/FTPPublishing.png)

</p>
</details> 
<details><summary>Her skal man vælge hvilken IP adresse, FTP serveren skal køre på, man kan også vælge hvilken port det skal køre, men som standard kør det på port 21. Samt vælger det SSL certifikat der blevet lavet tidligere.</summary>
   <p>
      
![Router login interface](https://github.com/WackyMoose/H2ServerTechnology/blob/main/FTPIP.png)

</p>
</details> 
<details><summary>I Authentication vælger vi Basic Authentication
I Authorization vælger vi “Specified roles or User groups”, og i det input felt der er skriver vi den gruppe, som skal have adgang til FTP serveren, i vores tilfælde er det “Developer” gruppen. Vi giver dem read / write permissions.
</summary>
   <p>
      
![Router login interface](https://github.com/WackyMoose/H2ServerTechnology/blob/main/BasicAuthentication.png)

</p>
</details> 
</details> 
<details><summary>Her kan man se at der er adgang til FTP serveren, og man kan se websites filer.</summary>
   <p>
      
![Router login interface](https://github.com/WackyMoose/H2ServerTechnology/blob/main/FTPWorking.png)

</p>
</details> 
