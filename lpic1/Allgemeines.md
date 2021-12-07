(U) = Ubuntu
(C) = CentOS

## 3 Hauptdistributionen  
  ### Red Hat
    Fedora  
    Red Hat Enterprise  
    CentOS  
  ### SuSE  
    SUSE Linux Enterprise server  
    openSUSE  
  ### Debian  
    Debian GNU  
    Knoppix  
    Ubuntu  
    Linux Mint (Beliebteste Distro)  
    Kali(Hackerlinux)  

### Programm global ausführbar machen  
    temporär  
        export PATH=$PATH:/pfad/  
    auf Dauer  
        in den files: .profile für login-shells und .bashrc für nonlogin

### Verschiedene Shells
    Interaktive login Shells .profile
    Interaktive nonlogin Shell .bashrc
    Nicht Interaktive Shell

### Packetverwaltung
    Ubuntu/Debian  
        apt/dpkg (snap)  
            packet -> .deb   
    Red Hat/CentOS  
        yum  
            packet -> .rpm  
    Arch  
        pacman  


3-Dateien werden in /etc/apt/sources.list.d abgespeichert  
apt update fragt da auch ab ob es updates gibt.  

Linux ist in gegensatz zu  zb Windows ein Multiusersystem. Dh mehrere Leute können gleichzeitig auf dem selben System arbeiten.  