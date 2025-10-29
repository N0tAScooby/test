---
title: "Reverse Proxy einrichten [veraltet]"
slug: /reverseproxyeinrichtenveraltet
---

# Reverse Proxy einrichten [veraltet]

Wir wollen das Web-UI, die REST-API, unseren Wallboardzugang und den Spaceport Socket hinter einen Reverse Proxy schalten. Um dies zu tun muss der NGINX Proxy Manager installiert sein. Wie das geht wurde im Guide zur SSL-Zertifikatseinrichtung gezeigt.



## Was ist ein Reverse Proxy?



Ein Reverse-Proxy sitzt vor einem Webserver und empfängt alle Anfragen, bevor sie den Server erreichen. Er stellt sicher, dass kein Client direkt mit dem Server kommuniziert.


![](/img/image-19.png)



Für unseren Zweck stellt, er sicher, dass alle Anfragen per SSL verschlüsselt sind und lässt uns einen Zugangspunkt zu unseren verschiedenen Anwendungen erstellen.



## Reverse Proxy einrichten



Logge dich in deinem NGINX Proxy Manager ein.



```
```ip-addresse```:81
```



Ersetze ```ip-addresse``` mit der Ip-Addresse deines Servers oder alternativ mit dem Hostnamen deines Servers. Den Hostnamen findest du heraus indem du folgendes im SSH-Fenster eingibst:



```
hostname
```


![](/img/image-20-1024x375.png)



Navigiere zu "Proxy Hosts" und klicke auf "Add Proxy Host".


![](/img/image-1.png)



Trage deine Domain ein und ersetze "hostname" mit dem Hostnamen deines ALARMiator Servers.



WICHTIG: Localhost oder 127.0.01 wird nicht akzeptiert es muss der Hostname oder die IP angegeben werden.



Klicke nun auf "Custom Locations".


![](/img/image-52.png)



Achte auch auf den extra / für die Api Location!



Um den Spaceport Client über den Proxy anzusprechen richte folgende Custom-Location ein:


![](/img/image.png)



Ersetze auch hier wieder "hostname".



Achte außerdem auf den Slash hinter "hostname" bei der API Zuweisung.



Nun wollen wir noch ein SSL-Zertifikat einrichten. Klicke dazu auf "SSL".


![](/img/image-8.png)



Force SSL und HSTS dienen beide der Erhöhung der Sicherheit und sollte gesetzt sein.



Wähle dein SSL-Zertifikat aus, dass du bereits wie im Guide dazu erstellt hast.



Nun hast du deinen Reverse Proxy eingerichtet allerdings wissen die Endgeräte deiner Nutzer nicht, dass Sie jetzt Anfragen über diesen senden sollen. Um das zu ändern gehe im Adminpanel deines Alarmiatorservers zum REST-API Plugin. Hier kannst du die Proxy Domain angeben.


![](/img/image-2-1024x157.png)



Hier musst du die URL angeben über welche die Endgeräte kommunizieren sollen, also die des Proxy z.B "https://example.com:443/api".



Wir geben hier explizit Port 443 an um über https zu kommunizieren.



Diese wird den Endgeräten mitgeteilt und Sie werden automatisch auf diese URL migriert, sobald Sie mit dem Server kommunizieren.



Damit die automatische Migrierung funktioniert muss natürlich der alte Kommunikationsweg noch bestehen, um die neue URL überhaupt zu bekommen. Deshalb muss für eine gewisse Zeit der Reverse Proxy und der alte Standardweg aktiv sein, bis sich alle Endgeräte einmal "gemeldet" haben. Danach kann der alte Weg abgestellt werden und der Port 5010 oder 5443 geschlossen werden, da wir ja nun Port 443 des Proxy Managers benutzen um intern auf Port 5010 des ALARMiator Server weiterzuleiten.



Als Nutzer kann man in den Profileinstellungen in der App sehen welche URL hinterlegt ist um nachzuprüfen welchen Kommunikationsweg man (wie das geht findest du im Guide zu den Handyapps)



## Wie funktionieren die Weiterleitungen?




| Domain | Service | Port |
| --- | --- | --- |
| example.com | Web-UI | 5000 |
| example.com/api/ | Rest-API | 5010 |
| example.com/socket.io/ | Wallboard | 5020 |
| example.com/spaceport/socket.io/ | Spaceport-Client | 5555 |




Hier siehst du wie du nun auf die verschiedenen Services zugreifen kannst, wenn du Sie an deinem Server aktiviert hast.



Die socket.io Verbindungen sind für die Datenübermittlung an das Wallboard und den Spaceport erforderlich. Die Wallboard Seite selbst wird über die Api angezeigt. DIe Daten zur Befüllung der WIdget werden im Hintergrund über die socket.io Verbindung geladen. Es muss also für ein funktionierendes Wallboard von außen sowohl die API als auch der Wallboardsocket erreichbar sein.