### Einführung, etc profile, etc bashrc
  * 2 Konfigurationsdateien für Shell
    * login Shell wird gestartet zb über ssh (**/etc/profile** und **/etc/bash.bashrc**)
    * nicht-login Shell wird lokal genutzt (**/etc/bash.bashrc**)
  * Code der immer ausgeführt werden soll wird dann in bash.bashrc geschrieben
  * Hinweis CentOs
    * Konfigurationsdatei heisst hier **/etc/bashrc**

### Die Dateien bash_login und bash_logout
  * beim Start einer login Shell wird **/etc/profile/** geladen
    * überprüft ob **~/.bash_profile/** existiert
      * wenn diese Datei vorhanden ist wird diese geladen. Falls nicht:
    * überprüft ob **~/.bash_login/** existiert
      * wenn diese Datei vorhanden ist wird diese geladen. Falls nicht:
    * überprüft ob **~/.profile/** existiert
      * wenn Datei vorhanden wird diese geladen
  * beim logout
    * **~/.bash_logout/** wird geladen

### Die Variable PS1
  * **~/.bashrc**
    * Hier befindet sich die Variable PS1
    * Unter CentOS: **/etc/bashrc/**
  * Variable PS1
    * sorgt dafür das in der Shell in jeder Zeile der User@system und der aktuelle Dateipfad stehen.
  * man bash
    * ruft das Manual von bash auf
    * unter Promtping stehen die Erklärung für die Parameter die zb für PS1 genutzt werden können
      * \u gibt den User aus
      * \h gibt den Hostname aus
      * \W gibt den aktuellen Pfad aus
      * \$ gibt eine # aus wenn man als root eingeloggt ist oder ein $ wenn man nicht als root eingeloggt ist.

### Farben in Shell, PS1 mit Farben
  * paket colortest
    * sudo apt-get install colortest
    * Paket gibt es nicht für CentOS, Farbcodes sind aber die selben
  * colortest-16b 
    * gibt die gesamten Fabcodes aus die zur verfügung stehen

### Die Variablen PS2, PS3, PS4
  * Variable PS2
    * Gibt an was am Anfang einer fortlaufenden Zeile steht.
    * Standardwert: >
  * Variable PS3
    * Gibt den Wert für den select Befehl an. (shell-script)
  * Variable PS4
    * wird mit set-x genutzt (aktiviert den Entwicklungsmodus)
      * kann mit set+x entfernt werden
    * gibt den Wert an der am Anfang einer Zeile steht wenn die Debug Informationen angezeigt werden
    * Standardwert: + 

### Aliase hinzufügen und entfernen
  * Aliase werden genutzt um lange Befehle zu verkürzen (shortkey)
    * z.B. statt date +“%d.%M.%Y %H:%I“ nur noch d
    * alias d=“date +‘%d.%M.%Y %H:%I‘“
  * unalias d
    * löst den alias d = date wieder auf
  * alias
    * Gibt alle alias in der Shell aus
  * Es können mehrere Befehle in einem Alias stehen
  * Wenn das alias auch nach einem Neustart vorhanden bleiben soll,
muss der Befehl in die ~/.bashrc geschrieben werden. (am besten ans Ende der Datei)
    * alias d=“date +‘%d.%M.%Y %H:%I‘“
  * Achtung
    * Alias nicht gleich wie ein Programm/Befehl nennen,
da diese sonst nicht mehr genutzt werden können  

### Die inputrc-Datei
  * Inputrc-Datei
    * Konfiguration für die eingaben
    * /etc/inputrc
    * Definition von Tastenkombinationen
  * Eignen code in /etc/.inputrc (Ubuntu)
    * $include /etc/inputrc an den Anfang der Datei schreiben
  * Control-S: ”ls -al“
    * nach hinzufügen des Codes muss die Datei neu eingebunden werden
      * bind -f ~/.inoutrc
    * durch drücken von strg+s wird dann ls -al ausgeführte

