## Shell-skripting

  * Du kannst mehrere Befehle gbündelt in einer Kommandozeile ausführen, indem du sie per Semikolon abtrennst.  
   `~$ Befehl; Befehl; Befehl`
<br><br>
  * Man kann Bash-Befehle auch gebünelt in einer Datei abspeichern, um sogenannte Shell-Skripte zu erstellen, di kompakt komplexe Logik ausdrücken (zB. Backup einer Datenbank erstellen und auf einem FTP-Server laden) und direkt in der Shell ausgeführt werden.
  <br><br>
  * Üblicherweise versieht man Shell-Skripte mit der **Endung .sh**, diese ist aber prinzipiell nicht notwendig.
  <br><br>
  * Mit der **Shebang**-Zeile gibst du an, mit welchem Programm deine Skripts ausgeführt werden sollen; bei einem Bash-Skript schreibst du dazu in die erste Zeile: `#!/bin/bash`
  <br><br>
  * Ohne explizite Shebang-Zeile werden Programme standardmässig als Bash-Skripte ausgeführt.
  <br><br>
  * Mit der Shebang-Zeile kannst du auch Skripte in anderen Programmiersprachen universell ausfürbar machen; für Python-Programme benutzt du etwa als Shebang-Zeile (insbesondere kannst du auch auf die Dateiendung .py vezichten): `#!/bin/python3`
  <br><br>
  * Wenn du Skripte per `./Skriptname` oder `bash Skriptname` ausführst, wird eine neue Bash-Shell gestartet, in der das Skript ausgeführt wird, Variablen aus dem Skript sind dann **nicht** im Rahmen der aktuellen Shell angelegt; wenn du hingegen ein Skript via `. Skriptname`innerhalb der aktuellen Shell ausführst, dann kannst du im Terminal auch die Variabblen daraus abrufen.
<br><br>
  * Um bei der Ausführung eines Skripts **Parameter** zu übergeben, schreibst du diese einfach hinter den Skritnamen: `./Skriptname.sh Parameter1 Parameter2 Parameter3`
<br><br>
  * Damit bei einer Ausgabe der Inhalt einer Variable eingesetzt werden, muss der Variablennamen innerhalb **doppelter Anführungszeichhen** stehen, bei Variablen in **einfachen Ausführungszeichen** wird nur der Name eingesetzt, ohne jegliches Anführungszeichen wird der Inhalt aufgelöst ( also zB die Tilde ~ als Pfad des Homeverzeichnisses) 
<br><br>
  * Als nützliches Pattern empfiehlt es sich, **lokale Variablen**, die du nur im Shell-Skript verwendest, klein zu schreiben, um zu verhindern, dass du versehentlich **globale Varablen** (die GROSS geschrieben werden) abänderst); überdies kannst du Variablen als `readonly` markieren.
<br><br>
  * Du kannst mit dem Raute-zeichen # ganze Zeilen in einem Bash-Skript **auskommentieren** (die Zeilen werden bei der Ausführung nicht beachtet; gilt natürlich nicht für die Shebang-Zeile)
<br><br>
  * Mittels **Befehlssubstitution** kannst du die Ausgabe eines Befehls als Wert einer Variablen setzten: `Variablenname="$(Befehl)"`
<br><br>
  * Mit **if-Anweisungen** kannst du Logik-Abfragen implementiert:
    * du beginnst eine if-Anweisung mit if und beendest sie mit **fi**
    * Teil einer if-Anweisung ist die von der Shell mitgelieferte **Testfunktion**, die per geöffneter eckiger Klammer [ aufgerufen wird und logische Ausdrücke (Vergleiche wie zB. =,!=,<,...) ausgewertet.
    * du musst zwingend **Leerzeichen** zwischen den eckigen Klammern und der enthaltenen Bedingung setzen, da jene als Parameter beim Aufruf der Testfunktion [ übergeben wird
    * auch im logischen Ausdruck musst du auf **Leerzeichen** zwischen Variable, Operation und Wert achten
    * indem du den Code mit dem Tabulator **Tab** einrückst stellst du ihn übersichtlich dar:
![if-einrücken](bilder/if-tab.png)
  * Du kannst auch mehrere Vergleiche miteinander verbinden, d.h. zwei Shell-Befehle miteinander verketten
    * **and - Operator** (liefert 0, wenn **beide** Bedingungenn erfüllt sind, sonst  1):
    ![and-operator](bilder/and-operator.jpg)
    * **or - Operator** liefert 0, sobald **mindestens eine** der Bedingungen erfüllt sind, sonst 1):
    ![or-operator](bilder/or-operator.jpg)
  * if-Angweisungen können mithilfe von **else** und **elif** erweitert werden, um auch die Fälle abzufangen, in denen die if-Bedingungen nicht erfüllt ist und darauf mit alternativen Befehlen zu reagieren.  
    ![elif-p1](bilder/elif-pt1.jpg)
    ![elif-p2](bilder/elif-pt2.jpg)
    <br><br>
  * Mittels Schleifen kannst du einen Block an Befehlen wiederholt ausführen lassen (in einem Bash-Skript wau auch im Terminal): 
    * Bei der **for-Schleife** wird die Anzahl der Schleifendurchläufe durch die Angabe eines Zahlenbereichs ({`ErsteZahl...LetzteZahl`}) im Vorfeld festgelegt:  
![for-schleife](bilder/for-Schleife.png)
    * Bei der **while-Schleife hingegen wird mit einem Test (wie bei einer if-Anweisung) nur eine Abbruchbedingung festgelegt:  
![while-Schleife](bilder/while-Schleife.png)
    * Daher besteht bei while-Schleifen die Gefahr von Endlosschleifen, also Schleifen, die nicht mehr verlassen werden und das Programm an dieser Stelle "einfrieren":  
![while-True-Schleife](bilder/while-true-schleife.png)
    * Im Allgemeinen ist deshalb eine for-Schleife einer while-Schleife vorzuziehen.
