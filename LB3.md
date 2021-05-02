# Einleitung
Dies ist die Dokumentation für das LB3 Docker-Projekt für Modul 300. Ich hwollte verschiedene Komponenten für dieses Docker-Projekt verwenden. Ich habe nur einen Reverse-Proxy, der 3 Webanwendungen schützen kann, 2 einfache Apache's und einen eigenen Apache-basierten Container. Alle HTML-Dateien sind externe Dateien, sodass sie beim Verschieben des Containers weiterhin verfügbar sind(Von Herrn Berger vorgeschlgen, bzw. gezeigt). Im Container befindet sich ein internes Netzwerk, auf das von außen nicht zugegriffen werden kann! Auf die Website kann nur über einen Reverse Proxy zugegriffen werden. Diese Art bietet zusätzlichen Schutz für Webanwendungen und war mir in dem Sinn sehr positiv vorgekommen.
# Servicebeschreibung
Dieser Service ist ein einfacher Webserver bei dem die Unterseiten auf verschidenen Server liegen. So kann man beispielsweise einen Blog und ein Wiki auf der genau gleichen IP haben!
# Netzwerkübersicht
Dieses Dockerprojekt hat ein Internes Netzwerk welches nur von den Containern selbst zugegrifen werden kann und eines das auch von Extern ereichbar ist. Von Extern ist nur der nginx Reverse Proxy über den Port 8080 ereichbar.


    +------------------------------------------------------------------------------------------------+
    ! Dockernetz - 172.28.0.0/16                                                                     !  
    ! NAT-Port: 8080                                                                                 !	
    !                                                                                                !	
    !    +--------------------+          +---------------------+          +---------------------+    !
    !    ! Web App1           !          ! Web App2            !          ! Web App3            !    !
    !    ! Container: httpd   !          ! Container: httpd    !          ! Container: eigener  !    !
    !    ! IP: 172.28.1.3/16  !          ! IP: 172.28.1.4/16   !          ! IP: 172.28.1.5/16   !    !
    !    ! Port: 80           !          ! Port: 80            !          ! Port: 80            !    !
    !    ! URL: /web1         !          ! URL: /web2          !          ! URL: /web3          !    !
    !    +--------------------+          +---------------------+          +---------------------+    !
    !                       /|\                    /|\                    /|\                        !
    !                        |                      |                      |                         !
    !                        \______________________|______________________/                         !
    !                                               |                                                !
    !                                               |                                                !
    !                                    +----------|----------+                                     !
    !                                    ! Nginx Server        !                                     !
    !                                    ! Reverse Proxy       !                                     !
    !                                    ! IP: 172.28.1.2/16   !                                     !
    !                                    ! Container: nginx    !                                     !
    !                                    ! Port: 80            !                                     !
    !                                    ! NAT: 8080           !                                     !
    !                                    +---------------------+                                     !
    !                                              /|\                                               !
    !                                               |                                                !
    +-----------------------------------------------|------------------------------------------------+
# Container Bauen

Um meinen eigenen Docker Container zu bauen benötigt man nur das Dockerfile. Mit dem Befehl docker build -t albion . Baut man sich meinen Container zusammen. Er Basiert auf Ubuntu 18.04 und Apache.
# Umgebung Starten
Um diese Docker Umgebung zu starten muss man gewisse vorbereitungen Treffen.

Alle Datein Herunterladen. (docker-compose.yml,Dockerfile,nginx.conf)
Ein Verzeichnis Erstellen.
Die Heruntergeladen Dateien in dieses Verzeichnis verschieben.
3 Ordner erstellen mit den Namen http1,http2 und http3.
In jedem Verzeichnis mindestens eine index.html erstellen.
Im nginx.conf die Line 7 Anpassen auf die IP des Host's auf dem die Docker Umgebung läuft, alternativ kan man hier auch einen DNS Namen eintragen der auf den Host zeigt.
Den eigenen Container Bauen mit docker build -t albion .
Mit docker-compose up -d die ganze Umgebung starten Fertig
Um Die Umgebung herunter zu fahren reicht ein Befehl: docker-compose down.
# Testfälle
Service	Testfall	Beschreibung	Erwartetes Ergebnis	Tatsächliches Ergebnis
Proxy	Nginx Anzeige	Wen man auf die IP geht bekommt man eine Webseite die vom Nginx stammt.	Nginx git den Fehler 404 zurück weil er selbst keine Webseite beinhaltet.	Nginx gibt den Fehler 404 zurück.
Web Server 1	Web Server 1 ereichbar	Der Web Server 1 sollte eine kleine Webseite Anzeigen	Die Webseite sollte angezeigt werden und auf der Seite sollte "Web 01" stehen.	Die Webseite wird angezeigt und es steht "Web 01".
Web Server 2	Web Server 2 ereichbar	Der Web Server 2 sollte eine kleine Webseite Anzeigen	Die Webseite sollte angezeigt werden und auf der Seite sollte "Web 02" stehen.	Die Webseite wird angezeigt und es steht "Web 02".
Web Server 3	Web Server 3 ereichbar	Der Web Server 3 sollte eine kleine Webseite Anzeigen	Die Webseite sollte angezeigt werden und auf der Seite sollte "Web 03" stehen.	Die Webseite wird angezeigt und es steht "Web 03".
