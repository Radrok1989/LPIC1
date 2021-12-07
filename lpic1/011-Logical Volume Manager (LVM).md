## Logical Volume Manager (LVM)

### Was ist ein LVM
Ein Logical Volume Manager sorgt für eine zusätzliche Abstraktionsschicht zwischen physischen Datenspeichern und dem logischen Datenspeicherbereich eines Rechners mit seinem Dateisystem. Sogenannte logische Volumes können sich über physische Laufwerksgrenzen hinweg erstrecken und sind flexibel handhabbar. Beispielsweise werden Partitionen dynamisch veränderbar.  
![LVM](bilder/LVM.jpg)

### Nachteile der klassischen Partittionierung
  * Die normale Aufteilung der Festplatten und Datenträger in Partitionen ist zu unflexibel
  * Partitionen werden einmal erstellt und behalten ihre Grösse ( spätere Änderungen nur schwer möglich)
  * Partitionen können maximal so gross sein, wie der Datenträger auf dem sie erstellt werden.

### Zusätzliche Features von LVM
  * Locical Volumes können sich über mehrere physische Festplatten erstrecken (Physical Volumes)
  * Unterstützt RAID zu Ausfallsicherheit (hard- und softwarebasiert)
  * Snapshots (Schnappschüsse von Volumes)
  * LV-Mirroring (zwei Physical Volumes spiegeln ein Logical Volume)

### Physical Volumes, Volume Groups und Logical Volumes erstellen
  * Auf CentOS ist LVM schon vorhanden. Auf Ubuntu `apt-get install lvm2`.
  * Mit `fdisk -l` überprüfen wir alle vorhandenen Datenträger.
  * Mit `pvcreate /dev/sdb1` (nur ein Beispiel) kreiert man physische Volumes
  * `pvs` gibt eine Übersicht über die Physischen Volumes aus.
  * `vgcreate storage /dev/sdb1 /dev/sdc1` erstellt eine Volumegroup
  * `vgs` gibt eine Übersicht über die Volumegroupes aus.
  * Mit `lvcreate -L3g -n bilder storage` kreiert man Logical Volume. -L gibt die grösse an und mit -n den Namen.
  * `lvs` zeigt die vorhandenen Logical Volumes.
  * `pvdisplay` gibt detailierte Infos aus über die Physischen Volumes.
  * `vgdisplay` gibt detailierte Infos aus über die Volume-Groups.
  * `lvdisplay` gibt detailierte Infos aus über die Logical Volumes.

### Logical Volumes formatieren und mounten
  * `mkfs.ext4 /dev/storage/bilder` formatiert das LVM mit ext4
  * `mkdir /bilder` erstellt ein Verzeichnis für das LVM 
  * `mount /dev/storage/bilder /da_wo_es_hin_soll/bilder` Mountet das LVM
  
### Volume Groups und Logical Volumes erweitern
Hierbei geht es darum wie man ein Logical Volumes um Speicherplatz erweitern kann.
  * `vgextend storage /dev/sdc3` die sdc3 der Volume Groupe hinzufügen.
  * bevor wir nun das dir bilder vergrössern können müssen wir en unmounten `umount /da_wo_es_ist/bilder`
  * Nun Prüfen wir bilder aus Konsistenzgründen `fsck.ext4 /dev/storage/bilder`
  * Jetzt resizen wir das LV entweder absolut oder um das was wir es erweitern wollen. Absolut: `lvresize -L 9g /dev/storage/bilder`| erweitern: `lvresize -L +5g /dev/storage/bilder`| mit - könnte man es auch verkleinern.
  * Haben wir die virtuelle  Partition vergrössert, müssen wir auch das Dateisystem anpassen. `resize2fs /dev/storage/bilder 9g`
  * jetzt können wir das LV auch wieder einhängen `mount /dev/storage/bilder /da_wo_es_hin_soll/bilder`

### Logical und Physical Volumes und Volume Group entfernen
Entfernt wird in der umgekehrten Reinfolge, in der erstellt wurde.
  * `umount /da_wo_es_ist/bilder`
  * `lvremove storage/bilder`
  * `vgremove storage`
  * `pvremove /dev/sdb1` `pvremove /dev/sdc1`

### Nützliche Befehle im Bezug auf LVM
  * **pvcreate** - Partitionen dem LVM verfügbar machen
  * **vgcreate** - Logical Volume erstellen
  * **vgs** - Zeigt Übersicht über Volume Groups
  * **pvs** - Zeigt Übersicht über Physical Volumes
  * **lvs** - Zeigt Übersicht über Logical Volumes
  * **pvdisplay** - Zeigt detailierte Informationen zu Physical Volumes
  * **vgdisplay** - Zeigt detailierte Informationen zu Volume Groups
  * **lvdisplay** - Zeigt detailierte Informationen zu Logical Volumes
  * **vgextend** - Volume Group um Partitionen erweitern
  * **lvresize** - grösse des Logical Volumes verändern
  * **resize2fs** - grösse des Dateisystems des LV verändern
  * **lvremove** - löscht ein Logical Volume
  * **vgremove** - Entfernt eine Volume Group
  * **pvremove** - Entfernt ein Physical Volume