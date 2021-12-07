### Nano
  * Ist ein textbasierter Editor, kann also auf de Kommandozeile gestartet werden
  * Standard-Editor von Ubuntu und den meisten Debian Distributionen
  * Unter CentOS kann nano mit Hilfe von `yum install nano` nachinstalliert werden
  * Der Befehl `nano` im Terminal öffnet den Editor mit leerem Inhalt
  * Der Befehl `nano` gefolt von einer Datei öffnet diese. Existiert sie nicht, wird sie erstellt
  * *Kopieren, Ausschneiden* und *Einfügen* sind über das Kontextmenü möglich
  * Tastenkombinationen sind nicht wie gewohnt belegt
  * Über die Datei **/etc/nanorc** können global Einstellungen zu Nano angepasst werden (erweiterte Rechte zur Bearbeitung erforderlich)
  * Wird eine Datei **~/.nanorc** im Userverzeichnis erstellt, können dort Einstellungen für den entsprechenden Benutzer vorgenommen werden die sich nicht global auswirken
  * Sieh auch Befehlsübersicht von nano als Zusatzmaterial

### Vi und vim
  * Ist einer der ältesten, heute noch verwendeten Texteditoren ffür Linux und Unix
  * Der originale `vi`ist nicht Open Source, so dass es auf dessen Basis diverse Klone und Weiterentwicklungen gibt
  * Die unter Linux am häufigsten eingesetzte Variante ist `vim` - das steht für Vi improved
  * `vim` ist Open Source und bietet zahlreiche Zusatzfunktionen bis hin zu grafischen Versionenn und einer eigenen Skriptsprache
  * Bei Aufruf von `vi` wird meistens `vim` gestartet (bei Ubuntu CentOS)
  * Es existieren diverse Varianten, mindestens eine Minimalversion ist oft auf jeder Distribution vorhanden und ist daher in Sachen Editor meist "der kleinste gemeinsame Nenner"
  * Es gibt 3 Modi
    * Befehlsmodus (Normalmodus), hier wird innerhalb der Datei navigiert
    * Einfügemodus, hier wird der Inhalt editieren
    * Kommandozeilen-Modus, Funktionen wie Speichern und/oder Schliessen
  * Bessere Bedienung des Editors wenn compatible mode zu `vi` mit `:set nocp `deaktiviert wurde
  * Unterstützt in der Standardeinstellungen reguläre Ausdrücke innerhalb der Suche
  * Die Konfigurationsdatei für `vim` kann wie bei `nano` im Homeverzeichnis unter dem Namen `~.vimrc` erstellt werden
  * Mit `apt install vim-gnome` kann eine grafische Version vom `vim` installiert werden. Diese wird mit `gvim` gestartet
  * Siehe auch Befehlsübersicht von `vim` als Zusatzmaterial

### Emacs
  * Wird vom GNU-Projekt bereitgestellt
  * Meistens muss Emacs mit Hilfe von `yum install emacs` (CentOS) bzw. `apt install emac` (Ubuntu) inkl. aller Abhängigkeiten nachinstalliert werden
  * Nach der Eingabe von emacs startet in der Standardeinstellung die GTK-Variante (https://www.gtk.org) mit grafischer Oberfläche
  * Die Bedienung erfolgt mit der Steuerungstaste (Strg) und der Metataste (Alt) in Kombination mitt einem Buchstaben
  * Siehe auch Befehlsübersicht von Emacs als Zusatzmaterial

### Syntax Highlighting
  * Eine Funktion von Texteditoren die das Hervorheben von Code-Elementen ermöglicht
  * Nano:
    * Die Definitionsdateien zum Syntaxhighlighting für Nano liegen unter /usr/share/nano
    * Über die Datei /etc/nanorc werden diese eingebunden
    * Aktiviert/deaktiviert wird das Syntax Highlighting über die Tastenkombination Alt+Y innerhalb von nano
  * Vim:
    * Aktiviert/deaktiviert wird das Syntax Highlighting durch den Befehl `:syntax on/off`
    * Über die Datei **/etc/vimrc** bzw. **~/.vimrc** werden die Definitionsdateien eingebunden
    * Die Farbschemas liegen unter **/usr/share/vim/vim74/colors**
    * Das Farbschema kann mit dem Befehl `:colorscheme <name>` geändert werden
  * Emacs:
    * Aktiviert/deaktiviert wird das Syntax Highlighting durch Alt+X gefolgt vom Befehl font-lock-mode

### Grafische Editoren
  * Auch unter Linux sind grafische Editoren verfügbar, zum Beispiel:
    * gedit (auf Ubuntu vorinstalliert)
    * Kate
    * notepad++ (lauffähig wie wine)

### Der Standard-Editor und das Alternativen-System
  * Das Alternativen-System legt bei mehreren Programmen mit gleicher Funktion fest. welches das Standard-Programm ist
  * Durch die eingabe von `editor` wird der standard-Editor gestartet
  * Der Standard-Editor wird automatisch gestartet, sobald eine Editorfunktion benötigt wird
  * Innerhalb des Programms `less` kann der Standard-Editor für die Bearbeitung einer Datei durch die Taste v aufgerufen werden
  * Der Standard-Editor kann auf einem Linux System konfiguriert werden
  * Der Filesystem Hierarchy Standard (FHS) legt fest, dass Konfigurationen ausschiesslich unter **/etc** erfolgen sollen
  * Die Datei **/usr/bin/editor** zeigt durch einen Symlink auf **/etc/alternatives/editor**. Von dort aus erfolgt dann die Verlinkung auf das dedizierte Standard-Programm
  * Der Befehl `update-alternatives --get-selections` zeigt alle Programmgruppen für die ein Standardeintrag vorhanden ist
  * Mit dem Befehl `update-alternatives --config editor` (als su ausführen) kann die Gruppe `editor` bearbeitet werden und damit der Standard-Editor eingestellt werden
  * Unter CentOS befinden sich die Verlinkungen auf entsprechnde Standard-Programme unter **/etc/alternatives**
  * In der Minimalversion von CentOS ist kein Standard-Editor definiert, daher funktioniert der Befehl `editor` auch nicht
  * Unter CentOS kann der Standard-Editor über die Dateienn **/etc/profile** (ganzes System) oder **~/.bash_profile (für entsprechenden benutzer) durch die Festlegung der Umgebungsvariable definiert werden. Folgende Zeile legt zum Beispiel Nano als Standard-Editor fest
    * export EDITOR="/bin/nano"
