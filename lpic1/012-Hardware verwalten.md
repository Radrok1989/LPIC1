### Die Kernelverzeichnisse /proc und /sys
Diese beiden Verzeichnisse stellen virtuelle Kernelverzeichnisse bereit. Sie existieren nur im Arbeitsspeicher. Dh die Daten sind flüchtig und gehen beim runterfahren des Systems verloren.  

**/proc**
Hier finden wir diverse Textdateien die den aktuellen Status von diversen Subsystemen, Komponenten und Prozessen erfassen. Proc hat zahlreiche Unterverzeichnisse. Eines der wichtigsten davon ist sys. Proc konzentriert sich eher auf die Prozesse.

**/sys**  
Enthält informationen über Geräte, Kernelmodule, Dateisysteme und andere Kernel-komponenten. Sys Konzentriert sich eher auf die Kernel-bezogenen Sachen.  

### Das Geräteverzeichnis /dev  
  * tty's sind virtuelle Terminals (**C**haracter Devices)
  * sda/b/c/etc. sind Festplatten (**B**lock Devices)
  * sr0 ist die Gerätedatei für Optische Laufwerke (**L**)
  * /dev/random ist ein zufallsgenerator
  * /dev/zero liefert die gewünschte anzahl an nullbites
  * /dev/null ist der virtuelle Mülleimer

### Gerätetreiber verwalten mit modprobe und Co.
Voraussetzung für die Kommunikation des Linuxkernels mit der im Computer vorhandenen oder am Computer angeschlossenen Hardware ist, dass der Kernel die Sprache der Komponente versteht. Hierfür gibt es Gerätetreiber. Dieser kann schon in den Kernel integriert sein oder dynamisch beim Systemstart oder wärend dem Betrieb als loadable kernelmodule (LKM) nachgeladen werden. Häufig hat man als Linux-Administrator fast keine Berührung mit der Treiberproblematik. Verantwortlich für die Kernelverwaltung ist eine Komponente namens "kmod" (Linux kernel module handling)  
Kerneltreiber haben die Endung .ko was für Kernel Object steht.  
Im Verzeichnis **/drivers** findet man die ganzen Kerneltreiber.

### Usb-Schnittstelle  
  * `lsusb` listet alle usb-devices auf.
  * `usb-devices` gibt detailierte Informationen über die Usb-devices aus.
  * Mit `mount` können wir uns das Dateisystem anzeigen lassen und da suchen sir uns das /dev/sd_ raus welches unser USB-stick ist.
  * wenn es nötig wäre könntest du den usbstick jetzt einbindnen mit `mount -t vfat /dev/sd_ /media/xyz` Warscheindlich ist  es aber dank D-BUS nicht nötig.

### Geräteverwaltung mit Udev  
Udev dient zur Geräteverwaltung und läuft als udevd im Hintergrund. Der Kernel erzeugt für jedes Ereignis Meldungen. Diese Meldungen werden als uevent bezeichnet.
Die uevents werden vom Udev Deamon abgefangen und ausgewertet. 
Die Events und Atkionen von Udev können wir uns mit dem Verwaltungstool von Udev ansehen `udevadm monitor --property`

### Der D-BUS
D D-BUS ist eine Programmbibliothek, die dazu dient, dass Prozesse untereinander über einen logischen BUS kommunizieren können  
Der D-BUS ist eine Middleware, eine Vermittlungsschicht zwischen Prozessen.  
Der D-BUS besteht aus drei Komponenten:
  * Dem D-BUS Daemon
  * Der D-BUS-Bibliothek namens libdbus
  * Dem D-BUS-Protokoll
Der D-BUS enthält Informationen von Hotplugging-EReignissen von Udev und sorgt im beispiel  des USB-Sticks dafür, dass diese Information an den Dateimanager weitergegeben werden.
Der D-BUS muss nicht konfiguriert werden.

### Hilfreiche Programme im bezug auf Hardware verwalten
  * `ps ax` - zeigt alle laufenden Prozdesse an.
  * `kmod list` - zeigt alle derzeit aktiven Kernel-Module
  * `ls mod` - tut das selbe wie kmod list und ist gängiger
  * `modinfo "kernel-module"` - gibt Informationen über das jeweilige Kernel-Modul
  * `modprobe -v "entsprechender Treiber"` - Damit kann man einen Treiber aktivieren und die -v flag sorgt dafür, dass alle abhängigkeiten auch direkt aktiviert werden.
  * `modprobe -vr "entsprechender Treiber"` - Damit kann man einen Treiber und seine Abhängigkeit wieder deaktivieren.
  * `lsusb` listet alle usb-devices auf.
  * `usb-devices` gibt detailierte Informationen über die Usb-devices aus.
  * mit `mount` können wir uns das Dateisystem anzeigen lassen.
  * `lspci` Zeigt die am BUS anliegenden Geräte.
  * `udevadm monitor --property` - Die Aktionen von Udev ansehen