# H2ServerTechnology

### Serverteknologi opgave af Victor, Michael, Pierre - 21hf43dat2pV - 10/12/2021

##### Table of Contents  
1. [Opgavebeskrivelse](#Opgavebeskrivelse)  
   - [Infrastruktur med router, server og klient](#infrastruktur-med-router-server-og-klient)  
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
8. [Backup løsning](#Backup-løsning)
9. [Konfigurationsvalg af fysisk server](#Konfigurationsvalg-af-fysisk-server)  
   - [Server konfiguration](#Server-konfiguration)  
   - [Active Directory konfiguration](#Active-Directory-konfiguration)  
   - [DHCP konfiguration](#DHCP-konfiguration)  
   - [DNS konfiguration](#DNS-konfiguration)  
   - [File Server Resource Manager Konfiguration](#File-Server-Resource-Manager-Konfiguration)
   - [WSUS konfiguration](#WSUS-konfiguration)  
10. [Beskriv: Sekundær DNS server](#Beskriv-Sekundær-DNS-server)  
11. [Beskriv: Remote Desktop adgang til serverne](#Beskriv-Remote-Desktop-adgang-til-serverne)  
12. [Beskriv: L2TP over IPSec VPN forbindelse](#Beskriv-L2TP-over-IPSec-VPN-forbindelse)
13. [Beskriv: VPN opsætning til hjemmearbejdsplads](#Beskriv-VPN-opsætning-til-hjemmearbejdsplads)  
14. [Beskriv: Adgang til website med FTP over SSL](#Beskriv-Adgang-til-website-med-FTP-over-SSL)
15. [Forklaring af DDNS-NetBIOS-WINS-LLMNR](#Forklaring-af-DDNS-NetBIOS-WINS-LLMNR)  
    - [DDNS](#DDNS)  
    - [NetBIOS](#NetBIOS)  
    - [WINS](#WINS)  
    - [LLMNR](#LLMNR)  
16. [Forklar: Cloud-baseret serverdrift](#Forklar-Cloud-baseret-serverdrift)
    - [Infrastructure as a service (IaaS)](#Infrastructure-as-a-service-IaaS)
    - [Platform as a service (PaaS) / Serverless](#Platform-as-a-service-PaaS--Serverless)
    - [Software as a service (SaaS)  ](#Software-as-a-service-SaaS)
    - [Fordele](#Fordele)
    - [Ulemper](#Ulemper)
17. [Konklusion](#Konklusion)
    - [Netværk](#Netværk)  
    - [WSUS](#WSUS)
    - [Print Server](#Print-Server)
18. [Henvisninger](#Henvisninger)  
19. [Bilag](#Bilag)
    - [Opsætning af Router](#Opsætning-af-Router)
    - [Active Directory opsætning](#Active-Directory-opsætning)
    - [GPO Struktur](#GPO-Struktur)
    - [DHCP konfiguration](#DHCP-konfiguration-bilag)
    - [DNS konfiguration](#DNS-konfiguration-bilag)
    - [IIS Dummy website](#IIS-Dummy-website)
    - [Folder Redirection](#Folder-Redirection)
    - [Afdelings mapper (NTFS og Share rettigheder)](#Afdelings-mapper-NTFS-og-Share-rettigheder)
    - [Backup løsning](#Backup-løsning-bilag)
    - [Opsætning af Disk Quota](#Opsætning-af-Disk-Quota)
    - [WSUS Konfiguration](#WSUS-Konfiguration-bilag)
    - [WSUS Fejl](#WSUS-Fejl)
    - [Print server](#Print-server)
    - [Opretning af SSL certifikat](#Opretning-af-SSL-certifikat)
    - [Opsætning af FTP server med SSL](#Opsætning-af-FTP-server-med-SSL)

## Opgavebeskrivelse
### Infrastruktur med router, server og klient
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
      
![NetværksDiagram](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/NetworksDiagram.png)

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

Til print serveren har vi brugt:  
Bullzip  
Win2PDF  
BioPDF  

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

## Backup løsning
Der er opsat Shadow Copy, der løbende genererer sikkerhedskopier af alle brugerens filer på et serverdrev, på samme måde som f.eks. OneDrive eller Dropbox. Sikkerhedskopierne gemmes på serverdrevet UserFolders$, og NTFS-rettighederne bliver også gemt, så admin ikke kan tilgå brugernes data på serverdrevet. [bilag](#Backup-løsning-Bilag)

## Konfigurationsvalg af fysisk server
### Server konfiguration
Vi har ændret navnet på serveren til MooseServer, fordi vi ville have et Moose tema.
Vi har givet serveren en statisk IP adresse, som er 192.168.1.2.

Partitioner (UserFolder, Backup, Public, Data)…  
Ud fra vores 1TB HDD, vi havde i serveren, har vi delt den op i 4 partitioner;  
- Public (P:)  
- Data (D:)  
- Backup (B:)  
- UserFolders$ (U:)  
Public og UserFolders$ er begge shared drives.

Public drevet bliver delt med klienterne via en gruppepolitik og fungerer som fællesdrev for alle brugere.

Folder Redirection er sat op på UserFolders$ drevet, hvor alle brugernes private mapper ligger. NTFS-rettigheder kopieres også over, så admin ikke kan se indholdet. [Bilag](#Active-Directory-opsætning)

Backup drevet er en kopi af UserFolders$ drevet, hvor ALT bliver kopieret med over, sådan at det er en tro kopi af UserFolders$ og alle brugerne private mapper, som bliver syncet med Folder Redirection. [Bilag](#Backup-løsning-Bilag)

Data drevet indeholder WSUS mappen, alle afdelings mapperne, som er shared mapper, hvor det kun er afdelingen der har adgang til mappen, som er sat med NTFS rettigheder. [Bilag](#Afdelings-mapper-NTFS-og-Share-rettigheder)

### Active Directory konfiguration
Active Directory forest hedder servertek.local. Directory Services Restore Mode password (DSRM) er DataIT2021!
Via Active Directory Administrative Center er der oprettet en OU i vores servertek.local forest.  
For overblik og detaljer over OU og User struktur, se [Tabel over Brugere](#Bruger-tabel). [Bilag](#Active-Directory-opsætning)

Via Group Policy Management er der oprettet nogle gruppepolitikker til Folder Redirection samt fælles netværksdrev og netværksdrev til afdelingerne. Se struktureren i gruppepolitikkerne i bilagende. [Bilag](#GPO-Struktur)

### DHCP konfiguration
Vi har oprettet et scope med navnet servertek med start IP 192.168.1.1 og slut IP 192.168.1.254. Grunden til at vi ikke har scope fra 192.168.1.0 til 192.168.1.255, er fordi at .0 er vores netværks adresse, og .255 er broadcast adressen i vores scope. Vi har oprettet en eksklusion fra 192.168.1 til 192.168.10. Vi har valgt dette scope, så der er plads til nye servere (192.168.1.3/4/5 osv.), og så der er plads i scopet til at ansætte nye medarbejdere. Lease time er sat til 8 timer, da det er en hel arbejdsdag.

I en rigtig virksomhed ville man ikke vælge denne løsning. I stedet ville man lægge servere, administration og hver afdeling i sit eget V-LAN/subnet.
Scopet udsender information om IP adressen til default gateway og DNS server, samt DNS Domain name. [Bilag](#DHCP-konfiguration-bilag)

### DNS konfiguration
Vores DNS server er konfigureret til at have en primary forward lookup zone, som vi har kaldt ServertekDNS, som bruges til vores Active Directory. Den har 2 forwarders, som er 8.8.8.8 som er Google DNS server, og så har vi 1.1.1.1 som er en anden offentlig DNS server. [Bilag](#DNS-konfiguration-bilag)

### File Server Resource Manager konfiguration  
File Server Resource Manager er en server rolle, der gør sådan at man kan lave Disk Quota’er på delte mappe. En Disk Quota er en lille service, der bestemmer hvor meget plads der må ligge i den delte mappe, men deler. 
Vi har lavet 4 afdelinger, som hver har deres egne netværksdrev, som brugerne får via en GPO når de logger ind. 
Følgende afdelinger, som har et netværksdrev:
- Administration
- Developer
- Designer
- DevOps

Vi har lavet en Disk Quota template, som hedder DepartmentsQuotas, hvor der er blevet sat en max limit på 20GB i mappen. Det ville sige at mappen ikke kan indeholde mere end 20GB. [Bilag](#Opsætning-af-Disk-Quota)

### WSUS konfiguration  
Når man opsætter WSUS, bliver man spurgt om hvor man vil gemme WSUS data henne, og der har vi valgt D:/WSUS mappen, det er en mappe der ligger på Data drevet.

Vi har valgt kun at få danske og engelske versioner af Windows 10 opdateringer, da vi ikke har brug for at have alle de andre sprog liggende. Første gang vi installerede WSUS, valgte vi at hente alle Windows opdateringerne, dvs. alle Windows Server versioner og Windows versioner, samt vi har valgt at hente alle update, tools, upgrades, service packs, osv (se bilag). Det viste sig at være alt for omfattende, så vi gen-installerede WSUS med kun de mest nødvendige, kritiske opdateringer til Windows 10 og Server 2021.

Vi har valgt at den skal synkronisere én gang hver dag kl. 16.00. [Bilag](#WSUS-Konfiguration-bilag)


## Beskriv: Sekundær DNS server
Den sekundære server sættes op med Windows Server 2022, og der installeres DNS server som på den primære server. Serveren navngives DevDNS.

På den primære server, kan man i DNS manager højreklikke på DNS rod-ikonet og vælge Connect to DNS server… og indtaste navnet dev.servertek.local. Så er den sekundære DNS server tilføjet stifinder panelet.

På den primære server skal DNS zone transfer slås til: I DNS manager, højreklik på det primære domæne (servertek.local) og vælge Zone transfer fanen. Sæt flueben ved Allow zone transfer og vælg Only to the following server, hvorefter den statiske IP adresse til den sekundære server indtastes.

På den sekundære server, I DNS manager højreklikkes på mappen DevDns/Forward lookup zones. Vælg New zone og Secondary zone, hvorefter navnet på den nye zone indtastes (dev.servertek.local). Derefter angives master DNS serveren, hvis IP indtastes. Derefter højreklik DevDNS/Forward lookup og vælg Transfer from master, hvorefter der hentes en read-only kopi af alle forward adresser fra den primære DNS server.

## Beskriv: Remote Desktop adgang til serverne
Inde på Server Manager, under “Local Server”, skal man enable den der hedder “Remote Desktop”. Den knap åbner et nyt vindue, hvor der er en sektion der hedder “Remote Desktop”, du kan vælge at slå det til. Som standard står den til “Don’t allow remote connections to this computer”, hvilket betyder at man ikke kan forbinde sig til den med Remote Desktop.

Tryk på “Allow remote connections to this computer”, for at slå det til. Derefter skal du vælge hvilke brugere/grupper der skal have adgang til at kunne tilgå serveren via Remote Desktop. Derinde ville man normalt lave en gruppe som alle brugerne, der skal have adgang til Remote Desktop, ligger i. 

Derefter kan man teste det ved at gå på klientens computer, og åbne “Remote Desktop Connection“. I det vindue, skal man skrive IP adressen ind på den server man gerne ville tilgå. Her kan man gør det nemt for ens medarbejdere, ved at lave et domænenavn til IP adressen, sådan at man ikke behøver at skulle huske på IP adressen hver gang.

## Beskriv: L2TP over IPSec VPN forbindelse
Som medarbejder har man behov for at kunne oprette en sikker forbindelse til sin virksomheds LAN over det usikre internet, f.eks. når man arbejder hjemmefra. Dette gøres med en Virtual Private Network (VPN) forbindelse, som beskrevet her. Da dette ikke er en installationsvejledning, beskrives her kun de vigtigste hovedpunkter.

(1) Rollen Remote Access installeres på serveren. Dette gøres i Server Manager med Role and feature installation wizard. Følg guiden. Under Select role Services, sæt flueben ved DirectAccess and VPN, kør installationen.

(2) konfigurer L2TP over IPSec. På serveren, vælg Tools og Routing and Remote Access Console. Højreklik på serveren i stifinderen til venstre og vælg Configure and Enable Routing and Remote Access. Følg guiden. Vælg Custom configuration, VPN access og start servicen.

(3) Konfigurer pre-shared key for IPSec autentificering. L2TP over IPSec VPN autentificerer de to hosts/netværk med en såkaldt pre-shared key. På serveren, vælg Tools og Routing and Remote Access Console. Højreklik på serveren i stifinderen til venstre og vælg Properties. Under fanen Security, vælg Allow custom IPSec Policy for L2TP/IKEv2 Connection og angiv en stærk pre-shared key.
Under IPv4, tilføj en static address pool, angiv IPv4 range og genstart Routing and Remote Access servicen.

(4) Tillad Dial-in Acces for udvalgte brugere. I Server Manager, under den udvalgte brugers properties, gå ind under Dial-in fanen,vælg Allow access og apply.

(5) Opsætning af VPN forbindelse. På klient-PC’en, gå ind under Network Connections. UNder fanen VPN, tilføj en ny VPN forbindelse. Angiv de nødvendige informationer, herunder en IP adresse indenfor det angivne range og pre-shared key.

### IPSec
Internet Protocol Security (IPSec) er en protokol-suite, der gør det muligt at autentificere hosts og kryptere pakker sendt over et usikkert netværk, og som dermed gør det muligt for to hosts at kommunikere lige så sikkert over internettet, som over et beskyttet LAN. 
IPSec gør det muligt for to hosts at autentificere sig overfor hinanden, og at kryptere og beskytte forbindelsen mellem to hosts, to netværk, eller mellem en host og et netværk. IPSec bliver benyttet til VPN.

## Beskriv: Adgang til website med FTP over SSL
Først skal der genereres et certifikat. I IIS manager, klik server certificate ikonet og vælg Create self-signed certificate… i fanen til højre. I fanen Specify Friendly Name, skriv navnet på certifikatet og vælg Web Hosting i drop down menuen.

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
I dag findes der en hastigt voksende mængde af cloudbaserede serviceudbydere, der tilbyder en bred vifte af produkter. De gør det muligt at lægge data, beregningsopgaver, software, samt servere, netværk og anden infrastruktur op i skyen, i stedet for at have det stående fysisk på virksomhedens adresse. Eksempler på cloud-serviceudbydere kunne være Amazon Web Services, Microsoft Azure og Google Cloud, men der findes mange flere, og antallet af serviceudbydere vokser hastigt.

Cloud services falder i følgende hovedkategorier:
 
### Infrastructure as a service (IaaS)  
Tidligere var virksomheder nødt til at opsætte fysiske servere og klienter i et lokalt netværk af routere og switches, hvilket medførte udgifter til plads, strøm, nettrafik og løbende vedligeholdelse og skalering. 
I dag vælger mange at bruge cloudbaseret serverdrift eller IaaS i stedet, hvilket vil sige, at hele virksomhedens infrastruktur ligger tilgængeligt virtuelt i skyen. 
Brugeren skal ikke forholde sig til tekniske problemer, skalering eller vedligehold, det klarer cloudservicens medarbejdere.
### Platform as a service (PaaS) / Serverless
PaaS tilbyder kunderne en web-platform, hvor de hurtigt kan komme i gang med at udvikle software, websites eller apps uden at skulle bekymre sig om opsætning og konfigurering af udviklingsmiljøet. 
Kunden kan behandle store filer og benytte CPU- og RAM-krævende software på en lille laptop, da alt det tunge beregningsarbejde foregår i skyen. 
Service- udbyderen sørger for opsætning og skalering af hardware, så kunden kan fokusere på at udvikle.
### Software as a service (SaaS)  
Tidligere installerede man software lokalt på PC’en via et fysisk medie som f.eks. en diskette eller CD-ROM, som man havde købt i en butik. I dag er det mere almindeligt at benytte SaaS, dvs. at man abonnerer på adgang til softwaren og betaler pr. brug eller via et månedligt abonnement. 
Softwaren hentes over nettet og installeres automatisk eller kører direkte i browseren via javascript eller det meget hurtige webassembly. 
Eksempler på software, der tidligere skulle installeres fysisk men som nu er SaaS kunne være Adobe Photoshop, der i dag er en del af Creative Cloud, eller platformen Steam, hvor man kan købe og installere spil over nettet, der tidligere blev solgt på DVD-ROM.

### Fordele  
Man er hurtigt i gang. Det kræver kun en subscription og nogle museklik at opsætte en virtuel cloud infrastruktur, som det ville tage lang tid at opsætte fysisk.

Fleksibel skalering. Det er nemt at skalere sin infrastruktur op (flere CPU kerner, mere RAM, større båndbredde osv.) eller ud (flere virtuelle maskiner).
Ingen hardware. Det kan være meget dyrt og tidskrævende at opsætte og køre et serverrum med køling, og det er dyrt at ansætte IT folk til at vedligeholde serveren. Alt det undgår man ved at vælge en IaaS service.

Nem adgang. Da servicen ligger i skyen kan den tilgås overalt i verden.

### Ulemper  
Når man først har valgt en cloud service, kan det være meget besværligt og tidskrævende at flytte til en anden serviceudbyder.

Tab af kontrol over data. Når man bruger en cloud service, vil data sandsynligvis blive transporteret på tværs af landegrænser og kontinenter, hvor der gælder forskellig lovgivning omkring datasikkerhed. Derfor kan det blive svært at sikre, at kundernes data ikke tilgås af andre eller sælges til tredjepart.

## Konklusion
Vi har sat en server op med Windows Server 2022 Evalution og følgende serverroller: 
- Active Directory Domain Service
- DNS server
- DHCP server
- IIS server. 
- WSUS
- Print server

Alle OU’er og brugere er oprettet som beskrevet i opgaven. Folder Redirection er sat op, sådan at brugerne mapper ligge oppe på serveren, og til enhver tid skifte PC, uden at miste noget. Vi har også sat et Public netværksdrev op, som alle brugere i domænet har adgang til, samt vi har lavet afdelings netværksdrev hvor vi har sat passende NTFS og Share-rettigheder op. Shadow Copy blev sat op på UserFolders$ drevet som en Back-up løsning.

I det store hele fik vi løst alle opgaver, på nær følgende ting:
### Netværk
Vi havde en del problemer med opsætning af Microtic Hex routeren og endte med at bytte den til en Linksys e900. Vi havde en snak med SKP folkene, der fortalte at de fik rigtig mange klager over Microtik routerne. Linksys routeren virkede fint hele ugen.
### WSUS
Vi har arbejdet intens på at få WSUS til at virke, men trods flere forsøg fik vi det ikke til at lykkedes.

Til at starte med gik alt fint, indtil at vores WSUS Console ikke ville begynde at synkronisere opdateringer uden at crashe. Der gik lidt tid på at finde ud af hvorfor, men efter noget tid tog vi beslutningen om at vi måtte geninstallere WSUS servicen. Efter vi havde geninstalleret WSUS services, fik vi en fejl der sagde at vores database havde en forkert version. Der gik lidt tid på, at finde ud hvorfor den gav den fejl besked, og vi fandt ud af at WSUS benytter en lokal database. Vi måtte installere Microsoft SQL Server Management Studio, og logge ind på den lokale database, WSUS havde oprettet. Det gjorde vi ved at forbinde til: \\.\pipe\microsoft##WID\tsql\query. Inde på den lokale database skulle vi slette en database der hed “SUSDB”, som kan ses på <details><summary>billedet</summary>
   <p>
      
![WSUSKonklusion](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/WSUSKonklusion.png)
  
   </p>
</details>

Efter vi havde slettet databasen, gik vi tilbage til WSUS services, og forsøgte igen at sætte WSUS op denne, gang blev vi mødt af fejlen at vi ikke kunne tilgå “Microsoft Update Service” som ligger ude på nettet et eller andet sted. Her begyndte et træls loop, der gjorde at vi aldrig kom videre med WSUS, da den så igen smed en fejl fordi vi brugte en forkert database version.

## Print Server
Vi har arbejdet intenst på at få lavet en delt netværks-PDF-print server. 
Dog kom vi ind i RIGTIG mange problemer, vi har forsøgt med at finde en løsning i Windows Server, samt med en driver hvor vi printede til en fil. 
Vi har også forsøgt at bruge tredjeparts drivere / programmer for at få det til at virke, dog uden held. 
Vi brugte også en del tid på at undersøge hvilke porte, der skulle åbnes og oprette de nødvendige firewall rules.
Vi har brugt følgende tredjeparts drivere / programmer: Bullzip, Win2PDF, BioPDF. [Bilag](#Print-server)

## Henvisninger
Sekundær DNS server
https://www.youtube.com/watch?v=LmpuiiQ_GS4
https://www.youtube.com/watch?v=A66835rpTvY

Table of Server Roles
https://sid-500.com/2017/07/18/windows-server-list-all-installed-roles-and-features-using-powershell/ 

FTP over SSL
https://winscp.net/eng/docs/guide_windows_ftps_server

L2TP over IPSec
https://msftwebcast.com/2020/02/how-to-setup-l2tp-ipsec-vpn-on-windows-server-2019.html

DDNS
https://www.cloudns.net/blog/what-is-dynamic-dns/
https://stevessmarthomeguide.com/dynamic-dns/

NetBIOS
https://www.techtarget.com/searchnetworking/definition/NetBIOS
https://www.lifewire.com/netbios-software-protocol-818229

WINS
https://www.techtarget.com/searchnetworking/answer/What-is-difference-between-a-WINS-server-and-a-DNS-server
https://networkencyclopedia.com/wins-server/

LLMNR
https://www.crowe.com/cybersecurity-watch/netbios-llmnr-giving-away-credentials
https://en.wikipedia.org/wiki/Link-Local_Multicast_Name_Resolution

Print Server
https://www.biopdf.com/
https://www.bullzip.com/
https://www.win2pdf.com/

## Bilag  
### Opsætning af Router  
<details><summary>Router login interface, koden er admin.</summary>
   <p>
      
![Router login interface](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/LoginInterface.png)

</p>
</details>  
<details><summary>Her kan man se hvilken MAC adresse WAN interfacet har, samt IP adresse.</summary>
   <p>
      
![MACWan](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/MACWan.png)

</p>
</details> 
<details><summary>Her kan man se hvilken MAC adresse LAN interfaces har, samt default gateway adressen. 
   Samt man kan se at DHCP serveren på routeren er disabled</summary>
   <p>
      
![MACLan](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/MACLan.png)

</p>
</details> 
<details><summary>Her kan man se at NAT er enabled på vores router.</summary>
   <p>
      
![NAT](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/Nat.png)

</p>
</details> 

### Active Directory opsætning
<details><summary>Her er et billede af hvordan vores OU struktur i vores Active Directory er sat op. Der kan ses i toppen af billedet, at afdelings OU’erne ligger inde i vores; servertek (local) forest -> servertek -> Users</summary>
   <p>
      
![ActiveDirectory](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/ActiveDirectory.png)

</p>
</details> 

### GPO Struktur
<details><summary>På billedet kan man se hvordan vi har struktureret vores GPO’er i vores OU’er.</summary>
   <p>
      
![GPOStructure](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/GPOStructure.png)

</p>
</details> 

### DHCP konfiguration Bilag
<details><summary>Installation af DHCP serverrolle gennem Server Manager.</summary>
   <p>
      
![DHCPInstallation01](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/DHCPInstallation01.png)
![DHCPInstallation02](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/DHCPInstallation02.png)

</p>
</details> 
<details><summary>DHCP post-install configuration</summary>
   <p>
      
![DHCPPostInstallation01](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/DHCPPostInstallation01.png)
![DHCPPostInstallation02](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/DHCPPostInstallation02.png)

</p>
</details> 
<details><summary>DHCP scope Wizard</summary>
   <p>
      
![DHCPScopeWizard01](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/DHCPScopeWizard01.png)
![DHCPScopeWizard02](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/DHCPScopeWizard02.png)

</p>
</details> 

### DNS konfiguration Bilag
</details> 
<details><summary>På billedet kan man se at hvordan vores DNS server er konfigureret.</summary>
   <p>
      
![DNSConfig](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/DNSConfig.png)

</p>
</details> 

### IIS Dummy website
</details> 
<details><summary>Her er et billede af vores dummy hjemmeside med logo og navn</summary>
   <p>
      
![IISWebsite](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/IISWebsite.png)

</p>
</details> 

### Folder Redirection
<details><summary>Her kan man se at der er sat Folder Redirection op på Dokument mappen, og den er sat til \\MOOSESERVER\UserFolders$, som er det mappe som brugerne skal ligge i.</summary>
   <p>
      
![FolderRedirect01](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/FolderRedirect01.png)

</p>
</details> 
<details><summary>Her kan man se at der er sat Folder Redirection op på Skrivebords mappen, og den er sat til \\MOOSESERVER\UserFolders$, som er det mappe som brugerne skal ligge i.</summary>
   <p>
      
![FolderRedirect02](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/FolderRedirect02.png)

</p>
</details> 
<details><summary>Her laver vi en gpupdate /force, for at få Folder Redirection gruppepolitikken.</summary>
   <p>
      
![FolderRedirect03](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/FolderRedirect03.png)

</p>
</details> 
<details><summary>Her kan man se at Dokument mappen nu kører på serveren, gennem Folder Redirection.</summary>
   <p>
      
![FolderRedirect04](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/FolderRedirect04.png)

</p>
</details> 
<details><summary>Her kan man se at Skrivebords mappen nu køre på serveren, gennem Folder Redirection.</summary>
   <p>
      
![FolderRedirect05](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/FolderRedirect05.png)

</p>
</details> 

### Afdelings mapper (NTFS og Share rettigheder)
<details><summary>Her kan man se at gruppepolitikken bliver lavet, og at stien er sat til \\MOOSESERVER\Designer.</summary>
   <p>
      
![ShareRights01](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/ShareRights01.png)

</p>
</details> 
<details><summary>På billedet kan man se NTFS rettigheder for Designer afdelingen, i deres afdelings mappe.</summary>
   <p>
      
![ShareRights02](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/ShareRights02.png)

</p>
</details> 
<details><summary>På billedet kan man se Share rettigheder for Designer afdelingen, i deres afdelings mappe.</summary>
   <p>
      
![ShareRights03](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/ShareRights03.png)

</p>
</details> 
<details><summary>Efter en gpupdate /force, kan vi se at afdelings mappen virker fint, samt vores public drive også virker som den skal.</summary>
   <p>
      
![ShareRights04](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/ShareRights04.png)

</p>
</details> 

### Backup løsning Bilag
<details><summary>Her kan man se at vi har sat en Shadow Copy op til at køre på UserFolders$ drevet.</summary>
   <p>
      
![Backup01](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/Backup01.png)

</p>
</details> 
<details><summary>På billedet kan man se at vi har sat vores Shadow Copy til at køre hver dag kl. 20:00.</summary>
   <p>
      
![Backup02](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/Backup02.png)

</p>
</details> 

### Opsætning af Disk Quota
<details><summary>For at kunne sætte Disk Quota op, skal man installere en server rolle, som hedder “File Server Resource Manager”.</summary>
   <p>
      
![DiskQuota01](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/DiskQuota01.png)

</p>
</details> 
<details><summary>Inde i Server Manager, og under “Shares” finder man den shared mappe, men gerne ville sætte en Disk Quota på. Højre klik på mappen og tryk på “Configure Quota”</summary>
   <p>
      
![DiskQuota02](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/DiskQuota02.png)

</p>
</details> 
<details><summary>Her skal man vælge den “template”, som man enten selv har oprettet eller også kan man vælge en af dem der allerede er lavet. Vi har lavet en der hedder DepartmentQuotas, som vi vælger. Når vi har markeret den, så trykker man bare på “Ok”. Denne Disk Quota har vi sat på alle vores afdelingsdrev.</summary>
   <p>
      
![DiskQuota03](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/DiskQuota03.png)

</p>
</details> 
<details><summary>Her kan man se at vores Disk Quota virker på vores Administration drev.</summary>
   <p>
      
![DiskQuota04](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/DiskQuota04.png)

</p>
</details> 

### WSUS Konfiguration Bilag
<details><summary>På billedet kan man se at vi vælger at synchronicer fra Microsoft Update.</summary>
   <p>
      
![WSUSConfig01](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/WSUSConfig01.png)

</p>
</details> 
<details><summary>På billedet kan man se at vi kun vælger at installere Danske og Engelske versioner.</summary>
   <p>
      
![WSUSConfig02](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/WSUSConfig02.png)

</p>
</details> 
<details><summary>På billedet kan man se at vi har valgt alle opdateringer inde for Windows.</summary>
   <p>
      
![WSUSConfig03](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/WSUSConfig03.png)

</p>
</details> 
<details><summary>På billedet kan man se at vi har valgt alle Classifications.</summary>
   <p>
      
![WSUSConfig04](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/WSUSConfig04.png)

</p>
</details> 
<details><summary>På billedet kan man se at vi har valgt at synkroniser automatisk hver dag kl. 16:00</summary>
   <p>
      
![WSUSConfig05](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/WSUSConfig05.png)

</p>
</details> 

### WSUS Fejl
<details><summary>Her er et billede af en af de fejl vi fik oftest</summary>
   <p>
      
![WSUSError01](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/WSUSError01.png)

</p>
</details> 
<details><summary>Her er en anden fejl, vi også har fået en del gange</summary>
   <p>
      
![WSUSError02](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/WSUSError02.png)

</p>
</details> 

### Print Server
<details><summary>Installation af print server</summary>
   <p>
      
![PrintServer01](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/PrintServer01.png)
![PrintServer02](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/PrintServer02.png)

</p>
</details>

### Opretning af SSL certifikat    
<details><summary>Her trykker man på “Server Certificates”.</summary>
   <p>
      
![ServerCertificates](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/ServerCertificates.png)

</p>
</details>  
<details><summary>Tryk på “Create Self-Signed Certificate…” ude i højre side.</summary>
   <p>
      
![SelfSignedCertificate](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/SelfSignedCertificate.png)

</p>
</details> 
<details><summary>Her skal man give den et navn, samt vælge hvad type certifikat det skal være.</summary>
   <p>
      
![NameCertificate](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/NameCertificate.png)

</p>
</details> 
<details><summary>Her kan man se at “servertek” certifikatet er blevet oprettet.</summary>
   <p>
      
![ServerTek](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/ServerTek.png)

</p>
</details> 

### Opsætning af FTP server med SSL  
<details><summary>FTP rollen installere gennem Server Manager -> Manage -> Add Roles and Features. Der ligger den under Server Roles og under Web Server (IIS).</summary>
   <p>
      
![ServerManager](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/ServerManager.png)

</p>
</details>  
<details><summary>På den side du gerne ville have FTP serveren, skal du højreklikke og tryk på “Add FTP Publishing…”.</summary>
   <p>
      
![FTPPublishing](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/FTPPublishing.png)

</p>
</details> 
<details><summary>Her skal man vælge hvilken IP adresse, FTP serveren skal køre på, man kan også vælge hvilken port det skal køre, men som standard kør det på port 21. Samt vælger det SSL certifikat der blevet lavet tidligere.</summary>
   <p>
      
![FTPIP](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/FTPIP.png)

</p>
</details> 
<details><summary>I Authentication vælger vi Basic Authentication
I Authorization vælger vi “Specified roles or User groups”, og i det input felt der er skriver vi den gruppe, som skal have adgang til FTP serveren, i vores tilfælde er det “Developer” gruppen. Vi giver dem read / write permissions.
</summary>
   <p>
      
![BasicAuthentication](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/BasicAuthentication.png)

</p>
</details> 
</details> 
<details><summary>Her kan man se at der er adgang til FTP serveren, og man kan se websites filer.</summary>
   <p>
      
![FTPWorking](https://github.com/WackyMoose/H2ServerTechnology/blob/main/Images/FTPWorking.png)

</p>
</details> 
