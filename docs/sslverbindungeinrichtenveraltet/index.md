---
title: "SSL Verbindung einrichten [veraltet]"
slug: /sslverbindungeinrichtenveraltet
---

# SSL Verbindung einrichten [veraltet]

**WICHTIG:**  Zur Einrichtung der SSL Verbindung gibt es 2 Möglichkeiten. Die folgende Möglichkeit wird ab Version 1.2.0 **nicht** mehr empfohlen, da die Erneuerung der Zertifikate auf diese Weise aufwändiger ist. Stattdessen wird empfohlen es über den [NginX Proxy Manager zu installieren.](https://handbuch.alarmiator.de/?docs=ssl-zertifikate-installieren-nginxpm)



Damit die Verbindung bei Nutzung der Weboberfläche verschlüsselt erfolgt, musst Du die dazu nötigen Zertifikate anfordern und in Betrieb nehmen. Nach der initialen Inbetriebnahme werden die Zertifikate dann automatisch verlängert.




## Voraussetzungen - Portweiterleitung



Damit die entsprechenden Zertifikate erstellt werden können, muss Dein ALARMiator Server über den Port 80 aus dem Internet erreichbar sein. Die Zertifizierungsstelle baut während des Prozesses kurz eine Verbindung zu Deinem Server auf um sicher zu stellen, dass die Anfrage auch berechtig ist.



Um Deinen ALARMiator Server über Port 80 aus dem Internet erreichbar zu machen, musst Du in Deinem Router (der die Internetverbindung herstellt) und ggf. auch in Firewall-Systemen (solltest Du welche betreiben) eine Port-Weiterleitung einrichten. Da dies von Router zu Router unterschiedlich eingerichtet wird, können wir hier an dieser Stelle keine direkte Anleitung geben. Suche in der Oberfläche Deines Router nach Begriffen wie "Port-Forwarding, Port-Weiterleitung, etc.".



Meist gibt man einen Quellport (das ist der aus dem Internet ankommende, hier Port 80), eine IP-Adress des Zielsystems (in unserem Fall Dein ALARMiator Server) und einen Zielport an (in unserem Fall auch wiederum Port 80).



## Erstinstallation



Starte einen SSH-Client, um über die Konsole auf Deinen ALARMiator Sever zugreifen zu können. (Windows - Putty, Mac OS/Linux - Konsole). Melde Dich am Server mit Deinem Benutzer an.



Als erstes muss snap auf Deinem Raspberry installiert werden. Snap ist ein Tool, mittels dem dann in den folgenden Schritten weitere Pakete installiert werden.



```
sudo apt update

sudo apt install snapd
```



Nun müssen ein paar Pakete installiert werden, die für die Erstausstellung der Zertifikate sowie für die dauerhafte automatische Erneuerung der Zertifikate sorgen. Gebe nun bitte folgende Befehle in der Kommandozeile ein und beantworte ggf. gestellte Fragen entsprechend:



```
sudo snap install core; sudo snap refresh core

sudo snap install --classic certbot

sudo ln -s /snap/bin/certbot /usr/bin/certbot

sudo certbot certonly --standalone

sudo chmod 0755 /etc/letsencrypt/{live,archive}

sudo certbot renew --dry-run

sudo sh -c 'printf "#!/bin/sh\npm2 stop /home/```username```/git/ALARMiator-Server/alarmiator_pm2.yml\n"  /etc/letsencrypt/renewal-hooks/pre/alarmiator_server.sh'

sudo sh -c 'printf "#!/bin/sh\npm2 start /home/```username```/git/ALARMiator-Server/alarmiator_pm2.yml\n"  /etc/letsencrypt/renewal-hooks/post/alarmiator_server.sh'

sudo chmod -R 755 /etc/letsencrypt/renewal-hooks/pre/alarmiator_server.sh

sudo chmod -R 755 /etc/letsencrypt/renewal-hooks/post/alarmiator_server.sh```
```



Damit die Zertifikate nun auch vom ALARMiator Server genutzt werden führe bitte im Unterordner `/certificates` innerhalb des ALARMiator Ordners nun folgende Befehle aus:



```
cd ~/alarmiatorserver/certificates
sudo ln -s /etc/letsencrypt/live/```yoururl.xyz```/privkey.pem privkey.pem
sudo ln -s /etc/letsencrypt/live/```yoururl.xyz```/fullchain.pem cert.pem
```



Damit auch die REST API des Servers durch SSL geschützt werden kann, führe nun folgende Befehle in dem entsprechenden Ordner `/plugins/inbound/api/certificares` aus



```
cd ~/alarmiatorserver/plugins/inbound/api/certificates
sudo ln -s /etc/letsencrypt/live/```yoururl.xyz```/privkey.pem privkey.pem
sudo ln -s /etc/letsencrypt/live/```yoururl.xyz```/fullchain.pem cert.pem
```



ersetze dabei yoirurl.xyz durch Deinen Domainnamen, den Du bei der Zertifikatregistrierung angegeben hast.



**Achtung:** Der Server startet nach der Zertifikatgenerierung einmal neu (damit die neu generierten Zertifikate auch genutzt werden).