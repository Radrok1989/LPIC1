### Mit Pfaden arbeiten
  * Der Befehl cd nimmt Pfadangaben als Parameter entgegen. Diese können wir absolut oder relativ angeben
    * **Absolute Pfade** - Starten mit einem / und beschhreiben den Pfad zum gewünschten Verzeichnis ausgehen vom obersten Punkt im Dateisystem
    * **Relative Pfade** - Beschreiben den Weg zum Ziel ausgehend vom aktuellen Standort
  * Die Variable $OLDPWD enthält das vorige Verzeichnis, indem man sich befunden hat
  * Um ein Programm auszuführen, haben wir zwei Möglichkeiten:
    * Wir geben den Absoluten Pfad vorneweg mit an
    * Wir schreiben ./ davor, das steht für das aktuelle Verzeichnis
  * Wenn wir Dateien öffnen wollen, zBsp. mit einem Editor, können wir direkt die Datei angeben, wenn sie sich im selben Verzeichnis befindet

### Verzeichnisse erstellen und löschen
  * Unter Ubuntu wird in der Datei **.profile** im Homeverzeichnis festgelegt, welche Pfade in der $PATH Variable aufgenommen werden
  * Unter CentOS wird in der Datei **.bash_profile** im Homeverzeichnis festgelegt welche Pfade in der $PATH Variable aufgenommen werden
  * Mit `mkdir` werden Verzeichnis erstellt, mit `rmdir` gelöscht (nur leere Verzeichnisse ohne Unterordner). Mit der Option `-p` auch mit Unterordner
  * Der Befehl `rm`löscht sowohl Dateien als auch Verzeichnisse. <span style="color:red">Vorsicht beim Einsatz dieses Befehls.</span>

### Verzeichnis-Listings verstehen
  * Ausgabe des Befehls `ls -l` (langes Format)
    * Jede Datei bzw. jedes Unterverzeichnis stehen in einer eigenen Zeile
    * Der Punkt steht für das aktuelle Verzeichnis und der doppelpunkt für das übergeordnete Verzeichnis
    * In der Spalte steht der Dateityp
      * `d` steht für Directory, also Verzeichhnis
      * `-` steht für normale Dateien
      * `l` wie Link, das sind Symbolische und Softlinks
      * `c` für Character Device, das sind zeichenorientierte Geräte (Modems)
      * `b` für Block Device, das sind blockorientierte Geräte (Speichermedien)
    * Ausführbare Dateien werden in grün dargestellt (anhand der Rechte)
    * Die Rechte werden entsprechend angezeigt
    * Die Anzahl der Links auf diesen Eintrag
    * Der Eigentümer des Eintrags und die Gruppe, der der Eintrag zugewiesen wurde
    * Die Grösse der Datei wird in Bytes angegeben
    * Datum der letzten Änderung
  * Der Befehl `ls -F` kann durch angehängte Zeichen kennzeichen, um welchen Art von Eintrag es sich handelt:
    * /Verzeichnisse
    * *ausführbare Dateien
    * @ symbolische Links
  * Der Befehl `ls` unterstützt sehr viele Optionen die flexibel miteinander kompiniert werden können. Siehe dazu Man-Page von `ls` und Befehlsübersicht dieses Kurses
  * Das Programm `tree`zeigt seh übersichtlich den Verzeichnsbaum ab einem bestimmten Punkt an

### Dateien erstellen, kopieren, verschieben und löschen
  * Es gibt drei gängige Möglichkeitenn eine Datei zu erstellen:
    * Datei anlegen mit `touch`
    * Mit einem Editor eine nicht existierende Datei öffnen und speichern
    * mit `echo` und dem Umleitungszeichen eine Datei erstellen und beschreiben
    * Mit dem Befehl `cp` (copy) können Dateien kopiert weren (auch unter anderem Namen) mit der Option `-r`sogar komplette Verzeichnisse
    * Mit dem Befehl `mv` (move) können Dateien verschoben und umbenannt werden. Vorsicht: Ist das Ziel kein Verzeichnis sondern eine existierende Datei, wird diese Datei mit dem Inhalt der Quelldatei überrschrieben

### Hardlinks und Softlinks
  * Ein Link ist ein Verweis auf einne andere Datei oder ein Verzeichnis
  * **Hardlinks** 
    * Es handelt sich um ein- und dieselbe Datei auf die mittels eines Inodes referenziert wird. Dies ist ein Verweis auf eine Datei, allerdings in der Dateisystem-Tabelle - quasi die ID der Datei
    * Harklinks können nur innerhalb einer Partition existieren
    * Hardlinks können nur auf Dateien angewendet werden, nicht auf Verzeichnisse
    * Wird ein Hardlink gelöscht, so besteht die Originaldatei noch weiter
  * **Softlinks / Symbolic Links, kurz: Symlinks**
    * Mit Symlinks können Verweise, also Links partitionsübergreifend erstellt werden
    * Das gilt gleichermassen für Dateien und Verzeichnisse
    * Sie sind deutlich einfach zu erkennen, da sie mit Pfaden arbeiten
    * Wird die Originaldatei gelöscht, existiert keine Kopie mehr

### Dateien archivieren und komprimieren
  * Um Daten und Dateien zu sichern oder bereit zu stellen, bietet es sich an, diese in einer Archivdatei als Ganzes einzupacken und zu komprimieren
  * Spart Platz auf dem Speichermedium bzw. Bandbreite bei der Übertragung
  * Das am häufigsten verwendette Programm zum Archivieren ist tar
  * Ein *Tarball* ist eine mit `tar` erstellte und mit `gzip` oder `bzip2` komprimierte Archivdatei
  * `Bzip2` (.bz2) und `gzip` (.gz) sind Kompressionsverfahren
  * `Bzip2` ist die neuere und effektivere Variante
  * Dateien mit der Endung `tar` sind unkomprimierte Archive die mit `tar -xf` entpackt werden können. Nach dem Entpacken bleibt die Archiv-datei weiterhin vorhanden
  * Archive beinhalten meistens mehrere Dateien und oft auch Verzeichnisse. Diese können mit `tar -tf` angezeigt werden.
  * Mehrere Dateien und Verzeichnisse können mit `tar -cf`zu einem Archiv zusammengefasst werden
  * Alternative zu `tar` ist `cpio`
  * Alternative zu `gzip` und `bzip2` ist `xz` 

### Zugriffsrechte auf Dateien und Verzeichnisse verstehen
  * Zugriffsrechte auf Dateien und Verzeichnisse werden mit `ls -l` dargestellt:  
![](bilder/Zugriffsrechte-auf-dateien.png)  
  * Das bedeutet die Zugriffsrechte:
  
  ![](bilder/Zugriffs.png)  
  * Zugriffsrechte setzen mit `chmod`:

![](bilder/Zugriffsreche-setzen.png)
  * User und Gruppen können mit `chown` gesetzt werden, der Befehl nimmt duch Doppelpunkt getrennt Benutzer und Gruppe an:
    * `chown <benutzer>:<gruppe> <datei>`
  * Die zweite Möglichkeit besteht darin, mit `chown` den User und mit `chgrp` die Gruppe festzulegen:
    * `chown <benutzer> <datei>`
    * `chgrp <gruppe> <datei>`
  * Zugriffsrechte setzen mit dder Octal-Methode:

![](bilder/Octal-Methode.png)

### Sonderrechte - SUID-Bit, SGID-Bit und Sticky-Bit
  * Wird bei den Benutzerrechten einer Datei ein **S** anstatt einem **X** angezeigt, handelt es sich um das Set UID-Bit oder Kurz: SUID-Bit. Es sorgt dafür, dass das Programm immer mit den Rechten des Dateibesitzers läuft
  * Das SUID-Bit können wir setzten mit `chmod u=rwxs <datei>` oder `chmod 4755 <datei>`
  * Das SGID-Bit sorgt bei einer Datei mit Ausführungsrechten dafür, dass sich immer im Kontext der Gruppe läuft. Bei einem Verzeichnis sorgt das Set GID-bit dafür, dass die für das Verzeichnis festgelegte Gruppe auf alle neu angelegten Unterverzeichnissen und Dateien vererbt wird.
  * Das SGID-Bit können wird setzenn mit `chmod g=rwxs <datei>`oder `chmod 2755 <datei>`
  * Das Sticky-Bit wird duch ein **t** anstatt des **x** für Others gesetzt, also ganz am Ende der Rechteliste und kommt z.B. beim /tmp-Verzeichnis zum Einsatz
  * Wird das Sticky-Bit auf einen Ordner angewendet - und das ist dereinzige Einsatzzweck - so könen darin erstellte Dateien und Verzeichnisse nur vom Dateibesitzer gelöscht oder umbenannt werden
  * Es wird über den buchstaben t gesetzt bzw. Octal über die 1 in der vierten, vorangestellten Ziffer

### Umask und die Standardrechte
  * Wird ein Verzeichnis erstellt, dann werden standardmässig bestimmte Rechte gesetzt:
    * **rwx** für den User
    * **rx** für die Gruppe
    * **rx** für Others
  * Wird eine Datei erstellt, dann werden standardmässig bestimmte Rechte gesetzt:
    * **rw** für den User
    * **r** für die Gruppe
    * **r** für Others
  * Die Standard-Rechte werden mit `umask` festgelegt
  * Da es sichh um eine Maske handelt, wird das angezeigt, was verdeckt wird, also nicht gesetzt ist:
    * Bei Verzeichnissen ziehen wir die `umask` von jeweils 7 ab, um die gesetzten Werte zu erhalten. Dabei wird die erste Stelle ignoriert, da sie Sonderrechte betrifft, die in der `umask` ohnehin nicht gesetzt werden
    * Bei Dateien ist die 6 der Ausgangswert
  * `umask`festlegen:
    * Bei CentOS, systemweit: **/etc/profile**, für Benutzer: **.bash_profile**
    * Bei Ubuntu, systemweit: **/etc/login.defs**, für Benutzer: **.profile**

### Nützliche Befehle
  * `cd` - Wechselt in das Homeverzeichnis des aktiven Benutzers
  * `cd ..` - Wechselt in das darüberliegende Verzeichnis
  * `cd -` - Wechselt in das vorige Verzeichnis ($OLDPWD)
  * `cd <verzeichnis>` - Wechselt in angegebenes Verzeichnis
  * `pwd` - Zeigt aktuelles Verzeichnis
  * `which <datei>` - Zeigt Verzeichnis gesuchter Datei
  * `mkdir <Verzeichnis>` - Erstellt ein neues Verzeichnis
  * `mkdir -p <Verzeichnis/Verzeichnis>` - Erstellt ein neues Verzeichnis inkl. Unterverzeichnis
  * `rmdir <Verzeichnis>` - Löscht angegebenes Verzeichnis, wenn es leer ist
  * `rmdir -p <verzeichnis/verzeichnis>` - Löscht angegebenes Verzeichnis inkl Unterverzeichnisse
  * `rm <verzeichnis/datei>` - Löscht einzelne  und Verzeichnisse
  * `rm -r <verzeichnis/datei>` - Löscht Dateien und Verzeichisse rekursiv (Achtung!)
  * `rm -i <verzeichnis/datei>`  - Löscht Dateien und Verzeichnisse mit Nachfrage
  * `ls -R <Verzeichnis>` - Zeigt alle Unterverzeichnisse des Verzeichnisses
  * `ls -l` - Zeigt Inhalt des Verzeichnisses im ausführlichen Format
  * `ls -a` - Zeigt Inhalt des Verzeinisses inkl. versteckter Dateien
  * `ls -F` - Zeigt Inhalt des Verzeichnisses inkl. Typendeklaration
  * `ls -t` - Zeigt Inhalt zeitlich sortiert nahh der letzten Änderung
  * `ls -r` - Zeigt Inhalt in umgekehrter Sortierung an
  * `ls -d` - Zeigt Verzeichnisnamen anstelle deren Inhalt
  * `ls -i` - Zeigt Verzeichnis inkl- Inodes (Verweise) an
  * `touch <datei>` - Erstellt eine Datei mit leerem Inhalt
  * `echo "Inhalt für Datei" > /Datei` - Dateie mittels echo und Umleitung erstellen und befüllen
  * `alias` - Zeigt aktive Alias an
  * `cp <Verzeichnis/Datei> <Verzeichnis>` - Kopiert Datei in Verzeichnis
  * `cp -r <Verzeichnis> <Verzeichnsi>` - Kopiert rekursiv auch Verzeichnisse
  * `mv <Verzeichnis/Datei> <Verzeichnis>` - Verschiebt Datei in verzeichnis
  * `mv -i <Verzeichnis/Datei> <Verzeichhnis>` Verschiebt Datei in Verzeichnis mit Nachfrage
  * `mv <Name_alt> <Name_neu>` - Umbenennen eines Verzeichnisses oder einer Datei
  * `ln <Datei> <Datei-hardlink>` - Erstellung eines Hardlinks
  * `ln -s <Datei> <Datei-softlink>` - Erstellung eines Softlinks
  * `gzip <Datei.tar>` - Datei komprimieren mit gzip
  * `gzip -d <Datei.tar.gz>` - Datei dekomprimieren mit gzip
  * `gunzip <Datei.tar.gz>` - Datei dekomprimieren mit gunzip
  * `bzip2 <Datei.tar>` - Datei komprimieren mit bzip2
  * `bzip -d <Datei.tar.bz2>` - Datei dekomprimieren mit bzip2
  * `bunzip <Datei.tar.bz2>` - Datei dekomprimieren mit bunzip
  * `tar -xf <Datei.tar>` - Entpacken eines Archivs
  * `tar -tf <Datei.tar>` - Zeigt Inhalte eines Archivs
  * `tar -cf <Datei.tar> <Datei_1> <Datei_2>` - Erstellt ein Archiv aus mehreren Dateien
  * `tar -cf <Datei.tar> <Verzeichnis>` - Erstellt ein Archiv rekursiv  aus einem Verzeichnis
  * `tar -czf <Datei.tar.gz> <Dateien>` - Erstellt ein Archiv und komprimiert mit gzip
  * `tar -cjf <Datei.tar.bz2> <Dateien>` - Erstellt ein Archiv und komprimiert mit bzip2
  * `tar -xzf <Datei.tar.gz>` - Entpackt ein Archiv und dekomprimiert mit gzip
  * `tar -xjf <Datei.tar.bz2>` - Entpackt ein Archiv und dekomprimiert mit bzip2
  * `chmod a=rwx <Datei>` - Setzt entsprechende Rechte für alle
  * `chmod o=rwx <Datei>` - Setzt entsprechende Rechte für others
  * `chmod g=rwx <Datei>` - Setzt entsprechende Rechte für group
  * `chmod u=rwx <Datei>` - Setzt entsprechende Rechte für user
  * `chmod o-w <Datei>` - Entfernt bei Others das Schreibrecht
  * `chown <Benutzer>:<Gruppe> <Datei>` - Eigentümer und Gruppe für Datei setzen
  * `chown <Benutzer> <Datei>` - Eigentümer für Datei setzen
  * `chgrp <Gruppe> <Datei>` - Gruppe für Datei setzen
  * `chmod 754 <Datei>` - Rechtevergabe mit der Octal-Methode 
  * `chmod -R 754 <Datei>` - Rekursive Rechtevergabe mit der octal-Methode
  * `chmod 640 <Verzeichnis>/*` - Rechtevergabe für alle Dateien in Verzeichnis
  * `chmod u=rwxs <Datei>` - Eine Variante um das SUID-Bit zu setzen
  * `chmod 2755 <Datei>` - Eine Variante um das GUID-Bit zu setzten
  * `chmod o=rwxt <Datei>` - Eine Variante umd das Sticky-Bit zu setzen
  * `umask -S` - Zeigt Standardrechte an
