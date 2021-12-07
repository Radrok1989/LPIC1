### CUPS & Co - Die Druckersystem-Standards
  * Das alte System V Printing Systems spricht die lokal angeschlossene Druckerhardware über **/dev/lp1, /dev/lp2, usw.** an 
  * Der Befehl `lp` dient hier zum Drucken von Dockumenten und unterrstützt noch diverse weitere Funktionen
  * Mit `lpstat` wird die Druckerwarteschlange undd die ausstehenden Druckjobbs angezeigt
  * `lpadmin` ist ein Verwaltungstool zur Administation des Druckersystems
  * Mit `lpmove`ist es möglich, Druckerjobs zwischen verschiedenen Druckerwarteschlangen zu verschieben
  * Das Berkley Printing System führte zwei Druckerprotokolle ein, die für die Kommunikation mit lokalen, aber auch Remote-Druckern im Netzwerk konzipiert sind
    * Line Printer Daemon Protocol (LPD)
    * Line Printer Remote Protocol (LPR)
  * Der Befehl zum Drucken heisst hier `lpr, lpq` zum Anzeigen der Druckerwarteschlange und `lprm` zum Löschen von Druckerjobs
  * Heute ist CUPS (Common UNIX Printig System) der Linux-Drucker-Standard
  * CUPS unterstützt LDP, sowie das Internet Printer Protocol (IPP) und ausserem noch eingeschränkt das Windows SMB-basierte Drucken
  * CUPS unterstützt aus Kompatibilitätsgründen diverse Befehle wie `lp, lpr, lpq, lprm` oder `lpadmin`

### Drucker einrichtenn in CUPS
  * CUPS ist auf vielen Linux-Systemen vorinstalliert
  * Die Druckerverwaltung geschieht web-basiert über Browser mit `localhost:631`
  * Dort können z.B. lokale Drucker oder Netzwerk-Drucker hinzugefügt und gelöscht werden, Drucker im Netzwerk freigegeben werden, Standarddrucker festgelegtm eine Testseite gedruckt werden oder die Druck-Aufträge verwaltet werden
  * Die Berechtigung einen Drucker einzurichten, setzt die Mitglieschaft in der Gruppe `lpadmin` voraus
  * Unter **/etc/cups** befinden sichh die CUPS-Konfiguration
  * In der Datei **printers.conf**, die wir nur als Admin öffnen können, sehen wir die konfigurierten Drucker mit ihren Einstellungen
  * `cupsd` ist der entsprechende Daemon-Prozess
  * DDie Hauptkonfigurationsdatei für diesen Prozess ist **/etc/cups/cupsd.conf**

### Druckerverwaltung auf der Kommandozeile
  * Drucken von der Kommandozeile:
    * `lp -d MyPrinter textdatei.txt` - (System V Printing-Systems)
      * -d (für Destination) ermöglicht die Angabe des gewünschten Druckers
      * Ohne -d wird das Dokument an die Warteschlange des Standard-Druckers geschickt
    * `lpr -P MyPrinter textdatei.txt` - (Berkley Printing-Systems)
      * -P (für Printer) ermöglicht die Angabe des gewüschten Druckers
      * ohne -P wird das Dokument an die Warteschlange des Standard-Druckers geschickt
  * Welcher der beiden Befehle genutzt wird, ist im Prinzip egal. Alle Legacy-Befehle, also von den anderen Systemen übernommene Befehle, sind ohnehin nur noch leere Hüllen zu Kompatibiltätszwecken, hinter denen sich CUPS-Verwaltungstools verstecken
  * mit `lpq` kannst du dir die Druckerwarteschlange anzeigen lassen. Ohne Option zeigt der Befehl die Warteschlange des Standarddruckers an
  * Mit `lpq -a` werden alle Aufträge angezeigt
  * Druckeraufträge können gelöscht  werden mit `lprm <Auftrags-ID>` - wird keine ID angegeben, wird der aktive Auftrag gelöscht
  * Mit `lprm -P MyPrinter` werdenn alle Druckaufträge für den angegeben Drucker auf eimmal gelöscht. Da ein normaler Bernutzer nur seine eigenen Druckaufträge löschen darf, erfordert dieser Befehl ggf. Admin-Rechte, also ein sudo davor
  * Mit `lpc status`werden alle verfügbaren Drucker undd deren aktueller Status ausgegeben
  * Unabhängig vom Status des Druckers ist es möglich, Druckjobs einzustellen. Dies kann mit CUPS manipuliert werden: `cupsaccept` / `cupsreject`
  * Ist das Drucken eingeschlatet, werden die Druckjobs auch tatschlich an das Druckgerät weitergeleitet. Dies kann mit CUPS manipuliert werden: `cupsenable` / `cupsdisable`
  * mit `lpadim` z.B. können Druckertreiber, -Warteschlangen, und -Klassen konfiguriert werden und `lpoptions` setzt Druckeroptionen und -Voreinstellungen

### Nützliche Befehle
  * `lp` - Kommando zum lokalen Drucken mit dem System V Printing System
  * `lp -d <Drucker>` - Drucken auf bestimmten Drucker mit dem System V Pringting System
  * `lpstat`Druckerjobs anzeigen mit dem System V Printing System
  * `lpadmin` - Verwaltungstool zur Administration des Druckersystem
  * `lpoptions` - Druckeroptionen und -Voreinstellungen setzen
  * `lpmove` - Druckerjobs zwischen Druckerwarteschlangen verschiebben
  * `lpr` - Kommando zum Drucken mit dem Berkeley Printing System
  * `lpr -P <Drucker>` - Drucken auf bestimmten Drucker mit dem Berkeley Printing System
  * `lpq` - Druckerjobs des Standarddruckers anzeigen
  * `lpq -P <Drucker>` - Druckjobs eines Bestimmten Druckers anzeigen
  * `lpq -a` - Druckjobs aller Drucker anzeigen
  * `lprm` - Löschen von gerade aktivem Druckerjob
  * `lprm <ID>` - Löschen von Druckerjobs unter Angabe der ID
  * `lprm -P <Drucker>`Löschen aller Druckjobs eines bestimmten Druckers
  * `lpc status` - Zeigt verfügbare Drucker und deren Status an
  * `cupsaccept` - Erlaubt Zugriff auf die Druckerwarteschlange mit CUPs
  * `cupsreject` - Verbietet Zugriff auf die Druckerwarteschlange mit CUPs
  * `cupsenable <Drucker>` - Drucker aktivieren mit CUPS
  * `cupsdisable <Drucker>` - Drucker deaktivieren mit CUPS