## Systemstart  
### BIOS - Basic Input Output System  
Seit 1989 - von IBM    
POST (Power On Self-Test) - Der Computer überprüft sich selbst und checkt alle elementaren Komponenten durch.  
Damit wird die Hardware initalisiert  
Das BIOS kann konfigurert werden. Beim Boot wärend des POST-vorgangs muss dafür Del, F2, F8, F12, oä gedrückt werden. Ist bei jedem Hersteller etwas anders.  
Das BIOS startet ein Programm, dass sich Bootloader nennt und im MBR gespeichert ist.  
Das BIOS wurde im Laufe der Zeit immer weiterentwickelt, aber irgendwann stiess es an seine Grenzen. Es ist nämlich nicht 64-Bit-tauglich. Dafür wurde das UEFI entwickelt.  
Auf dem nichtflüchtigen Speicher des Motherboards gespeichert

### MBR - Master Boot Record  
Enthält das Startprogramm für das Betriebssystem (Bootloader)  
Befindet sich in den ersten 512 Bytes des Bootmediums. Dieser Sektor wird dafür reserviert.  
Das MBR kann nur ab Festplatten bis zu 2TB booten.  
Schwachpunkt: Single-point-of-failure - ist der MBR zerschossen, so funktioniert die gesamte Festplatte nicht mehr.  

### (U)EFI - Uinified Extensible Firmware Interface  
zuerst hiess es nur EFI  
Fokus auf 64-Bit-Systeme  
Secure Boot (erfordert signierte Bootloader) verhindert Maleware die direkt beim Systemstart initialisiert wird.  
Es hat eine grafische Oberfläche und ist wesentlich leistungsfähiger als das BIOS. In den meisten Fällen sogar Mausunterstützt.  
Deshalb kann es auch Treiber selektiv laden, der Computer ist schon in diesem frühen Stadium netzwerkfähig, etc.  
Eines der Features ist das GPT.
Auf dem nichtflüchtigen Speicher des Motherboards gespeichert  

### GPT - GUID Partition Table  
Das GPT kann auch von grösseren Festplatten als 2TB booten.  
Redundanz durch sekundäre GUID-Partitionstabelle - verbesserung zur MBR single-point-of-failure  
  
## Der Bootmanager GRUB2 - GRand Unified Boot system  
GRUB ist der Standard-Bootloader von Linux  
Original-GRUB -> GRUB1 oder GRUB Legacy  
Aktueller GRUB -> GRUB2 (oft nur GRUB genannt)  
Der GRUB wird in die ersten 512 Bytes der Festplatte geschrieben (MBR oder GPT) und vom BIOS bzw UEFI aufgerufen.  
Er verweisst an /boot/grub(2) das 2 ist je nach Distro und übergibt dort den weiteren Start an den Kernel
  * Zusammengesetzte Conf-Datei: /boot/grub/grub.cfg -> bei GRUB Legacy /boot/grub/menu.lst
  * Das Startmenü konfigurieren /etc/default/grub
  * ausserdem werden umfangreiche scripte aus /etc/grub.d in die grub.cfg eingelesen
  * den Befehl um die grub.cfg einzulesen `grub-mkconfig -o /boot/grub/grub.cfg`
  * den Befehl um GRUB zu installieren `grub-install /dev/sda`
  * <span style="background-color: #AA4A44">Wichtier Unterschied zwischen Debian-Derivaten und Red Hat-Derivaten:</span> Bei Red Hat muss überall "grub2" verwendet werden, zB /boot/grub2, `grub2-install`, etc.

## Grundlagen der Partitionierung
### Gründe für eine Partitionierung
  * Logische Unterteilung der Daten (Betriebssystem, Programme, Nutzdaten)
  * Das Betriebssystem kann unabhängig von den restlichen Daten wiederhergestellt werden
  * Durch Partitionsimages können einfache Backups erstellt werden
  * Jede Partition kann mit einem passenden Dateisystem versehen werden
  * Defragmentiereung dauerrt bei kleineren Partitionen weniger lang (nicht mehr so schlimm,da es nur noch sehr wenig HDD's gibt)
  * Partitionen sind separat administrierbar
    * Quotas (Speicherbeschränkung)
    * Verschlüsselung
    * Medientausch
    * Dateisystem-Reperatur

### MBR und GPT partitionierungen  
Klassische Partitionierung
  * Grösse grundsätzlich frei wählbar
  * Maximal 4 Partitionen (Primär oder Erweitert)
  * Beliebige Logische Partitionen in der erweiterten Partition

GPT-Partitionierung
  * Grösse grundsätzlich frei wählbar
  * Maximal 128 Partitionen
  * Keine Unterteilung in primär und logisch

![Partitionen](/bilder/partitionen_mbr_GPT.jpg)  
*Ja dieses Bild ist von Windows aber das Prinzip ist das selbe*

### Dateisysteme: EXT4, XFS, BtrFS, ZFS & Co.  
Partitionen werden mit einem Dateisystem formatiert.  
Das Dateisystem organisiert die Ablage der Daten  
Dateisysteme können weitere Features enthalten 
  * Standard-Dateisysteme für Linux:  
    * ext2 (sprich Second Extended): altes  Dateisystem, Rechteverwaltung, max. 32 TB, keine Journaling-Funktion(Doppelter Boden, erfasst änderungsvorgänge am Dateisystems).
    * ext3 baut auf ext2 auf, ebenfalls max. 32 TB, alle ext2-Features, enthält Journaling-Funktion.
    * ext4 baut auf ext3 auf, max. 1 EB, Journaling-Funktion, Geschwindigkeitsoptimierung
  * Weitere Dateisysteme:  
    * Btrfs (sprich Better FS): Standard in SUSE Linux, sehr leistungsfähig (zb Snapshots) aber teilweise fehlerhaft
    * ReiserFS: arbeitet sehr effizient mit kleinen Dateien, wird aber nur noch selten eingesetzt, aktuell: Reiser4
    * XFS: bereits älter, ausgereift und stabil, arbeitet gut mit grossen Partitionen, selten im Desktop-Bereich
    * ZFS: Eines der leistungsfähigsten Dateisysteme mit vielen Features, Einsatz hauptsächlich auf Servern
    * Microsoft: Fat16, Fat32 (keine Rechteverwaltung), NTFS(MS Standard), exFAT(Flash-Speicher)

### /etc/fstab - Dateisysteme automatisch einbinden  
Beim Systemstart müssen die Daten, auf die das Betriebssystsem wärend dem Bootvorgang zugreifen muss, schon vorhanden sein. Wir können also nicht erst booten und dann im laufenden Betrieb alle Dateisysteme mounten. Daher stellt die Datei /etc/fstab eine Liste mit Mountpunkten und Parametern bereit, damit das System zu Anfang vom Bootvorgang schon weiss wo was hingehört.

### Hilfreiche Programme im Bezug auf Partitionierung 
  * fdisk - damit kann man eine Festplatte partitionieren mit MBR
    * `fdisk /dev/sdb` (wenn sdb die Festplatte zum partitionieren ist) damit kommt man ins Partitionierungsmenü
    * Mit m kann man die verfügbaren Befehle sehen
    * Mit n kann man eine neue Partition erstellen
    * Der Rest ist relativ selbst erklärend
    * Wenn du mit deinen Partitionen einvarstanden bist, dann kannst du mit w die Tabelle schreiben und das Programm beenden.
    * <span style="background-color: #AA4A44">Wichtig! wenn du eine Festplatte beschreibst, wird alles  was vorher darauf war formatiert und ist verloren! Wenn also dein MBR darauf ist, dann fährst du dein System an die Wand.</span>
  * gdisk - damit kann man eine Festplatte partitionieren mit GPT
    * `gdisk /dev/sdb` (wenn sdb die Festplatte zum partitionieren ist) damit kommt man ins Partitionierungsmenü
    * Mit ? kann man die verfügbaren Befehle sehen
    * Ab jetzt ist es ähnlich.
  * parted - funktioniert sehr ähnlich wie fdisk und gdisk
    * parted hat eine grafische Oberfläche ich mit `apt install gparted` installiert werden kann.
  * mkswap - um eine Partition zu einer Swap-Partition formatiert.
  * swapon - um eine Swap-Partition zu aktivieren.
  * mkfs - Befehl zum erstellen von Dateisystemen
  * fsck - ist eine Software zur Prüfung der Konsistenz von Dateisystemen. Sollte nicht bei laufendem Betrieb bei gemounteten Partitionenn gemacht werden.
  * mount - damit kann ein Verzeichnis gemountet werden
  * umount - damit kann ein Verzeichnis geunmountet werden
  * debugfs - zum manuel debuggen von ext file-systemen. Sehr mächtig aber du solltest wissen was du tust.


### Wichtige Pfade im Bezug auf Partitionierung  
  * /dev - da sind alle Gerätedateien wie Festplatten, Optische Laufwerke, Usbsticks, etc. zu finden  
  * /proc/swaps - zeigt alle Swap-Partitionen
  * /etc/fstab - bindet Dateien automatisch ein

### Festplattennutzung anzeigen  
  * nützliche Programme  
    * df (/usr/bin/df) - steht für disk free und zeigt alle gemounteten Systeme an
      * -h steht für human readable und zeigt es in MB, GB, TB an.
    * du (usr/bin/du) - steht für disk usage und fasst den verbrauchte Speicherplatz des aktuellen Verzeichnisses an.
      * -h steht wieder für human readable
      * -a steht für alle
      * -s nur die Summe (es wird aber nur das angezeigt, bei dem man auch die berechtigung hat. Empfehlung: mit sudo ausführen.)


### Quotas einrichten
Quotas ist eine Speicherplatz-Limitation für den User. Es gibt ein Softlimit und ein Hardlimit. Das Hardlimit kann nie überschritten werden. Das Softlimit aber schon. Wenn das Softlimit überschritten wird, hat der User eine gewisse Zeit, zeit um wieder unter das Softlimit zu kommen. Wenn er das nicht tut, wird das Softlimit zum Hardlimit, bis der User wieder unter das Softlimit kommt.
  * `apt install quota` (u)
  * `yum install quota` (c)
    * Um sie zu ermöglichen gehen wir in die Datei /etc/fstab und ergänzen in den Optionen einer /dev usrquota und ggf. grpquota.
    * um sie zu aktivieren `quotacheck -cug /pfad`
    * quota konfigurieren: `edquota -u "user"` da kann dann ein Soft- und Hardlimit gesetzt werden und auch die Zeit in Tagen um wieder unter das Softlimit zu kommen.
    * mit `quotaoff /dev/pfad`und mit `quotaon /dev/pfad` kann es aus oder eingeschalten werden.