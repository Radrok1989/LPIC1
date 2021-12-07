### Deine Laborumgebung mit VirtualBox
  * Nutze für dein Labor eine Virtualisierungslösung wie zum Beispiel VirtualBox, VMware Workstation bzw. –Player, Hyper-V, Xen
  * Dadurch Flexibilität und Unabhängigkeit vom Betriebssystem deines Arbeitscomputers
  * Kann schnell wieder zurückgesetzt werden und Fehlkonfigurationen haben keinen Einfluss auf den Arbeitscomputer

### Ubuntu und Gasterweiterungen installieren
  * Wird von der Firma Canonical entwickelt und bereitgestellt
  * Unterscheidet sich in den Editionen Server und Desktop
  * Long Term-Version (kurz: LTS für Long Term Support), ist ggf. etwas älter, erhält aber dafür 5 Jahre lang Updates
  * Speicherplatz Dynamisch alloziert bedeutet, dass dieser nicht sofort auf deiner physischen Festplatte für die VM belegt wird. Stattdessen wird der Platz nur soweit in Anspruch genommen, wie die virtuelle Festplatte tatsächlich benötigt
  * Die Netzwerkeinstellung Netzwerkbrücke bedeutet, dass die virtuelle Maschine direkt über die physische Schnittstelle des Hosts kommunizieren kann. Dadurch sieht es für andere Systeme im Netzwerk so aus, als wäre die VM ein ganz normales, physisches System im lokalen Netz und kann auch von außen angesprochen werden 
  * Für die Installation muss die Installations-DVD in Form eines ISO-Images in die virtuelle Maschine eingelegt werden, das geschieht über Einstellungen des Massenspeichers
  * Der  Logical Volume Manager dient dazu die Partitionierung dynamisch und flexibler zu gestalten
  * Durch die Installation der Gasterweiterung werden unter anderen Optimierungen die Möglichkeiten bei der Grafikauflösung erweitert
  * Zur Installation der Gasterweiterung muss die Datei VBoxLInuxAddtions.run ausgeführt werden
  * Ein grüner Eintrag nach Absetzen des List-Befehls (ls) zeigt an, dass die Datei ausführbar ist
  * Durch die Tabulator-Taste werden eindeutige Befehle im Terminal vervollständigt

### Einen Sicherungspunkt (Snapshot) erzeugen
  * Die VM wird damit im aktuellen Zustand gespeichert und kann jederzeit an diesen Punkt zurückgesetzt werden 
  * Sicherungspunkt bei ausgeschalteter VM erstellen damit der Arbeitsspeicher nicht unnötigerweise gesichert werden muss
  * Wir empfehlen einen Sicherungspunkt direkt nach der Installation zu erstellen

### CentOS - Red Hat Enterprise Linux - installieren
  * In den Einstellungen der VM unter System das Zeigergerät USB-Tablet wählen, da PS/2-Maus  von CentOS nicht korrekt erkannt wird 
  * Bei der Installation zunächst Voreinstellungen belassen und Minimalinstallation wählen
    * Dieser Ansatz ist für eine Server-Umgebung oftmals der Beste für eine umfassende Kontrolle, welche Komponenten auf dem System installiert und aktiv sind
    * Dies reduziert die Angriffsfläche (was nicht da ist, kann auch nicht angegriffen werden) und erhöht zudem die Performance, da unnötige Komponenten keine Ressourcen beanspruchen
    * Alles, was wir zusätzlich an Funktionalität benötigen, können wir ggf. nachinstallieren 
  * Die Paketsammlung Systemadministrationstools können gleich mitinstalliert werden
  * Root ist der Systemadministrator eines Unix- bzw. Linux-Systems
  * Das Passwort von Root bei der Installation festlegen und mit Bedacht wählen
  * Einen zusätzlichen Benutzer bei der Installation anlegen und ein Passwort vergeben 
  * Das System bootet ohne grafische Oberfläche und bietet uns einen textbasierten Login auf der Konsole an

### Lokale An- und Abmeldung am System
  * CentOS:
    * Anmeldung mit Benutzer Root oder selbst erstelltem Benutzer möglich
    * Der Prompt zeigt den angemeldeten Benutzer, den Namen des Systems und das aktuelle Verzeichnis 
    * Das Zeichen ~ wird Tilde genannt und symbolisiert das Homeverzeichnis des Benutzers 
    * Das Dollarzeichen $ am Ende des Prompts zeigt uns, dass wir mit einem normalen Benutzer ohne Administrator-Privilegien arbeiten
    * Die Raute # am Ende des Prompts zeigt uns, dass wir mit Root angemeldet sind
    * Möchten wir einen Befehl absetzen, der erhöhte Rechte benötigt, werden wir aufgefordert, das Root-Passwort einzugeben
    * Über Alt+F2 gelangen wir auf eine zweite Konsole und können eine neue Session eröffnen
    * Analog dazu kann man über Alt+F<1-6> auf die Sessions 1 bis 6 wechseln 
  * Ubuntu
    * Die grafische Oberfläche schlägt uns den selbst angelegten Benutzer zu Anmeldung vor
    * Der Anzeigename ist nicht gleich der Benutzername
    * Der Benutzer Root existiert auf einem Ubuntu-System per Default nicht 
    * Ubuntu bedient sich stattdessen eines Konzeptes namens sudo (siehe unten)
    * Eine Shell ist die Umgebung in einem Terminal

### Root und su - Arbeiten als Superuser
  * Linux ermöglicht es, im Rahmen einer Terminal-Session eine weitere Session zu erstellen
  * Damit temporär zu root werden, ohne die Original-Session zu verlassen 
  * Der Befehl hierzu heißt su und steht für Substitute User
  * Ohne weitere Optionen geht das System davon aus, dass wir nun zu root werden wollen und erwartet dessen Passwort
  * Wie wir am Prompt sehen können, sind wir danach root (#)
  * PWD zeigt uns, dass wir uns noch immer im Homeverzeichnis des vorherigen Benutzers befinden
  * Daher zeigt auch der Prompt nicht mehr die Tilde (~), also das Homeverzeichnis des aktiven Benutzers, sondern das aktuelle Verzeichnis an
  * Hinter den Kulissen wurde eine neue Shell gestartet, in der wir als Benutzer root arbeiten. Die Original-Shell ist aber nach wie vor vorhanden
  * Im Rahmen der Shell werden bestimmte Umgebungsparameter gesetzt, die für jeden Benutzer individuell sein können
  * Mit su - oder su -l erstellen wir eine neue  Login-Shell für den Benutzer root mit allen Umgebungsparametern 
  * Dazu gehört z.B. die PATH-Variable. Sie umfasst alle Verzeichnisse, die automatisch nach dem eingegebenen Befehl durchsucht werden. Damit muss ein Benutzer nicht mehr den kompletten Pfad zur ausführbaren Datei angeben

### Sudo - Wenn kein Root vorhanden ist
  * Da unter Ubuntu per Default keine Anmeldung als Benutzer root existiert, wird ein Konzept namens sudo verwendet um Systemadministrationsaufgaben durchzuführen
  * Hier wird das Prinzip von su abgewandelt und als su do, also su "machen" auf normale Benutzer angewendet 
  * Dadurch ist es möglich, bestimmte Administrationsaufgaben an normale Benutzer bzw. Benutzergruppen zu delegieren
  * Sudo greift in erster Linie auf eine Datei namens **/etc/sudoers** zu. Dort werden alle Benutzer mit ihren jeweiligen Privilegien aufgeführt, die erhöhte Rechte erhalten sollen 
  * Hier erfahren wir, dass dem Benutzer root, den es ja eigentlich gar nicht gibt, der Gruppe Admin und der Gruppe sudo, sämtliche Privilegen zugestanden werden 
  * Das Prozentzeichen vor dem Namen kennzeichnet eine Gruppe.
  * Das kann man ziemlich granular steuern und diverse Beschränkungen einführen
  * Unter dem Strich sind alle Mitglieder der Gruppen Admin und Sudo de facto Administratoren 
  * In der Datei **/etc/group** sind alle Gruppen des Systems und deren Benutzer aufgeführt

### Grundlegende Netzwerk-Konfigurationen - statisch und via DHCP
  * DHCP, das Dynamic Host Configuration Protocol vergibt eine dynamische IP-Konfiguration
  * LAN steht für Local Area Network und bezeichnet das lokale Netzwerk in deinem HomeOffice oder Büro-Standort
  * Die MAC-Adresse, wird unter Ubuntu Geräteadresse genannt und kennzeichnet die Adresse der Hardware
  * Default-Gateway, wird unter Ubuntu Vorgabestrecke genannt und bezeichnet den Router, über den wir ins Internet gelangen
  * Der DNS-Server, ist für die Namensauflösung der IP-Adressen zuständig
  * Die Subnetzmaske beschreibt den Netz- und den Hostanteil in der IP-Adresse
  * Ubuntu:
    * Eine Komponente namens Network-Manager steuert die Netzwerk-Schnittstellenverwaltung. Sie ist komplett grafisch orientiert 
    * Es ist auch möglich die Konfiguration auf der Kommandozeile vorzunehmen
    * Über den Multifunktionsbutton oben rechts gelangst du in die Schnittstellenkonfiguration. Hier wählen wir LAN-Einstellungen um die IP-Parameter anzupassen 
    * Erst, wenn wir das Interface deaktivieren und erneut aktivieren, sehen wir die neuen Einstellungen 
  * CentOS  
    * Im Verzeichnis **/etc/sysconfig/network-scripts** befinden sich die Netzwerk-bezogenen Einstellungen in Form von Konfigurationsdateien und Skripts
    * Mit einem Texteditor können wir die Konfigurationsdateien entsprechend anpassen
    * Per Default steht nur vi bzw. vim, also verbesserte Version des vi-Editors zur Verfügung (Bedienung siehe Befehlsübersicht) 
    * Die relevante Konfiguration befindet sich in **ifcfg-enp0s3** 
      * Vor Änderungen mit copy eine Sicherungskopie erstellen
      * bootproto=dhcp stellt das Interface auf DHCP ein 
      * bootproto=static für eine statische IP-Konfiguration
        * IPADDR=, NETMASK= und GATEWAY= festlegen
      * onboot=no verhindert, dass das Interface beim Start eine DHCP-Anfrage stellt
      * Raute stellt Kommentare dar, so können Zeilen auskommentiert werden
    * Mit service network restart initialisieren wir die Netzwerk-Schnittstelle neu. Dabei wird die veränderte Konfiguration eingelesen 
    * In der Datei /etc/resolv.conf werden DNS-Server festgelegt

### Zugang zum System via SSH
  * Der Remote-Zugang ist einer der ersten Konfigurationsschritte im Rahmen der Installation eines Servers
  * Dazu nutzen wir meistens SSH, die Secure Shell
  * In CentOS ist bereits in der Minimal-Version ein SSH-Serverdienst integriert, siehe Prozess-Eintrag **/usr/sbin/sshd**
  * -D steht für Daemon, also gestartet als Dienst
  *  Mit einem SSH-Client kann eine entsprechende Verbindung aufgebaut werden
  *  Unter Windows kann beispielsweise PuTTY verwendet werden
  *  Dazu IP-Adresse der Ziels auf welches die Verbindung aufgebaut werden soll in PuTTY eingeben und Verbindung öffnen (Connection Typ = SSH)
  *  Der SSH-Schlüssel des Servers ist zunächst noch nicht vertrauenswürdig ist. Wir bestätigen dies und gelangen somit zur Anmeldung 
  *  Während in vielen Default-Einstellungen zu SSH keine Root-Anmeldung via SSH erlaubt ist, können wir uns unter CentOS auch ganz regulär mit root anmelden, ansonsten mit einem normalen Benutzer und dann eben mit su zu Root werden
  *  Die SSH-Session verhält sich weitgehen optisch und inhaltlich wie eine normale Konsolen-Session, allerdings können wir in den PuTTY-Einstellungen die Darstellung granular ändern
  *  Die SSH-Session dann durch Eingabe von Exit oder schlicht durch das Schließen des Fensters beendet werden

### Der Ubuntu-Desktop - Ein erster Rundgang
  * Ubuntu nutzte zunächst GNOME und dann viele Jahre eine eigene Standardoberfläche namens Unity
  * Seit Ubuntu 17.10 ist Canonical jedoch wieder zum GNOME-Desktop zurückgekehrt
  * Oben rechts befindet sich das Systemmenü:
    * Hier können Netzwerk- und Benutzereinstellungen vorgenommen werden
    * System ausschalten, neu starten und Bildschirm sperren 
    * Hier gelangt man auch zu den Einstellungen
  * Der persönliche Ordner des aktiven Benutzers ist per Default auf dem Desktop dargestellt: 
    * Wenn wir dies öffnen, dann öffnet sich die Ansicht des Dateimanagers, Nautilus
    * Ubuntu nutzt ein ähnliches Ordner-Schema wie Windows. Die meisten Funktionen des Windows-Explorers sind ebenfalls vorhanden
    * Rechtsklick auf das Objekt öffnet das Kontextmenü (Ausschneiden, kopieren, löschen). Auch die Eigenschaften des Objektes lassen sich so betrachten und manipulieren, insbesondere die Zugriffsrechte 
    * Drag & Drop wird unter Nautilus unterstützt
  * Die Favoritenleiste links enthält die wichtigsten Programme: 
    * Ist eines dieser Programme geöffnet, wird ein kleiner roter Punkt daneben angezeigt
    * Per Rechtsklick kannst du einen Favoriten entfernen
    * Die Favoritenleiste zeigt zudem unten generell die geöffneten Programme an
  * Oben im Panel wird das Kontextmenü des gerade aktiven Programms angezeigt. Sein Inhalt variiert von Programm zu Programm 
  * Über Anwendungen anzeigen erhältst du Zugriff auf alle installierten Anwendungen:
    * Du kannst über das Suchfenster explizit nach Anwendungen suchen, die wir entweder direkt starten oder aber über Rechtsklick den Favoriten hinzufügen können
    * Nur die Anwendungen die von Ubuntu für den Desktop registriert sind werden hier angezeigt. Dein Ubuntu-System hat noch hunderte weitere Linux-Programme und –Tools, die du nur über den direkten Aufruf via Terminal starten kannst
  * Der Button Aktivitäten oben links zeigt die aktuell geöffneten Programme
    * Ermöglicht den direkten Wechsel zum gewünschten Programm 
    * Es ist auch ein Wechsel zwischen verschiedenen Desktops möglich

Links:  
[Virtualbox](www.virtualbox.org)  
[Ubuntu/download](www.ubuntu.com/download)  
[CentOs](www.centos.org)  
[PuTTY](www.putty.org)


### Nützliche Befehle
`ls` - zeigt den Inhalt des aktuellen Verzeichnis  
`./` - Steht für das aktuelle Verzeichnis  
`sudo ls` - Führt ls als Superuser aus  
`pwd` - Zeicht Verzeichnis an, in welchem man sich befindet  
`hostnamectl set-hostname <Name.Domain>` - Ändert den Namen des Systems  
`hostname` - Hostname inkl. Domain wird angezeigt  
`hostname -s` - Nur Hostname wird angezeigt  
`hostname -d` - Nur Domain wird angezeigt   
`who` - Zeigt alle angemeldeten Benutzer inkl. deren Session  
`exit` - Beendet das Terminal  
`logout` - Beendet die Login-Session  
`shutdown -h now` - Sofortiges Herunterfahren des Systems  
`shutdown -r now` - Sofortiger Neustart des Systems  
`date` - Zeigt aktuelle Systemzeit an  
`date <MMDDHHYY>` Setzt die Systemzeit auf den angegebenen Wert  
`su <Benutzer>` - Wechselt Identität auf angegebenen Benutzer   
`su` - Wechselt identität auf root  
`su -` - Öffnet eine root-Shell  
`echo $PATH` - Zeigt den Inhalt der PATH-Variable  
`ps -ef` - Zeigt ausführliche liste aller laufenden Prozesse  
`cat <Datei>` - Zeigt u.a. Inhalt von Dateien im Terminal an  
`groups` - Zeigt verfügbare Gruppen  
`ip addr show` - Zeigt die IP-Konfiguration der Schnittstellen an  
`service network restart` - Neuinitialisieren der Netzwerk-Schnittstelle
`Cp <Datei> <Datei>` - Kopiert eine Datei
`vi <Datei>` - Öffnet angegebene Datei mit dem vi-Editor
  * Befehle im vi-Editor
    * `:wq` - Speichern und Schliessen  
    * `:q!` - Schliessen ohne Speichern  
    * `i` - Wechselt in den Insert-Modus
    * `a` - Zeilen anhängen