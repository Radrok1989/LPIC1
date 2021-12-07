### uniq und sort
  * uniq
    * Enfernt alle Einträge aus (z.B einer Liste) die doppelt hintereinander vorkommen. Es wird sich nur die letze Zeile gemerkt
    * `cat names.txt | uniq`
      * Gibt die Datei name.txt aus ohne sich wiederholende Zeilen
  * sort
    * sortiert alle Einträge nach dem Alphabet
    * `cat names.txt | sort`
      * Gibt die Datei names.txt sortiert aus
  * `cat names.txt | sort | uniq`
    * gibt die Datei names.txt sortiert aus und entfernt alle doppelten Zeilen
    * Da alle Zeilen vorher sortiert werden Filtert uniq diese auch vollständig heraus

### Spalten auswählen - cut
  * `head ./nasa-logs.tsv`
    * gibt die ersten Zeilen von der Datei aus
  * `tail ./nasa-logs.tsv`
    * gibt die letzten Zeilen von der Datei aus
  * cut
    * gibt Ausgewählte Teile einer Zeile von einer Datei aus
  * `cut --help`
    * gibt die Hilfe von cut aus
  * `cut -f5 nasalogs.tsv | head`
    * gibt von der Datei nur die 5te Spalte aus
    * das head bewirkt das nicht die komplette Datei ausgegeben wird sondern nur die ersten Zeilen
  * `cut -f3,5 nasalogs.tsv | head`
    * gibt von der Datei die 3te und 5te Spalte aus

### wc - word count
  * `wc nasalogs-tsv`
    * gibt 3 Zahlen aus. 1. Anzahl der Zeilen, 2. - Wörter, 3. - Zeichen
  * `wc -l nasalogs.tsv`
    * gibt nur die Anzahl der Zeilen aus
  * `wc -c nasalogs.tsv`
    * gibt nur die Anzahl der  bytes aus

### nl - gibt Zeilennummern aus
  * `nl test.ccp`
    * Gibt die Datei vollständig mit Zeilennummern aus
  * `nl --help`
    * gibt die Hilfe des Programms aus.
  * `nl -b a test.ccp`
    * Gibt die Datei vollständig mit Zeilennummern aus (Leere Zeichen werden nicht gezählt)

### od - Kann Dateien in Oktal oder Hexadezimal umwandeln
  * od namen.txt
    * gibt die Datei in Oktal-Schreibweise aus
  * od --help
    * gibt die Hilfe der Datei aus
  * od -t x1 namen.txt
    * gibt die Datei in Hexadezimal aus

### Zeichen ersetzen mit tr
  * echo ”Hallo Welt“ | tr H h
    * ersetzt in der Eingabe alle großen H durch h
  * echo ”Hallo Welt“ | tr HW wh 
    * Ausgabe: wallo helt
    * Zuerst die zu ersetzenden Symbole danach wodurch diese ersetzt werden sollen
  * echo“hallo Welt“ | tr A-Z a-z
    * ersetzt alle Großbuchstaben durch Kleinbuchstaben (Keine Umlaute)
  * echo ”Hallo Welt“ | tr [:upper:] [:lower:]
    * ersetzt alle Großbuchstaben durch Kleinbuchstaben (Keine Umlaute)
  * echo ”Hallo Welt“ | tr -d [:blank:]
    * entfernt alle Leerzeichen
  * echo ”Hallo Welt“ | tr -s [:blank:]
    * entfernt alle sich wiederholende Leerzeichen

### der Stream Editor
  * sed
    * Es können hier auch reguläre Ausdrücke verwendet werden.
  * `echo "Hallo Welt..." | sed -E 's/\.\.\./!!!/'`
    * ersetzt alle Punkte durch ausrufezeichen. Mehr zu sed findest du im Kapitel 'Objekte im Linuxsystem finden'

### expand und unexpand
  * expand 
    * Programm um u bestimmen wie bei Ausgaben Tabulatoren in Leerzeichen umgewandelt werden sollen
  * `expand --help`
    * gibt die Hilfe von expand aus
  * `head -n 10 nasa-logs.tsv | expand --tabs=20`
    * gibt die ersten 10 Zeilen von der Datei aus und wandelt Tabulatoren in 20 Leerzeichen um
  * unexpand
    * Programm um Leerzeichen wieder in Tabulatoren umzuwandeln
    * benutzt die Selben Parameter nur das keine Zeichen hinzugefügt werden.

### Texte formatieren - fmt und pr
  * fmt
    * Programm um Text zu formatieren (Datei oder Eingabe)
  * `fmt lorem.txt`
    * gibt die Datei formatiert aus
  * `fmt --help`
    * ruft die Hilfe des Programms auf
  * `fmt --wiidth=100 lorem.txt`
    * Gibt die Datei in 100 Zeichen pro Zeile aus
  * pr 
    * generiert eine Druck-Version einer Datei
  * `pr --help`
    * ruft die Hilfe des Programms auf

### der Paste-Befehl
  * paste
    * Programm um Dateien zusammen zu fügen
  * `paste namen.txt telefonnummern.txt`
    * Gibt eine Lister der beiden zusammengefügten Dateienn aus
  * `paste --help`
    * gibt die Hilfe des Programms aus

### Split und Prüfsummen
  * split
    * Programm um Dateien aufzuteilen
  * `split --verbbose nasa-logs.txt`
    * Teilt die Datei in mehrere Dateien auf mit jeweils 1000 Zeilen pro Datei
    * Jede erstellte Datei beginnt mit x
  * md5sum
    * erstellt eine md5sum einer Datei
    * 2 Identische Dateien haben die selbe Prüfsumme
    * In sehr seltenen Fällen sind die Prüfsummen gleich obwohl die Datei unterschiedlich ist.
  * Prüfsummen werden genutzt um zu überprüfen ob Dateien vollständig oder unverändert sind
  * sha256sum
    * Prüfsumme mit anderem Algorithmus

### Mit komprimierten Datenn arbeiten (bzcat, bzip2)
  * bzip2 nasa-logs.tsv
    * komprimiert die Datei bzip
    * erstellt die Datei nasa-logs.tsv.bz2
  * bzcat nasa-logs.tsv.bz2
    * dekomprimiert die dAtei und gibt diese aus
    * man muss die Datei nicht nicht vorher entpacken
