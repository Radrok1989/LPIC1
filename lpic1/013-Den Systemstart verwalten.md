### Der Linux-Systemstart im Überblick
BIOS/UEFI ->&ensp; &ensp; &ensp; &ensp;MBR ->&ensp; &ensp; &ensp; &ensp; GRUB -> &ensp; &ensp; &ensp; &ensp; initrd -> &ensp; &ensp; &ensp; &ensp; Linux-Kernel -> &ensp; &ensp; &ensp; &ensp;init (PID 1) -> &ensp; &ensp; &ensp; &ensp; Prozesse  
  * initrd: Initial RAM disc -> enthält einen Minimalkernel, der nur eine einzige  Aufgabe hat. Die notwendigen Kernelmodule zu identifizieren um die gefundene Hardware zu unterstützen. In diesem Zusammenhang werden die gefundenen Festplatten gemountet.
  * Linux Kernel: Dann übergibt initrd an den Richtigen Kernel auf der Festplatte. Das ist der vmlinuz. Das ist so weil es handlicher und flexibler ist.
  * Init (PID 1): Zum Starten der Programme als Bestandteil des Betriebssystems initiert der Kernel einen Init-Prozess. Dieser erste Prozess hat immer die ProzessID 1. Er seinerseits prüft die Systemkonfigurationsdateien und ermittelt die Parameter für den Linux Systemstart. Dazu gehört das Systemlevel in das gebootet werden soll, auch als run-level bekannt und natürlich die Programme die nach dem Bootvorgang gestartet sein und zur Verfügung stehen sollen. Viele Jahre war das SysVinit (Sysfiveinit) Standard bei Linuxsystemen. Zwischenzeitlich gab es Upstart und jetzt ist es Systemd.

### Kerneloptionen und Bootparameter beim Systemstart übergeben  
Der Bootloader GRUB ermöglicht das Editieren von Systemstart-Parametern
Mit der Taste e öffnet sich der Editor mit den Startparametern.
Mit ctrl+x könnte man das System mit den geänderten Einstellungen starten

### SysVinit
Ist nicht mehr der Standard aber nach wie vor kompatibel. Momentan befinden wir uns in der Übergangsphase. Längerfristig werden die SysVinit abgelöst.
  * Der Kernel startet inen ersten Prozess namens **init**
  * Init hat die Prozess-ID 1
  * Init startet (forked) weitere Prozesse in einer vorgegebenen Reihenfolge hintereinander (nicht parallel!)
  * In /etc/init.d/ stehen die init-Skripte
  * in /etc/rc[0-6].d/ stehen verlinkungen auf Initskripte:
    * `S< Ziffer>< Skriptname>` - startet Skript
    * `K < Ziffer>< Skriptname>` - stoppt Skript
  * Bootlevels werden in Runleveln unterschieden
  * Bootlevels werden in Runlevel unterschieden (rcX -> X=Runlevel)
  * Kontroll-Befehl für Dienste: `service < Dienstname> start|stop|restart|status

**Runlevel**
<span style="background-color: #87CEFA">0 - Shutdown  
S - Single-User (kein Stadard)
1 - Einzelnutzer ohne Netzwerk
2 - Lokaler Mehrnutzerbetrieb ohne Netzwerk
3 - Multiuser mit Netzwerkbetrieb
4 - nicht reserviert
5 - Wie 3 mit Graphical User Interfacce
6 - Reboot</span>

### systemd  
systemd verfolg eine radikale umstrukturierung des Systemstartkonzepts.
Kritikpunkte an systemd:
  * Mehrere Aufgaben
  * Komplizierter
  * Fehleranfälliger
  * Schwächen werden bestritten
systemd ist abwärtskompatibel zu sysVinit-skripten; kann diese also auch verwalten. Im Gegensatz zu sysVinit startet systemd die Prozesse allerdings parellel zu einander. systemd kann verwirrend sein, weil ja die ganzen sysVinit Befehle noch funktionieren. systemd wird über units konfiguriert units sind dateien in einem Windowsini ähnlichen Format. Sie sind unter /lib/systemd/system gespeichert.

### Boot Targets  
Die systemd Targets sind Units die dazu da sind einen bestimmten Systemzustand bzw. bestimmte Aspekte des Zustandes herzustellen. Die Boottarggets sind das systemdpendatnt zu den Runleveln von sysVinit.
Zu finden sind sie unter **/lib/systemd/system/**.
<span style="background-color: #87CEFA">Runlvl Boottarget(/lib/systemd/system)
0 &ensp; &ensp; &ensp; &ensp; poweroff.target (runlevel0.target)  
1 &ensp; &ensp; &ensp; &ensp; rescue.target (runlevel1.target)
3 &ensp; &ensp; &ensp; &ensp; multi-user.target (runlevel[2-4].target
5 &ensp; &ensp; &ensp; &ensp; graphical.target (runlevel5.target)
6 &ensp; &ensp; &ensp; &ensp; reboot.target(runlevel6.target)
</span>
  * Das Default-Target wird unter **/etc/systemd/system/** als Symlink auf eines der obenstehenden Targets gesetzt.
  * Unter **/etc/systemd/system/** werden Einstellungen vorgenommen, die in jedem Fall berücksichtigt werden.
  * **/etc/systemd/system/*.target.wants-Verzeichnisse** enthalten die Dienste, die in dem jeweiligen Systemzustand gestartet werden sollen - analog zu den Runlevel-Verzeichnissen.
  * Die Datei **/etc/inittab/** regelt unter SysVinit den Systemzustand. Bei Systemd spielt sie keine Rolle mehr.

### Die System-dUnits unter der Lupe
  * Systemweite Units werden unter **/lib/systemd/system** abgelegt, **/usr/lib/.../** ist ein Symlink
  * Anpassungen werden immer in **/etc/systemd/system vrogenommen.
  * User-Units werden im Gegensatz zu System-Units nicht beim Systemstart, sondern bei An- und Abmeldung eines Benutzers ausgeführt bzw. beendet.
  * User-Units stehen z.B. unter **/lib/systemd/user/** bzw **/etc/systemd/user/** - hier werden sie übber *.target.wants-Verzeichnisse ebenso verlinkt, wie die System-Units
  * Im Home-Verzeichis eines Benutzers können .local und .config als Unterverzeichnisse existieren, in denen User-Units abgelegt werden können.
  * Wichtiger als User-Units sind die System-Units, deren Konfigurationsdateien folgenden Aufbau haben (Beispiel: Service-Unit):
    * [Unit] - enthält allgemeine Informationen zur Unit.
    * [Service] - enthält Einstellungen, wie der Service zu starten ist.
    * [install] - enthält Abhängigkeiten und Startbedingungen.
  * Service - Units können aktiviert oder deaktiviert werden (systemctl| enable|disable < Unit >)
  * Aktivierte Units können gestartet und gestoppt werden (systemctl| start|stop < Unit >)
  * Weiter Unit-Typen sind:
    * .mount - zum Ein- und Aushängen von Dateisystemen
    * .socket - zum Herstellen von Verbindungen zwischen Prozessen
    * .path - zur Ausführung von Prozessen in Abhängigkeit von bestimmten Änderungen

### Nützliche Befehle im Bezug auf den Systemstart
  * `init 0` - System herunterfahren
  * `init 6` - System neu starten
  * `reboot` - System neu startenn
  * `halt` - schaltet das System sofort aus.
  * `shutdown -r now`
  * `runlevel` - zeigt das Runlevel an.
  * `systemctl set-default graphical` das default Runlevel auf graphical ändern. 