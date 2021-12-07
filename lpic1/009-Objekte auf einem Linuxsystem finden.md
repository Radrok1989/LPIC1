Ubuntu = (u)    
  * ## find - zum durchsuchen geeignet  
    * find . - listet alles in diesem und allen Unterordnern auf
    * find . -name "*.js" - listet von diesem und allen Unterordnern alle Dateien auf die auf ".js" enden.
    * find . -size +1M - listet von diesem und allen Unterordnern alle Dateienn auf die grösser als 1 MB sind
    * find . -size -1M -and -name "*spotify* kombiniert die beiden Parameter
    * find . -name "*JPG*" -delete lösche ab hier alle Dateien die JPG im Namen haben 
    * find . -iname "*JPG*" -delete lösche ab hier alle Dateien die JPG im Namen haben und es wird gross/kleinschreibung ignoriert

  * ## grep    
    * grep - Dateien durchsuchen
      * grep "money" * - Zeigt in den Dateien alle Zeilen mit dem Wort money
      * grep --count "money" - Zeigt wie oft das Wort money jeweils in en Dateien vorkommt. 
      * -i - ignoriert wieder gross/kleinschreibung

  * ## sed - Stream-Editor (u)  
    * **sed 's/Welt/Linux/' hallo.txt** - sucht im File hallo.txt alle Worte "Welt" und ersetzt sie durch Linux aber nur das erste "Welt" pro Zeile und gibt diese dann aus Das "s" steht für substitute und heisst ersetzten. Die Single Quotes müssen gesetzt werden.
    * **sed 's/Welt/Linux/g' hallo.txt** - tut fast das Selbe. Das g steht aber für Global und das bedeutet es ersetzt alle Worte "Welt" und nicht nur das erste pro Zeile und gibt diese dann aus. 
    * **sed -i 's/Welt/Linux/2' hallo.txt** - tut auch wieder fast das Selbe ausser, dass es dieses mal nicht ausgegeben wird sondern effektiv in dem File überschrieben wird. Das 2 steht dafür das nur das 2te vorkommen pro Zeile überschrieben wird.
    * **sed '3d' hallo.txt löscht die dritte Zeile aus hallo.txt
    * mit **sed -E** kann man die volle Bandbreite der Regular Expressions (Regex) freischalten. Ich Empfehle Regex auf [Regexr](https://regexr.com) zu üben.