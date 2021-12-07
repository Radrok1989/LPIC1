### Einführung in die Netzwerk-Kommunikation  
![Osi-model](bilder/Osi.png)
### Wireshark installieren und starten
  * Wireshark ist ein Netzwerksniffer, der alle Datenpakete auf dem Netzwerk mitschneiden kann, die an der eigenen Schnittstelle hereinkommen under herausgehen
  * Wireshark wird für Windows, Linux/Unix angeboten
  * Wireshark erfordert eine grafische Oberfläche (z.B. GNOME oder KDE)
  * Wireshark unterstützt zahlreiche Filterregeln, sowohl während des Mitschnitts (Capture-Filter) als auch danach (Display-Filter)
  * Wireshark stellt mitgeschnittene Pakete in drei Fenster dar:
    * Paketliste: Enthält jedes einzelne Paket
    * Details: Enthält die Interpretation der Daten
    * Rohdaten: Enthält eine hexadezimale Darstellung
  * ICMP wird für die Übermittlung von Fehler- und Statusmeldungen genutzt
  * Ping nutzt ICMP Echo Request (Typ 8) und Echo Reply (Typ 0)

### Einführung in die IPv4-Adressierung  
![](bilder/Einführung%20IPv4.png)  
Private IP-Adressbereiche nach RFC 1918  
![](bilder/Einführung%20IPv4-2.png)  
  * Private IP-Adressen können in jedem lokalen Netzwerk nach Belieben eingesetzt werden
  * Private IP-Adressen werden im Internet nicht geroutet

**Network Address Translation** (NAT) ermöglicht die Kommunikation interner Systeme mit privaten IP-Adressen mit Systemen aus dem Internet.  
![](bilder/Einführung%20IPv4-3.png)  
Spezielle Adressen:
  * Loopback (localhost): 127.0.0.0/255.0.0.0 -> meist 127.0.0.1
  * APIPA/ZeroConf: 169.254.0.0/255.255.0.0

### Klassisches Subnetting  
![](bilder/Subnetting-1.png)  
![](bilder/Subnetting-2.png)  
![](bilder/Subnetting-3.png)  
Pro Subnetz sind 64-2=**62 Hostadressen** verfügbar
**Aufgabe** in welchem Subnetz befinden sich die folgenden Hosts:
  * 192.168.125.55 / 255.255.255.192 -> Subnetz 192.168.125.0
  * 192.168.125.82 / 255.255.255.192 -> Subnetz 192.168.125.64
  * 192.168.125.212 / 255.255.255.192 -> Subnetz 192.168.125.192

![](bilder/Subnetting-4.png)  
![](bilder/Subnetting-5.png)  
![](bilder/Subnetting-6.png)  
Wie viele Bits werden im Subnetz-Anteil benötigt? Antwort: **4 Bits (16 Kombinationen)**  
Wie lautet die Subnetzmaske? Antwort: **16+4=20Bits => 255.255.240.0**  
Was ist das *Interesting Octet*? Antwort: **das 3. Oktett**  
Wie Gross ist die Schrittweite zwischen den Subnetzten (-> **Magic Number**)?  
  * Variante A -ablesen: 4 Bits von links in der Byte-Tabelle -> Schrittweite = **16**
  * Variante B - rechnen: 256-240= **16**  
**Besonderheiten im Klasse-B-Netz:** Teile des 3. und das gesamte vierte Oktett stellen den Hostanteil dar  
![](bilder/Subnetting-7.png)  

### VLSM und CIDR
  * Ausgehend von den Klassen-Netzen (Classful Networks) werden gleichgrosse Kuchenstücke erstellt
  * Fehlennde Flexibilität bei Anforderungen an unterschiedlich grossen Subnetzen
  * Die Klassen bestimmen die grundsätzliche Netzgrösse
  * Klassenbasiertes Subnetting löst nicht die grundsätzlichen Probleme beim Routing im Internet  
![](bilder/VLSM-1.png)  
![](bilder/VLSM-2.png)  
![](bilder/strich.png)  
  * VLSM ermöglicht die beliebige Aufteilung des Ausgangsnetzwerks 
  * Die Subnetze können beliebige Subnetzmasken aufweisen
  * Ziel: möglichst wenig "Verschnitt" an IP-Adressen
  * Effizientere Nutzung des IP-Adressenbereichs
  * Routingtechnisch: Optimierung durch Routen-Zusammenfassung (Supernetting)  
![](bilder/VLSM-3.png)  
VLSM dient insbesondere der optimalen Auffteilung der Netze. Primäre Fragenstellung: Wieviele Hostadressen werden benötigt?  
Ansatz: **Wie viele Bits muss ich für den hostanteil reservieren?**  
![](bilder/VLSM-4.png)  
![](bilder/VLSM-5.png)   
Beispiel: in einem Subnetz werden 120 Hosts benötigt  
-> 7 Bits müssen für den Hostanteil reserviert werden  
-> 32-7=25 Bits bleiben für den Netzanteil  
-> Präfix /25 bzw. Subnetzmaske: 255.255.255.128  
![](bilder/VLSM-6.png)  

### Arp und die Mac-Adressen  
  * ARP steht für Address Resolution Protocol
  * ARP löst IP-Adressen in MAC-Adressen auf
  * Innerhalb eines Subnetzes wird auf Layer 2 mittels MAC-Adressen kommuniziert
  * Wird ein IP-Paket an ein Ziel ausserhalb des eigenen Subnetzes versendet, so wird es and die MAC-Adresse des Default-Gateways bzw. zuständigen Routers gesendet, der es dann via Routinglogik weiterleitet  
![](bilder/ARP.png)  

### TCP und UDP (Transmission Control Protocol, User Datagram Protocol)
  * TCP und UDP sind die generischen Transprtprotokolle auf Layer 4 (Transport Layer)
  * die meisten Anwendungen nutzen das Transmission Control (TCP) zur Übertragung der Anwendungsprotokolle (wie HTTP, FTP, SSH, etc.)
  * TCP erstellt Sitzungen und überwacht die Datenkommunikation
  * Bei jeder TCP-Sitzung wird ein 3 Way-Handshake durchgeführt:
    * 1. Packet: SYN-Flag gesetzt (Client zum Server)
    * 2. Packet: SYN/ACK-Flag gesetzt (Server zu Client)
    * 3. Packet: ACK-Flag gesetzt (Client zum Server)
  * Mittels SEQ- und ACK-Numbers prüft TCP. ob der Kommunikationspartner jeweils alle Daten erhalen hat (Fehlerkorrektur/Errorcorrection)
  * Window-Size: legt die Grösse des Empfangspuffers fest (Flusskontrolle/Flowcontrol)
  * TCP und UDP nutzen Ports. Jede Netzwerkanwendung wird an einen Port zwischen 1 und 65535 gebunden (ermöglicht Multiplexing)
  * Die Serverports liegen unter 1024 (Well-Known-Ports)
  * Client-Ports werden vom Betriebssystem dynamisch festgelegt
  * Das User Datagram Protocol (UDP) ist eine leichtgewichtige Alternative zu TCP
  * UDP erstellt keine Sitzungen, Übertragungen geschehen nach "Best Effort"  

### Die Well-Known-Ports
  * 21/tcp - File Transfer Protocol (FTP)
  * 22/tcp - Secure Shell (SSH)
  * 23/tcp - Telnet
  * 25/tcp - Simple Mail Transfer Protocol (SMTP)
  * 53/udp+tcp - Domain Name System (DNS)
  * 67+68/udp - Dynamic Host Configruation Protocol(DHCP)
  * 69/udp - Trivial File Transfer Protocol(TFTP)
  * 80/tcp+udp - Hypertext Transmition Protocol (HTTP)
  * 110/tcp - Post Office Protocol (POP3)
  * 123/udp - Network Time Protocol (NTP)
  * 143/tcp - Internet Message Access Protocol (IMAP4)
  * 161/udp - Simple Netwrok Management Protocol (SNMP)
  * 162/udp - SNMP Trap
  * 389/tcp - Lightweight Directory Access Protocol (LDAP)
  * 443/tcp - Secure Socket Layer (SSL) / Transport Layer Securtiy (TLS) -> HyperText Transmition Protocol Secure (HTTPS)
  * 514/udp+tcp - Syslog
  * 636/tcp - Secure LDAP (LDAPS)  

In der Datei **/etc/services** sind fast alle Standard-Ports enthalten

### Wichtige TCP/IP-Anwendungen  
**File Transfer Protocol (FFTP)- Ports 21/tcp und 20/tcp (bei aktivem File Transfer Protocol):**  
  * Eines der ältesten Protokolle
  * Wird zur Übertragung von Dateienn verwendet
  * Unterstützt Active und Passive FTP
  * Überträgt im Klartext bzw. unverschlüsselt
  * unterstützt Anonymous-FTP, Zugriff ohne Authentisierung möglich
  * Auch heute noch an vielen Stellen im Einsatz
  * Unter Linux existeiren zahlreiche Varianten, wichtige sind:
    * **ProFTPd**: umfangreicer, einfach zu konfiguerender Server
    * **vsFTPd**: auf Sicherheit ausgelegter FTP-Server
  * Sichere Varianten via SSH-Suite (SCP und SFTP)  

**Secure Shell (SSH) - Port 22/tcp:**
  * Standard-Anwendung für Systemadministration von Linux-Systemen und Netwerk-Geräten
  * Unterstützt zahlreiche Algorythen, die während der Initialisierugsphase festgelegt werden
  * Die SSH-Suite in der Version 2 bringt SCP und SFTP mit zur sicheren Dateiübertragung
  * Unter Windows unter anderem mit PuTTY und WinSCP einsetzbar
  * unter Linux ist OpenSSH-Server der Standard
  * SSh erstetzt rlogin und rsh sowie Telnet (Klartextkommunikation
  * SSh unterstützt X11 Forwarding)  

**Simple Mail Transfer Protocol (SMTP) - Port 25/tcp:**
  * Eines der ältesten TCP/IP-Protokolle (1982, noch älter als FTP)
  * Standard-Protokoll für den Mail-Versand
  * Überträgt Datenn in Klartext
  * erweiterung dur Extended SMTP (ESMTP)
    * ermöglicht dieverse Zusatzfunktionen (optional vom Server bereitgestellt)
    * Unterstützt auch diverse Authentifizierungsmethoden
  * Heutzutage meistens via TLS abgesichert  

**Dynamic Host Configuration Protocol (DHCP) - Ports 67+6/udp:**  
  * Automatische IP-Konfiguration von DHCP-Clients
  * Wird in den meisten Netzwerkumgebungen eingesetzt
  * Fast jeder HomeOffice-Router unterstützt DHCP
  * Mindestkonfigurationsparameter im Rahmen einer Lease:
    * IP-Adresse
    * Subnetzmaske
    * Default-Gateway
  * Viele zusätzliche DHCP-Optionen möglich:
    * DNS-Server
    * DNS-Suffic
    * TFTP-Server (für PXE-Boot)
  * Die DHCP-Lease muss nach Ablauf zurückgegeben werden  

**Domain Name System (DNS) - Port 53/udp+tcp:**
  * Hierarchisches System zur Namensauflösung im Internet  
  * Ersetzt bzw. ergänzt Datei **/etc/hosts**
  * Namensraum wird in Zonen (domains) unterteilt
  * Verwaltung der Zonen kann delegiert werden
  * Toplevel-Domain (TLD): verwaltet oberste Ebene
  * Domain: wird an Institutionen oder Einzelersonen vergeben
  * Subdomain: Optionale Untereinheit
  * Hostname: Werden in der Zone verwaltet, die für eine Domain zuständig ist
  * Die Zone enthält verschiedene Eintragstypen (RRs), z.B. A, AAAA, MX, NS, TXT, PTR
  * Jede Zone muss im Internet redundant durch mindestens zwei DNS-Server geführt werden  

**HyperText Transfer Protocol (HTTP) - Port 80/tcp:**  
  * Kommunikationsprotokoll ffür Webinhalte (WWW)
  * HTTP ist statuslos (viele einzelne Verbindungen)
  * Mittlerweile ist HTTP/2 veröffentlicht
  * Wichtige HTTP-Methoden:
    * GET (Für normale Anfragen)
    * POST (Für formularbasierten Datenupload)
    * HEAD (Abgleich von Browsercache-Daten)
  * Wichtige HTTP-Statuscodes:
    * 2xx - Erfolgreiche Operationen (z.B. 200 OK)
    * 3xx - Umleitung des Browsers (z.B. 301 Moved Permanently zur Umleitung auf HTTPS)
    * 4xx - Client-Fehler (z.B. 404 Not Found)
    * 5xx - Server-Fehler (z.B. 502 Bad Gateway für Proxy-Weiterleitungsfehler)
  * Übertragung heute meist via HTTPS (SSL/TLS) Port443/tcp  

**Lightweight Directory Access Protocol (LDAP) - 389/tcp + 636/tcp (LDAPS)
  * Netzwerkprotokoll zur Abfrage von LDAP-basierten Verzeichnisdiensten
  * Verzeichnisdienste speichern Informationen zu Ressourcen im Netwerk, z.B.:
    * Benutzer- und Gruppeninformationen
    * Computer
    * Drucker
    * Standortinformationen
    * Usw.
  * Jedes Objekt hat einen eindeutigen Identifier, z.B.
    * *O=acme, c=de, ou=webdesign, ou=people, uid=juser*
  * Microsoft Windows Active Directory basiert auf LDAP

### Einführung in IPv6
  * Bereits Mitte der 1990er Jahre war absehbar, dass die IPv4-Adressen ca. 2011 ausgehen
  * IPv6 wurde als Nextgen Internet Protocol entwickelt (RFC 2460 1998)
  * IPV6 bietet einen deutlich erweiteren Adressraum:
    * IPv4: 32 Bit: ca. 4,3 Milliarden Adressen
    * IPv6: 128 Bit: ca 340 Sextilionen Adressen:
      * *340.282.366.920.938.463.463.374.607.431.768.211.456*
    * Das reicht, um jedem Sandkorn auf der Erder mehrere Adressen zuzuweisen
    * Der Adressraum von IPv6 reicht aus, um jedem Quadratmilimeter der Erdoberfläche inklusive der Ozeane mit rund 600 Billiarden Adressen zu versorgen.  
    * 
![](bilder/strich.png)
  * Die 128 Bits der Adresse werden in acht hexadezimale, 16-Bit lange Blöcke unterteilt, die durch Doppelpunkte getrennt werden, z.B.:
    * *2001:0DB8:0000:0000:0000:0208:FEC5:5E7A*
  * Führende Nullen können ausgelassen werden:
    * *2001:DB8:0:0:0:208:FEC5:5E7A*
  * Aufeinanderfolgende Null-Blöcke können EINMAL pro Adresse durch zwei Doppelpunkte ersetzt werden
    * *2001:DB8::208:FEC5:5E7A*
  * IPv4-Adressen können in IPv6-Adressen eingebettet (bzw. zum Schluss angehängt) werden, z.B. *X:X:X:X:X:X:192.168.0.1 oder 0:0:0:0:0:0:192.168.0.1 bzw ::192.168.0.1, oder aber: ::C0A8:1  

![](bilder/strich.png)
  * Präfixe geben den Netzanteil der Adresse wieder(wie bei IPv4)
  * Die ersten 64 Bit einer Adresse können als Netzanteil frei aufgeteilt werden, während die letzten 64 Bit immer die Interface-ID darstellen  
![](bilder/IPv6.png)  

![](bilder/strich.png)
  * Multicast spielt bei IPv6 eine viel grössere Rolle als bei IPv4, unter andere für:
    * Neighbour Discovery 
    * Router Discovery
    * U.a
  * ICMPv6 wird für diverse essentielle Funktionen genutzt (u.a. ND und RD)
  * Neighbour Discovery ersetzt ARP
  * IPv6 unterstützt Autoconfiguration (Netz-Präfix wird vom IPv6-Router geliefert)
  * DHCPv6 kann ergänzend zur Autoconfiguration (stateless) oder anstatt Autoconfiguration (stateful) eingesetzt werden
  * DHCPv6 ist ein eigener Dienst und nicht IPv4 kompatibel (Client: 546/udp, Server: 547/udp)
  * DNS wurde nur um AAAA-Einträge ergänzt und kann normal weiterverwendet werden
  * Die meisten Anwendungen funktionieren weitgehend problemlos mit IPv6