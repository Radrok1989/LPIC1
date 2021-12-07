(u) = Ubuntu  
(c) = CentOs  

### head und tail
*  **head /pfad/**  
    * Gibt die ersten 10 Zeilen eines Files aus.  
*  **head -n 100 /pfad/**  
    * Gibt die ersten 100 Zeilen eines Files aus.  
*  **tail**  
    * Für tail gilt das selbe eifach das Ende vom File.  
    
### less and more  
*  **more /pfad/**  
   *  Man kann durch die Zeilen eines Files scrollen. Aber nur nach unten.
*  **less /pfad/**  
   * Das selbe wie more aber man kann auch wieder hochscrollen. Mit q beenden.  
### man und help
  * **man**
    * Syntax: man "Programm"  
      * Bedienungsanleitung für das Programm.  
  * **help**
    * Syntax "Programm" --help
      * Bedienungsanleitung für das Programm.  

### which  
  * Syntax: which "Programm*
    * Zeigt den Pfad des Programms an

### apt-get ("aptitude" gäbe es auch noch) zu finden /etc/apt **(U)**
  * **update**
    * lädt ein update der Packetlisten herunter **keine installation**
  * **upgrade**
    * installiert die Updates der Packetlisten die zuvor runtergeladen wurden.
       * empfehlung: dist-upgrade ffalls es neue abhängikeite gibt.
  * **install**
    * apt-get install "Programm"
      * lädt das Programm und alle abhängigkeiten herunter.

### dpkg installation von 3t-Anbietern -> Programm manuel herunterladen (U)
  * Syntax: dpkg -i "pfad der "exe"
    * evtl braucht es noch ein apt install -f um die abhängigkeiten zu forcieren

### yum (C)
  * yum install "Programm" oder "URL"
  * yum remove "Programm"
  * yum downgrade "Programm"
    * auf eine vorherige Version downgraden

### cat

