### mehrere Befehle kombinieren
  * Befehle können mit einem Semikolon aneinandergehangen werden
    * `ls; ls`
    * Falls das erste Programm einer Fehler wirft, wird das zweite trotzdem ausgeführt
    * unabhängige Befehle
  * Befehle können auch mit && verknüpft werden
    * `ls && ls`
    * Falls der erste Befehl einen Fehler wirft wird der zweite Befehl nicht ausgeführt
    * verkettete Befehle
  * Befehle können mit einem || verknüpft werden 
    * `ls || ls` 
    * Nur wenn der erste Befehl einen Fehler wirft, wird der zweite Befehl ausgeführt
    * Oder Befehle

### Programme im Hintergrund laufen lassen
  * `wget https://Linkzumdownload.ch/test.zip &`
    * Durch das & am Ende wird das Programm im Hintergrund ausgeführt und man kann die Shell weiterhin nutzen.
    * Ausgabe des Programms wird in ein log umgeleitet welches automatisch erstellt wird.
  * `jobs`
    * zeigt die momentan aktiven Befehle
  * `fg` - foreground
    * brint das zuletzt verwendete Programm wieder in den Vordergrund
  * Crtl+Z
    * Bringt das Programm wieder in den Hintergrund, wird jedoch angehalten
  * `bg` - background
    * das Programm wirdd  wieder ausgeführt
  * `ps`
    * zeigt aktive Prozesse im System
  * `kill prozdessid`
    * Beendet das Programm mit der ProzessID
  * `kill -9 prozessid`
    * Beendet (killt) das Programm sofort
  * `nohup wget https://Linkzumdownload.de/test.zip &`
   * Programm läuft im Hintergrund weiter selbst wenn die Shell geschlossen wird.

### Terminal mit  anderen Leuten teilen, der Screen-Befehl
  * `sudo apt-get install screen`
    * Installiert das Programm Screen
  * `screen`
    * startet das Programm
  * `screen -x` 
    * verbindet sich mit der aktiven screen instanz
    * auch von einem anderen Standort aus
    * Mehrere Leute können gleichzeitig an einer Shell arbeiten (sehen die selbe Shell)
  * Ctrl+A+F
    * Passt die Shell an die Grösse des Bildschirms an
  * exit
    * beendet den screen für alle Leute
  * Ctrl+A+D
    * screen wird verlassen läuft aber im Hintergrun weiter
  * screen -x -r ID
    * Falls mehrere screens aktiv sindd muss beim aufrufen die entsprechende ID übergeben werden

### Befehlssubstitution
  * currentDate=$(date+"%Y-%m-%d")
    * schreibt den Befehl date in die Variabble currentDate
  * cp hallo.txt hallo.txt.$currentDate
    * kopiert hallo.txt und erstellt die Datei hallo.txt.2021-08-25
      * Befehl in der Variable wird ausgeführt und die ausgabe in denn Namen der neuen Datei geschrieben
  * contents=$(cat hallo.txt)
    * echo "$contetns"
      * gibt den Inhalt der Datei hallo.txt aus

### Variablensubstitution
  * echo "{d}test"
    * gibt die Variable d aus und dahinter test
  * echo  "{d:-heute}test"
    * überprüft ob die Bariable d existiert
      * Falls ja wird Variable d + test ausgegebben
      * Falls nein wird heute + test ausgegeben
  * echo "{e:=heute}test"
    * überprüft ob Variable e existiert
      * Falls ja wird Variable e + test ausgegeben
      * Falls nein wird heute in die Variable e geschrieben und dann + test ausgegeben
  * echo "{g:?Variable g existiert nicht}..."
    * überprüft ob Variable g existiert
      * Falls ja wird alles ausgegeben
      * Falls nein wird der String hinter dem Fragezeichen ausgegeben
      * und der restliche Befehl abgebrochen

