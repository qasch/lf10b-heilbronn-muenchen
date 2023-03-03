= Dokumentation LF10b

== Grundlegende Linux Kommandos

* `ls` Inhalt eines Verzeichnisses anzeigen
* `ls -a` zeigt _alles_ an, auch versteckte Dateien (Kurzform)
* `ls --all` zeigt _alles_ an, auch versteckte Dateien (Langform)
* `ls -l` zeigt ausführliche Informationen der Dateien an
* `touch` leere Datei(en) zu erstellen
* `clear` leert den Bildschirm
* `rm` Dateien löschen
* `ssh user@hostname` als Bentuzer `user` auf `hostname` anmelden (Secure Shell)
* `whoami` gibt den aktuell angemeldeten Benutzer aus
* `pwd` absoluten Pfad des aktuellen Verzeichnisses an
* `nano` einfacher Texteditor (auf fast allen Distributionen vorinstalliert)
* `history` Liste alle bisher eingegebenen Befehle 
* `ip address` listet alle Netzwerkadapter inkl. ihrer Konfiguration (IP Adresse etc.)
* `ip addr` listet alle Netzwerkadapter inkl. ihrer Konfiguration (IP Adresse etc.)
* `ip a` listet alle Netzwerkadapter inkl. ihrer Konfiguration (IP Adresse etc.)
* `systemctl status ssh` zeigt an, ob der Dienst `ssh` läuft 
* `man <kommando>` Handbuchseite von `<kommando>`
* `cd verzeichnis` wechselt in das Verzeichnis `verzeichnis`
* `cd` wechselt direkt ins Heimatverzeichnis des aktuellen Benutzers
* `cd -` wechselt zwichen dem aktuellen und vorherigen Verzeichnis
* `echo was auch immer` gibt den Text `was auch immer` auf der Shell aus
* `less`: ist ein Pager, d.h. ein Programm, dass Text(ströme) so anzeigen kann, dass man sich darin bewegen/scrollen kann, darin suchen kann etc.
* `grep` sucht nach Text innerhalb von Dateien
* `grep alias ~/.bashrc` gibt alle Zeilen der Datei `.bashrc` aus, in denen der Suchbegriff, das Pattern `alias` vorkommt
* `wc -l` zählt die Anzahl der Zeilen eines Textstroms

== Zwei Arten von Kommandos:

* extern realisiert (rm, ls, man, date ...)
* interne Kommandos (in die BASH eingebaut): Builtins

== Pfadangaben - der Weg zu einer Datei

* relativer Pfad: ausgehend vom aktuellen Verzeichnis
* absoluter Pfad: ausgehend von der Wurzel (`/`) des Dateisystems

== Umleitungen und Redirect

=== Unix/Linux Philosophie

1. Schreibe Programme, die genau eine Sache tun, und diese sollen sie gut machen
2. Schreibe Programme, die zusammenarbeiten
3. Schreibe Programme, die mit Textströmen umgehen können, denn das ist ein universelles Interface

==== KISS - Prinzip

* Keep it simple, stupid!
* Keep it stupid simple
* Keep it super simple
* -> Halte es so einfach wie möglich

=== Kanäle/Streams

* `stdin`: Standardeingabekanal - `0` 
* `stdout`: Standardausgabekanal - `1`
* `stderr`: Standardfehlerkanal - `2`

*Jedes* Kommando ist verknüpft mit diesen drei Kanälen.

=== Kanäle umleiten

* `>` einfacher Redirect: leitet Kanäle um, erzeugt Datei bzw. überschreibt Inhalt
* `>>` doppelter Redirect: leitet Kanäle um, erzeugt Datei, überschreibt Inhalt *nicht*, sondern hängt ihn an das Ende der Datei an
* `|` die _Pipe_ leitet die Standardausgabe eines Kommandos um in die Standardeingabe eines anderen Kommandos

==== Beispiele

* `cat 1>ausgabekanal.txt` leitet alle den Standardausgabekanal von `cat` um in die Datei `ausgabekanal.txt` (die `1` kann in diesem Fall auch weggelassen werden)
* `cat datei.txt > kopie.txt` schreibt den Inhalt von `datei.txt` in die Datei `kopie.txt`
* `echo huhu > datei.txt` schreibt den Text `huhu` in die Datei `datei.txt`
* `ls -l /etc > ls_ausgabe.txt` schreibt die Ausgabe des Kommandos `ls -l /etc` in die Datei `ls_ausgabe.txt`
* `ls -l alksjdlkjflkasldkjf 2> ls_fehler.txt` da dieses Kommando einen Fehler produziert, wird die Ausgabe, also der Fehlerkanal in die Datei `ls_fehler.txt` geschrieben
* `ls -l /etc | wc -l` zählt die Anzahl der Dateien und Verzeichnisse im Verzeichnis `/etc`

== Prozesse

* `PID` Process ID, bestimmt einen Prozess eindeutig
* `init` Mutter aller Prozesse
* `systemd` auf fast allen modernen Systemen für das Starten und Steuern von Diensten zuständig 
* An Prozesse können Signale gesendet werden:
* `kill -15 4711` `kill -s SIGTERM 4711` freundliche Aufforderung an den Prozess, sich zu beenden
* `kill -9 4711` tötet den Prozess, keine Gegenwehr möglich
* `killall -15 sleep` beendet alle Prozesse mit dem Namen `sleep` ordnungsgemäß

== Benutzer

Es gibt zwei Arten von Benutzern: 

* _reale_ Benutzer 
* _Systembenutzer_ oder _Pseudobenutzer_

Reale Benutzer können sich mit einer Shell (z.B. `/bin/bash`) interaktiv am System anmelden und so Kommandos ausführen.

System- oder Pseudobenutzer sind zur Steuerunt von Diensten da. Die Dienste laufen dann also mit den (eingeschränkten) Rechten dieser Benutzer.

=== Benutzer und Gruppen erstellen

* `adduser tux` erstellt den Benutzer `tux` interaktiv, inkl. Passwort
* `addgroup gfn` erstellt die Gruppe `gfn`

=== Gruppenzugehörigkeiten ändern

* `usermod -aG gfn tux`: den Benutzer `tux` der Gruppe `gfn` hinzufügen
* Hinweis: Benutzer und Gruppe müssen bereits existieren

==== Ändern der Besitzverhältnisse

 chown user:group datei    # Besitzer und Gruppe ändern
 chown user datei    	   # nur Besitzer ändern
 chown :group datei        # nur Gruppe ändern

 chown karla datei         # karla als Besitzer von datei festlegen 
 chown tux:karla datei     # tux als Besitzer von datei festlegen, datei der Gruppe karla zuweisen
 chgroup karla datei       # datei der Gruppe karla zuweisen

== Berechtigungen

Es gibt folgende Berechtigungen:

 r : read 
 w : write
 x : execute

 (Minus) - : Recht nicht vorhanden

Verzeichnislisting mit `ls -l`:

 user  group other
 rw-   r--   r-- 

=== Symbolische Rechtevergabe

 chmod u-w datei    # User Schreibrecht entziehen
 chmod g+w datei    # Gruppe Schreibrecht hinzufügen
 chmod o+x datei    # Others Ausführungsrechte hinzufügen
 chmod a-wx datei   # Allen (User, Group, Others) Ausführungsrechte und Schreibrecht entziehen

=== Oktale Rechtevergabe

Die einzelnen Rechte werden auch oktal repräsentiert:

 s: symbolisch
 o: okal
 b: binär

 s   o    b
 -----------
 r : 4   100
 w : 2   010
 x : 1   001  

Grund für die oktale Rechtevergabe bzw. interne Repräsentation auf dem Datenträger:

  7  6  4    Oktal
 111110100   Binär
 rwxrw-r--   Symbolisch

Dateiberechtigungen folgendermaßen setzen: User: `rwx`, Group: `rw`, Others: `r`

       ugo
 chmod 764 datei

*Hinweis:* Bei der oktalen Rechtevergabe können Berechtigungen nur *komplett* vergeben werden, also nur für Besitzer, Gruppe und Others in einem, nicht einzeln für z.B. den Besitzer. 


== Installation von Software

* Software in Paketen gebündelt (ausführbare Datei, Handbauchseiten, Konfigurationsdateien etc.)
* Pakete werden auf speziellen Servern gehostet (Paketquellen/Repository)
* dazu werden Paketmanager genutzt (`apt` auf Debian, `dnf` auf RedHat etc.)
* Upgrade *aller* auf dem System vorhandener Software:
** `apt update` Paketquellen aktualisieren
** `apt upgrade` aktualisiert alle Pakete, für die es eine neue Version gibt
* Installationsschritte:
** `apt update` Paketquellen aktualisieren
** `apt install <paket>` Paketquellen aktualisieren
* Pakete entfernen:
** `apt remove <paket>` Paket <paket> entfernen

== sudo 

* mit `sudo` kann man unter anderem für ein Kommando erhöhte Rechte, also Root (bzw. Administrator)-Rechte erlangen:

 sudo apt install <package>

* dazu muss das Paket `sudo` installiert werden
* und unter Debian die Benutzer, die `sudo` benutzen können sollen, der Gruppe `sudo` hinzugefügt werden
* wird ein Kommando mit `sudo` ausgeführt, so muss das _eigene_ Benutzerpasswort, nicht das Root-Passwort eingegeben werden
* die erhöhten Rechte gelten nur für dieses eine Kommando


















































