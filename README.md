# Dokumentation
## Gruppe Hector 
### Tobias Wittwer, Matthias Vogel, Christian Benthake

#### Warum docker-compose? 

Für ein schnelleres Setup von Container und deren Verknüpfungen haben wir Docker Compose verwendet.
Die docker-compose Datei ist einfacher aufgebaut als die ansible playbooks. Das Rezeptartige der Compose-File macht es sehr flexibel und erweiterbar.
Desweiteren ist docker-compose in der Verbindung mit docker logischer strukturiert, da die docker commands direkt in die File übersetzt wurden.

#### Requirements

Für die Installation mit Docker in Ubuntu werden die Dockerversion größer 1.11 und Docker-Compose größer 1.7 benötigt.

#### docker-compose file
Die docker-compose.yml ist eine Art Rezept in der bestimmt wird welche docker-container wie konfiguriert und verknüpft sind.
Diese Rezept-Definition liegt in Form einer yaml-Datei vor und erleichtert damit das Setup im Vergleich zu z.B. Ansible. 
Nach der Festlegung der Version werden die Services/Container für die Applikation definiert.  
Zuerst wird der haproxy konfiguriert. Im "build" Property wird der Quellordner des Dockerfiles angegeben. Im Build-Verzeichnis liegt neben dem Dockerfile noch die Konfigurationen für den haproxy. Im weiteren Verlauf der Containerdefinition werden der Containername und die Umgebungsvariablen für den haproxy, Portweiterleitungen und Abhängigkeiten sowie Verknüpfungen zu anderen Containern definiert.  

Der interne Port 80 des haproxy-Containers kann von außen mittels Port 8881 aufgerufen werden. Der haproxy ist abhängig und verknüpft mit beiden nginx-containern, die exakt gleich aussehen. 
Die nginx-builds liegen in dem Verzeichnis ./nginx. Indem Dockerfile und Konfigurationen enthalten sind. 
Die Nginx-Container haben die Namen nginx1 und nginx2, hören während der Laufzeit kontinuierlich auf den Port 80 und leiten diesen nach außen auf die Ports 8081 und 8082 weiter. 

#### dockerfile

Das dockerfile  enthält den Installationsprozess eines Containers.
Der haproxy benötigt als Basis ein Ubuntu-image, auf dem alle nötigen Programme installiert werden. Im ersten RUN-Segment werden die Basis-Befehle für die Ubuntu-Distribution ausgeführt: Das Multiverse-Repository hinzugefügt sowie mehrere Programme installiert.
Es folgen die Festlegung der Umgebung und des Arbeitsordners. Dann wird haproxy installiert und alle Konfigurationen dafür in die aktuelle Arbeitsumgebung gelegt.  Nun kann haproxy gestartet und die Ports 80 sowie 443 freigeschaltet werden. Genauso sieht die nginx-dockerfile auch auch, nur dass eben nginx installiert wird und für nginx benötigte Dateien zur Verfügung gestellt werden, wie zum Beispiel der Zertifikate-Ordner.

#### Konfigurationen

Die Konfiguration des haproxy stammt aus der gegebenen Vorlage und die  des nginx entspricht der Standard Konfiguration.