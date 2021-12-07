### Was sind Cronjobs überhaupt
Cronjobs
  * bieten die Möglichkeit Programme regelmässig auszuführen.
  * Beispiel: Logs eines Webservers einmal am Tag zu archivieren und komprimieren.

### Einen ersten Cronjob erstellen
  * **/etc/crontab/**
    * in dieser Datei werden cronjobs vom System eingetragen
    * unter CentOS kann es sein das die Datei leer ist (macht keinen Unterschied)
  * Format
    * Minute / Stunde / Tag des Monats / Monat / Wochentag / Benutzername / Programm
    * Beispiel Minute
      * '*' steht für immer
      * 5: steht für 5 Minuten
      * 5-25 Jede Minute 5-25
      * 5,25 Minute 5 und Minute 25
      * '*'/5 Alle 5 Minuten
      * 0: Minute 0
    * 0 * * * * root /bin/ls
      * das Programm ls wird einmal jede volle Stunde (Minute 0 ) ausgeführt
  * Cronjob anlegen
    * `vim /etc/crontab/`
    * cronjob in die Datei reinschreiben
    * Datei abspeichern
    * Wird automatisch eingelesen und Cronjob ist aktiv

### Die anderen Cron-Ordner
  * **/etc/crontab/**
    * hier ist angegeben wann die anderen Ordner ausgeführt werden
    * eigene cronjobs können ggf bei Updates überschrieben werden
  * In den Folgenden Ordnern können Skripte angelegt werden die täglich, stündlich, usw. ausgeführt werden sollen.(Wird bei update nicht überschrieben)
    * **/etc/cron.d/**
    * **/etc/cron.daily**
    * **/etc/cron.hourly**
    * **/etc/cron.weekly**
    * **/etc/cron.monthly**

### Cronjobs für Benutzer
  * Standardmässig ist allen Benutzern verboten eigene cronjobs zu erstellen wenn cron.allow oder cron.deny nicht existieren
    * Ausnahme: Unter Ubuntu ist es erlaubt wenn beide Dateienn nicht existieren.
  * **/etc/cron.allow** (Whitelist)
    * hier können einfach die Benutzernamen reingeschrieben werden die cronjobs erstellen dürfen
  * **/etc/cron.deny** (Blacklist)
    * Wenn diese Datei existiert dürfen alle Nutzer cronjobs nutzen ausser diejenigen die in dieser Datei stehen
  * cronjobs als Benutzer anlegen
    * crontab -e
    * Minute Stunde Tag Monat Wochentag command
      * Benutzer muss nicht angegeben werden da es unter dem eingeloggten Benutzer angelegt wird
    * crontab übernimmt automatisch den cronjob
    * **/var/spool/cron/crontabs**
      * Ort der Benutzer cronjobs

### Log-Dateien einsehen
  * CentOS
    * **/var/log/cron/**
  * Ubuntu
    * **/var/log/cron/**
    * syslog konfigurieren
      * **/etc/rsyslog.c/50-default.conf/** oder **/etc/rsyslog.conf/** 
  
### Das Tool at
  * `sudo apt-get install at`
    * Installation von dem Programm at
  * man at
    * Anleitung für at
  * at wird dazu genutzt Befehle oder Skripte einmal in der Zukunft auszuführen
  * `at now + 1 minute`
    * danach erfolgt die Eingabe der Befehle, bestätigen mit Return und abspeichern mit Ctl+D
    * Befehle werden vom jetztigen Zeitpunkt in einer Minute ausgeführt
  * `at no + 1 minute -f ./at.sh` 
    * Das Skript at.sh wird von jetzt an in einer Minute einmal ausgeführt
  * `atq` 
    * zeigt noch aszuführende Jobs an
  * `atrm nummer`
    * löscht den Job mit der angegebenen Nummer
  * **/etc/at.deny** (Blacklist)
    * Datei mit dem Nutzer die at nicht nutzen dürfen
  * **/etc/at.allow** (Whitelist) 
    * Datei mit der Nutzer at nutzen dürfen

### Anacron und Anacrontab
  * ein tool welches sich merkt wann zulest ein cronjob ausgeführt wurde
  * **/etc/anacrontab/**
    * Tage Minuten die vor der Ausführung gewartet werden soll Name Befehl
    * Die Cronjobas können hier nicht auf den Zeitpunkt genau gestartet werden
  *  **/var/spool/anacron**
    * Hier liegen die Dateien wann ein ein cronjob ausgeführt wurde
