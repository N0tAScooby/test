---
title: "SSL Zertifikate installieren (NginxPM) (veraltet)"
slug: /sslzertifikateinstallierennginxpmveraltet
---

# SSL Zertifikate installieren (NginxPM) (veraltet)

Um die SSL Zertifikate zu installieren muss zuerst der NGINX Proxy Manager installiert werden.



Dieser hat den Vorteil, dass alles schön einfach über ein Webinterface konfiguriert werden kann. Neben dem SSL-Zertifikat kann dieser auch weitere Einstellungen vornehmen aber vorerst schauen wir uns in diesem Guide die Installation und die SSL-Zertifikatsanforderung an.




## Voraussetzungen - Portweiterleitung



Damit die entsprechenden Zertifikate erstellt werden können, muss Dein ALARMiator Server über den Port 80 und Port 443 aus dem Internet erreichbar sein. Die Zertifizierungsstelle baut während des Prozesses kurz eine Verbindung zu Deinem Server auf um sicher zu stellen, dass die Anfrage auch berechtig ist.



Um Deinen ALARMiator Server über Port 80 und Port 443 aus dem Internet erreichbar zu machen, musst Du in Deinem Router (der die Internetverbindung herstellt) und ggf. auch in Firewall-Systemen (solltest Du welche betreiben) eine Port-Weiterleitung einrichten. Da dies von Router zu Router unterschiedlich eingerichtet wird, können wir hier an dieser Stelle keine direkte Anleitung geben. Suche in der Oberfläche Deines Router nach Begriffen wie "Port-Forwarding, Port-Weiterleitung, etc.".



Meist gibt man einen Quellport (das ist der aus dem Internet ankommende, hier Port 80 und Port 443), eine IP-Adress des Zielsystems (in unserem Fall Dein ALARMiator Server) und einen Zielport an (in unserem Fall auch wiederum Port 80 und Port 443).




## Erstinstallation



Starte einen SSH-Client, um über die Konsole auf Deinen ALARMiator Sever zugreifen zu können. (Windows - Putty, Mac OS/Linux - Konsole). Melde Dich am Server mit Deinem Benutzer an.



Gehe zu deinem alarmiatorserver Ordner:



```
cd ~/alarmiatorserver
```



Lege hier einen neuen Ordner an für die NGINX Proxy Manager Installation:



```
mkdir dockercontainer
```



Wechsel nun in diesen Ordner:



```
cd dockercontainer
```



Jetzt können wir mit der eigentlichen Installation beginnen:



Führe zunächst folgenden Befehl aus, um dein System zu aktualisieren:



```
sudo apt update && sudo apt upgrade -y
```



Jetzt wollen wir Docker und Portainer installieren. Um uns die Installation zu vereinfachen. Das machen wir mit folgenden Befehlen:



```
curl -fsSL https://get.docker.com -o get-docker.sh
```



```
sudo sh get-docker.sh
```



Jetzt wollen wir eine neue Gruppe "docker" anlegen und unseren root-user in die Gruppe aufnehmen:



```
sudo groupadd docker | sudo usermod -aG docker ```root-user```
```



Erstze hier ```root-user``` mit deinem Root user. Das ist bei einem Raspberry Pi z.B "pi". Bei anderen Linux Distros ist dies der User den du zuerst bei der Installation deines Betriebssystems erstellt hast.



Nun starten wir unser System neu, damit Docker auch sicher aktiv ist und orgnungsgemäß läuft:



```
sudo reboot now
```



Dein Server startet sich nun neu. Verbinde dich nach dem Neustart wieder per SSH und wechsle wieder in unseren Installationsordner:



```
cd ~/alarmiatorserver/dockercontainer
```



Nun erstellen wir ein Docker Volume namens "portainer\_data", welches benutzt wird um die Daten des Containers dauerhaft zu speichern:



```
docker volume create portainer_data
```



Wir starten den Container mit folgendem Befehl, da hier "--restart=always" steht wird dein Container auch wieder von alleine starten nach einem reboot:



```
docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```



Jetzt wollen wir docker-compose installieren. Docker-Compose hilft uns dabei Docker Container zu installieren:



```
sudo apt install docker-compose -y
```



Um zu sehen, ob der Portainer Container läuft kann darauf über einen Browser zugegriffen werden. Gebe folgendes in deinem Browser ein:



```
```ip-addresse```:9000
```



Ersetze ```ip-addresse``` mit der Ip-Addresse deines Servers oder alternativ mit dem Hostnamen deines Servers. Den Hostnamen findest du heraus indem du folgendes im SSH-Fenster eingibst:



```
hostname
```



Hat die Installation geklappt siehst du nun folgendes:


![](/img/image-4.png)



Vergebe hier einen Username und ein sicheres Passwort für deinen Portainer.



Nun wollen wir über Portainer unseren NGINX Proxy Manager installieren.



Nach dem Login sind wir im Homescreen:


![](/img/image-6-1024x375.png)



Hier wollen wir auf local klicken um unsere Container verwalten zu können.



Wir klicken links in der Leiste auf "Containers" um zur Übersicht zu gelangen.


![](/img/image-7-1024x209.png)



Hier sehen wir aktuell unseren Portainer Container der am laufen ist. Um unseren NGINX Proxy Manager Container zu erstellen klicken wir auf "Add Container".



Es öffnet sich folgendes Fenster. Hier wurden bereits alle nötigen Einstellungen ausgefüllt:


![](/img/image-47-1024x598.png)



Wir sehen einen Titel, das Docker image "jc21/nginx-proxy-manager" und die Portzuweisungen. Wir mappen unsere localhost Ports auf die Ports des Docker Containers. Jeder Docker Container hat eine eigene IP-Addresse. Ohne dieses Mapping wäre der Container nur unter seiner eigenen Addresse erreichbar und nicht direkt über unseren ALARMiator-Server Hostnamen / IP-Addresse.



Um Ports zuzuweisen zu können musst du auf den grauen Button: "publish a new network port" klicken.



Nun müssen wir noch unsere "Restart Policy" einstellen, um zu gewährleisten, dass unser Container nach reboots von alleine startet.



Dazu scrollen wir runter klicken auf "Restart Policy" und auf "Always":


![](/img/image-18-1024x323.png)



Gibt man kein Volume an, werden die Daten eines Containers nur so lange behalten, wie der Container läuft, damit die Daten einen Container "überleben" können müssen Sie irgendwo auf dem Host abgelegt werden. Dazu gehen wir zu "Volumes" und richten sogenannte Bind-Mounts ein wie auf dem Bild:


![](/img/image-49-1024x191.png)



Statt Volume wählen wir "Bind" und geben den gewünschten Pfad an. Dieser muss absolut angegeben sein am einfachsten ist es du navigierst zu diesem Ordner:



```
cd ~/alarmiatorserver/dockercontainer
```



Gibst hier in die folgendes ein Konsole:



```
pwd
```



In der Konsole wird nun der absolute Pfad zu diesem Ordner angezeigt diesen schreibst du in das Feld wie im Bild und hängst jeweils folgendes an:



```
/dockervolumes/proxymanager/data

und 

/dockervolumes/proxymanager/etc/letsencrypt
```



**Für Nutzer einer VPS / vServers:**



Ihr müsst unter dem Tab "Network" zusätzlich noch den Network Mode auf "host" umstellen, damit der Nginx Container direkt mit eurem Rechner kommunizieren kann. Für Nutzer die einen eigenen Server hinter einem Router betreiben ist dies nicht nötig.


![](/img/image-1024x158.png)



Wenn alles vollständig eingegeben ist klicken wir auf "Deploy Container".



Wir sollten danach unseren neuen Container in der Übersicht sehen. Das Adminpanel vom Proxy Manager liegt auf Port 81.



Diesen können wir direkt unter "Published Ports" anklicken um dorthin zu gelangen:


![](/img/image-8-1024x275.png)

Hier auf 81:81 klicken




## SSL-Zertifikat anfordern



Verbinde dich in deinem Browser mit deinem NGINX Proxy Manager:



```
```ip-addresse```:81
```



Standardlogin bei Erstanmeldung ist:



```
Email:    admin@example.com
Password: changeme
```



Nach Änderung deiner Login Daten solltest du folgendes sehen:


![](/img/image-1024x341.png)



Klicke auf "SSL Certificates" dann auf "Add SSL Certificate":


![](/img/image-1.png)



Trage unter Domain Names deine Domain für deinen ALARMiator ein und drücke Enter.



Gebe nun noch eine Mail an, um dein SSL Zertifikat von Let's Encrypt anzufordern.



Drücke auf Save.



Du hast nun dein SSL Zertifikat angefordert.



## Dashboard für den Proxy Manager auf Raspberry Pi installieren



Wir wollen nun noch ein Dashboard namens "goaccess" zur Übersicht installieren. Das machen wir wieder mit ein paar Klicks über Portainer.



Navigiere zu "App Templates  Custom Templates" links in der Leiste:


![](/img/image-13-1024x224.png)



Klicke auf "Add Custom Template".



Hier wurde bereits alles nötige ausgefüllt:


![](/img/image-14-1024x923.png)



Du musst folgendes copy-pasten:



```
services:
  goaccess:
    image: justsky/goaccess-for-nginxproxymanager:latest
    container_name: goaccess-npm-dashboard
    restart: always
    environment:
        - TZ=Europe/Berlin
        - SKIP_ARCHIVED_LOGS=False #optional   
        - BASIC_AUTH=False #optional
        - BASIC_AUTH_USERNAME=user #optional
        - BASIC_AUTH_PASSWORD=pass #optional                
    ports:
        - '7880:7880'
    volumes:
        - /home/pi/data/logs:/opt/log
```



Nach dem kopieren in das Feld klicke auf "Create Custom Template".



Du siehst dein Template nun in der "Custom Templates" Übersicht. Klicke auf dein Template du siehst folgendes Fenster:


![](/img/image-15-1024x533.png)



Klicke nun auf "Deploy the Stack".



Nachdem wir den Container Deployen ist dieser unter Port 7880 deines Hosts zu finden.



```
```ip-addresse```:7880
```



Er sollte jetzt auch unter deinen Containern aufgelistet sein.



## Dashboard für den Proxy Manager installieren (nicht Raspberry Pi)



Wir wollen nun noch ein Dashboard namens "goaccess" zur Übersicht installieren. Das machen wir wieder mit ein paar Klicks über Portainer.



Navigiere zu deinen Containern:


![](/img/image-9-1024x275.png)



Klicke auf "Add Container.



Hier wurde bereits alles nötige ausgefüllt:


![](/img/image-51-1024x555.png)



Auch hier vergeben wir einen Namen, spezifizieren das Docker Image "xavierh/goaccess-for-nginxproxymanager" und weisen einen Port zu.



Wir stellen die "Restart Policy" auf "Always"ein.



Auch hier brauchen wir einen dauerhaften Speicherort um die Containerdaten zu sichern:



* Hier wählen wir einen Bind Mount aus dazu rechts statt volume Bind anklicken. Dort gibst du den Pfad zu deinen Nginx Proxy Manager Logs an.


![](/img/image-1024x173.png)



Ersetze auch hier den Pfad bis zum "alarmiatorserver" Ordner mit dem Pfad auf deinem Server. Wie bei der vorherigen Installation vom Proxy Manager!



Nachdem wir den Container Deployen ist dieser unter Port 7880 deines Hosts zu finden.



```
```ip-addresse```:7880
```