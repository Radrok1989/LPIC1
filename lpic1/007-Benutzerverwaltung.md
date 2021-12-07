### Benutzerverwaltung
  * Linux ist ein Multiuser-System: Es können mehrere Benutzer das System verwenden, auch gleichzeitig
  * Jeder Benutzer hat normalerweise sein eigenes Benutzer-Konto
  * Obligatorisch ist ein Benutzername, mit dem der Benutzer sich am System anmeldet. Dieser Name muss auf dem System eindeutig sein
  * Linux unterscheidet generell zwischen Gross- und Kleinschreibung, das gilt auch für die Benutzernamen
  * Die UID, also die User-ID muss ebenfalls eindeutig sein und wird normalerweise vom System automatisch vergeben
    * Spezielle Benutzer haben IDs unter 1000, Root z.B. hat immer die ID 0
    * Normale Benutzer erhalten eine ID von 1000 aufwärts
  * Benutzer erhalten eine Login-Shell. Das ist die Umgebung, die für sie bereitgestellt wird, wenn sie sich auf der Konsole einloggen
  * Dash steht für Debian Almquist Shell und wird als Standard-Shell für Debian-Derivate verwendet
  * einem benutzer wird ein Home-Verzeichnis zugewiesen
    * root hat sein Home-Verzeichnis unter **/root**
    * Normaler Benutzer in der Regel unter **/home/Benuztername**
  * Einstellungen im Zusammenhang mit `useradd`und `userdel` werden in der Datei **/etc/login.defs** festgelegt
  * In der Datei **/etc/default/useradd** können weitere Voreinstellungen getroffen werden (zBsp. Festlegung der Login-Shell als Standard bei der Erstellung eines neuen Benutzers)
  * Gruppen werden verwendet, um mehreren Accounnts Zugriffsrechte auf eine Ressource zuzuweisen
  * Jedem Benutzer wird eine Hauptgruppe zugewiesen. Standardmässig wird hierfür eine Gruppe mit demselben Namen und derselben ID wie die des Benutzers angelegt
  * Als Alternative erhalten alle Benutzer dieselbe Hauptgruppe namens `users`. Sie hat meistens die ID 100. Das ist jedoch nicht so flexibel, daher erhalten Benutzer auf den meisten Systemen per Default ihre eigene Gruppe
  * Benutzer können Mitglied mehrerer Gruppen sein und werden teilweise automatisch bestimmten zusätzlichen Gruppen zugewiesen
  * Der Befehl `usermod` unterstützt fast alle Optionen, die auch `useradd` bereitstellt. Leider funktioniert die Option `-m` nicht wie bei `useradd`. Daher müssen das Home-Verzeichnis manuell kopiert, und die Zugriffsrechte entsprechend gesetzt werden.
  * Es existieren diverse Systembenuzter. Denn aus Sicherheitsgründen werden viele Dienste des Systems im kontext eines eigenen Benutzers betrieben, der in seinen Zugriffsrechten entsprechend gestaltet werden kann.
  * "Pseudo-Shells" wie bei **/false** oder **/sbin/nologin** stellen keine Umgebung bereit, bzw. verhindern, dass sich ein Benutzer interaktiv anmelden kann. Das wird als Sicherheitsfeature verwendet, um Systembenutzer, entsprechend auf das System selbst einzuschränken.
  * Change Age (chage) gibt Informationen aus **/etc/shadow** lesbarer im Terminal aus und kann (auch dialoggeführt)  Parameter anpassen
  * Die Hauptgruppe eines Benutzers ist in **/etc/passwd** erfasst, daher taucht der Benutzer in **/etc/group** nicht noch einmal auf
  * Wird eine neue Gruppe erstellt, zählt die GID automatisch bei der zuletzt erstellten ID weiter hoch
  * Das Programm `adduser` stellt eine dialoggeführte Erstellung von benutzern inkl. Passwort bereit
  * Das Programm `deluser` kann mit der Option `--remove-all-files` den Benutzer mit sämtlichen Dateien des Benutzers auf dem ganzen System löschen
  * Das Verhalten von `adduser` und `deluser` kann durch die beiden Dateienn **/etc/adduser.conf** und **/etc/deluser.conf** angepasst werden


### Die Benutzerverwaltungsdateien
  * Benutzer-Informationen werdden in drei Dateien verwaltet:
    * **/etc/passwd**
      * Login-Name
      * UID (Benutzer-ID)
      * GID (Hauptgruppen-ID)
      * GECOS-Feld (Kommentarfeld)
      * Home-Verzeichnis
      * Login-Shell
    * **/etc/shadow**
      * Login-Name
      * Passwort (als Hash)
      * Datum der letzten Passwort-Änderung (Tage seit dem 1.1.1970)
      * Minimale Anzahl an Tagen zwischen zwei Passwort-Änderungen
      * Anzahl der Tage an denen vor Passwortablauf gewarnt wird
      * Anzahl an Tagen bis zur Deaktivierung, nachdem das Passwort abgelaufen ist
      * Datum an dem Account automatisch deaktiviert wird
    * **/etc/group**
      * Gruppenname
      * Group ID
      * Gruppenmitglieder

### Nützliche Befehle
  * `useradd <user>` - Erstellt einen neuen Benuzter nach Standardverhalten
  * `useradd -m <user>` - Erstellt einen neuen Benutzer inkl. Homeverzeichnis und kopiert den Inhalt von **/etc/skel** dort hin
  * `useradd -D` - Zeigt Standardverhalten von `useradd`
  * `useradd -u <UID> <user>` - Erstellt neuen Benutzer mit festgelegter UID
  * `useradd -d <pfad> <user>` - Erstellt neuen Benutzer und konfiguriert angegebenen Pfad als Homeverzeichnis
  * `useradd -s /bin/bash <user>` - Erstellt neuen Benutzer und legt **/bin/bash** als Standard-Shell fest
  * `useradd -c <Kommentar> <user>` - Erstellt neuen Benutzer mit einem Kommentar
  * `usermod <user>` - Ändert Attribute von vorhandenem user
  * `usermod -G <group> <user>` - Weist < user > einer zusätzlichen Gruppe < group > zu
  * `usermod -g <GID> <user>` - Weisst < user > einer neuen Hauptgruppe < GID > zu
  * `userdel <user>` - Löscht angegebenen Benutzer
  * `userdel -r >user>` - Löscht Benuzter inkl. Home- und Mailverzeichnis
  * `groups <user>` - Zeigt alle Gruppen des angegebenen Benutzers
  * `groupadd <group>` - Erstellt eine neue Gruppe
  * `groupadd -g <id> <group>` - Erstellt neue Gruppe mit festgelegter GID
  * `groupdel <group>` - Löscht eine bestehende Gruppe (wenn sie keinen Benutzer mehr hat)
  * `passwd <user>` - Legt Passwort für < user > fest
  * `chage -l <user>` - Gibt Inhalte aus **/etc/shadow** für < user > in lesbarer Form im Terminal aus
  * `id <user>` - Zeigt viele Benutzerinformationen an
  * `su - <user>` - Identitätswechsel zu < user >
  * `echo $SHELL` - Zeigt Inhalt der Umgebungsvariable $SHELL
  * `cp -r <quelle> <ziel>` - Kopiert alle Inhalte rekursiv
  * `chown -R <user>:<group> <verzeichnis>` - Setzt Inhaber < user > und < group > für < verzeichnis >
  * `less <datei>` - Dateiinhalt über einen Pager im Terminal darstellen
  * `pwd` - Zeigt aktuellen Verzeichnis-Pfad
  * `ls -a` - Zeigt Inhalt des Verzeichnisses inkl. versteckter Dateien

