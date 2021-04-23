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
                                                |
