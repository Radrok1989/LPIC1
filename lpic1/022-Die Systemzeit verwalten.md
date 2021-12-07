### Die Systemzeit verwalten
  * Die Zeit ist aus verschiedenen Gründen wichtig:
    * Protokolleinträge werden mittels Zeitstempel datiert
    * Authentifizierungsvorgänge erfolgen teilweise mittels Zeitstempel (zB. Kerberos, Zertifikate)
  * Koordinierte Weltzeit heisst UTC (Universal Time Coordinated), ehemals GMT
  * Sommerzeit in Europa heisst CEST (Central European Summer Time)
  * Der Befehl `date`zeigt die Zeit an und ermöglicht es, den Zeitstempel des Systems zu setzten 
  * Es gibt eine Systemzeit und eine Hardware-Zeit (Mainboard)
  * `hwclock`zeigt die Hardware-Zeit an
  * `cat /etc/adjtime` zeigt, in welcher Zeitform die Hardware-Zeit gesetzt ist, meist UTC
  * Hardware-Zeit und Systemzeit können manuell gesetzt und synchronisiert werden.

### Zeitsynchronisation mit NTP
  * NTP (Network Time Protocol) ermöglicht die automatische Zeitsynchronisation mit Zeitservern
  * NTP unterscheidet zwischen Stratum-werten:
    * Stratum 0: die Zeitquelle selbst (zB. Atom- oder Funkuhr)
    * Stratum 1: Systeme, die sich direkt von der Zeitquelle die Zeit holen
    * Stratum 2: Systeme, die sich direkt von der Zeitquelle die Zeit holen
    * usw.  
![NTP-Schichten](bilder/zeitsynchronisation.png)
  * Zeitserver können bei https://www.pool.ntp.org gefunden werden  
  * Ubuntu nnutzt einen Daemon namens timesyncd aus Systemd, um die Zeit zu synchronisieren
  * mit ntpdate kann ein Zeitserver festgelegt werden für NTP, ntpdate ist veraltet
  * ntpd ist der modernere Befel, er wird über **/etc/ntp.conf** konfiguriert
  * ntpd ist ein Daemon, der sich an Port 123/udp bindet und  dort auf NTP-Anfragen lauscht
  * Über die Konfiguration von NTP-Server-Pools können diverse Zeitserver angegeben werden
  * Weicht die Zeit ab, kann NTP den Zeitwert smooth anpassen - die Zeit läuft ddann eine Zeit lang schneller oder langsamer, je nachdem (damit werden Fehler und Zeitsprünge vermieden)
  * Beim Herunterfahren überschreibt der Zeitdaemon den Wert der Hardware-Uhr

### Nützliche Befehle im bezug auf NTP:
  * `date` - zeigt die Systemzeit an
  * `hwclock` - Fragt die Zeit auf dem Mainboard an
  * `date --help` - Zeigt die Optionen von date
  * `date 082709442021`Setzt die Zeit auf 27. Aug. 2021 09:44 Uhr
  * `hwclock -w` - Überschreibt die  Hardware-Zeit mit der Systemzeit
  * `hwclock -s` - überschreibt die Systemzeit mit der Hardware-Zeit
  * `ntpdate 0.de.pool.ntp.org` - veralteter Befehl zum Setzen eines Zeitservers für NTP
  * `systemctl start ntpd` - Startet den NTP-Daemon
  * `ntpq -p` Zeigt den Status der NTP-Kommunikation
