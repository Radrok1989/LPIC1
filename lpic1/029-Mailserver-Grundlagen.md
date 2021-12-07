### Grundlagen der E-Mail-Kommunikation
  * Der Mail-Client, z.B. Outlook oder hunderbird, wird als *Mail User Agent*, kurz: MUA, bezeichnet. Er läufft auf dem System des Benutzers
  * Schreibt der Benutzer eine E-Mail, wird diese an den im MUA konfigurerten Mailserver weitergeleitet
  * Der Mailserver wird als *Mail Transfer Agent, kurz: MTA bezeichnet
  * Erhält der MTA eine Mail von einem authentifierten MUA, reiht er diese Mail in seine Mailqueue, also Mail-Warteschlange ein, wo sie auf ihre Verarbeitung wartet
  * Der MTA identifiziert anhand der Domain der Empfängeradresse via DNS, welcher MTA für das Ziel zuständig ist.
  * Dazu fragt er den DNS-Server nach den MX-Einträgen für diese Domain
  * Er erhält eine Liste mit zuständigenn "Mail Exchangern" (MTAs) und wird der Priorität nach versuchen die Mailserver zu erreichen
  * Die Priorität ist als Ziffer für jeden MX-Eintrag einer Zone hinterlegt, je niedriger die Ziffer desto höher die Priorität
  * Über das Simple Mail Transfer Protocol, kurz: SMTP - entweder auf Port 25 oder 587/tcp wird eine Kommunikation zum zuständigen MTA aufgebaut
  * Der empfangende MTA prüft, ob er einen entsprechenden benutzer kennt. Ist dies der Fall, stellt er die Mail zu, indem er sie im Postfach-Verzeichnis des Benutzers ablegt
  * Der Empfänger ruft via *Post Office Protocol*, kurz: POP3, oder *Internet Mail Access Protocol*, kurz IMAP4, die neuen E-Mails seines Postfaches ab
  * Der Unterschied zwischen POP3 und IMAP4 ist, dass POP die Nachrichten komplett herunterlädt und dem Benutzer damit auch offline verfügbar macht, während IMAP zur Bereitstellung des Inhalts des Postfaches immer eine Netzwerkverbindung zum Server benötigt

### Mailserver Postfix einrichten
  * Mailserver-Systeme bestehen meistens aus diversen Komponenten und sind vergliechbar mit Web-Plattformen
  * Ursprünglich haben praktischh alle Linux-Systeme *Sendmail* verwendet, einer dessen Nachfolger ist Postfix (andere sind qmail oder Exim)
  * "Smarhost" bezeichnet einen Mailserver, an den der Mailserver alle seine zu versendenden Mails weiterleitet
  * Heutzutage werden im Internet oftmals nur Mails von Mailservern mit einer entsprechenden Reputation angenommen. Der Versand von Mails durch einen selbsterstellten Mailserver wird daher oftmals abgelehnt
  * Daher versenden viele interne Mailserver ihre Mails an einen Mailserver des Providers (Smarthost), der eine entsprechende Reputation hat und die Mails an den Adressaten versendet
  * Eine Liste alle verfügbaren Konfigurationsoptionen von Postfix zeigt der Befehl `postconf`
  * Die Konfigurationsdateienn für Postfix liegen unter **/etc/postfix**
  * Die wichtigste Konfigurationsdatei ist **main.cf**
  * Die Direktive `smtpd_relay_restrictions` bestimmt, von welchen Clients Mails weitergeleitet werden. Das soll verhindern, dass unberechtigte Systeme Mails über den Mailserver leiten und der Mailserver zu einem "Open-Relay" wird
  * Die Option `permit_mynetworks`besagt, dass alle IP-Adressen und Subnetze, die in der Variablen `mynetworks` enthalten sind, Mails über diesen Mailserver versenden dürfen
  * Unter `mydestination` werden alle Domains aufgeführt, für die dieser Mailserver zuständig ist
  * Der Mailserver wird neu gestartet mit `systemctl restart postfix`
  * Überprüfung, dass Postfix läuft mit dem Befehl `systemctl status postfix`

### SMTP-Kommunitkation
  * Es ist möglich, via Telnet eine SMTP-Session manuell durchzuführen
  * Der Befehl telnet `<Ip-Mailserver> 25`baut mit Hilfe von Telnet eine Verbindung über Port 25 (SMTP) mit dem Mailserver auf
  * Eine Mail verfassen innerhalb der Telnet-Kommunikation:
    * `mail from: absender@domain.ch`
    * `rcpt to: empfaenger@domain.ch`
    * data
    * `subject: Betreff`
    * `Textinhalt der Mail (Diverse Zeilen möglich)`
    * `.`
  * Das Verfassen der Mail wird mit einem Punkt (".") in einer einzelnen Zeile abgeschlossen. Damit wird die Nachricht in die Warteschlange des Mailservers zur Verarbeitung gegeben
  * Unter **/var/mail** befinden sich die Postfächer der Benutzer. Diese werden angelegt, sobald ein Benutzer die erste E-Mail erhält. Meist existert ein Symlink auf **/var/spool/mail**

### Mailaliase und Weiterleitungen kofigurieren
  * In der Datei **/etc/aliases** werden Aliase und Weiterleitungen eingerichtet 
  * Nach jeder Änderung an der Datei **aliases** muss der Befehl `newaliases` ausgeführt werden. Dadurch wird eine aktuelle Datenbankdatei mit der Endung .db für aliases erstellt
  * Damit die Änderung wirksam werden, muss der Mailserver neu gestartet werden
  * Das Programm `mail` ist im Paket `mailutils` enthalten und kann mit `apt install mailutils` installiert werden (Debian-Derivate, bei CentOS paket mailx)
  * Der Befehl `mail -s "Testmail" empfaenger@domain.ch`leitet die ERstellung einer Mail durch das Tool `mail`ein
  * Wird in einer eigenen Zeile die Tastenkomination Ctrl+D eingegeben wird das Programm beendet und die Mail wird über das lokale "Sendmail"-System - also eigentlich Postfix - zugestellt
  * Der Befehl `mailq` dient zur Anzeige der derzeitigen Mailqeueu

### Nützliche Befehle
  * `postconf` - Zeigt eine Liste aller verfügbaren Konfigurationsoptionen von Postfix
  * `systemctl restart postfix` - Neustart des Postfix-Mailservers
  * `systemctl status postfix` - Status der Postfix-Mailservers abfragen
  * `telnet <Zieladresse> 25` - Baut eine TELNET-Verbindung über Port 25 (SMTP) auf
  * `mail -s "Testmail" empfaenger@domain.ch` - Leitet die ERstellung einer Mail durch das Tool **mail** ein
  * `mailq` - Anzeige der derzeitigen Mailqueue