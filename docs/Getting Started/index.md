---
title: "Getting Started - Legen wir los"
slug: /gettingstarted
---

Diese Anleitung führt dich Schritt für Schritt durch die Einrichtung deines ALARMiator Servers.

## 📋 Voraussetzungen

Bevor du startest, stelle sicher dass du:
- Einen **Raspberry Pi 4** (empfohlen) oder Linux-Server hast
- Eine **stabile Internetverbindung** verfügbar ist
- Grundkenntnisse in **SSH und Linux** mitbringst (hilfreich, aber nicht zwingend)

→ [Detaillierte Systemanforderungen](../systemanforderungen/index)

## 🎯 Installationsschritte

### 1️⃣ Hardware vorbereiten
- [Raspberry Pi / Linux Server einrichten](../installationraspberrypilinuxserver/index)
- Betriebssystem installieren
- Netzwerkverbindung herstellen

### 2️⃣ ALARMiator Server installieren

**Empfohlen (Docker Compose)**:
- [Installation via Docker Compose](../automatic_ordering/Docker/installationdesserversviadockercomposeempfohlen/index)
- Einfache Updates
- Weniger Wartungsaufwand

**Alternativ (Native Installation)**:
- [Native Installation](../installationalarmiator-servernativ/index)
- Mehr Kontrolle
- Für erfahrene Nutzer

### 3️⃣ Netzwerk & SSL einrichten
1. [Eigene Domain einrichten](../eigenedomaineinrichten/index)
2. [SSL-Zertifikate konfigurieren](../automatic_ordering/Docker/installationdesserversviadockercomposeempfohlen/index#ssl-zertifikate-installieren)
3. [Nginx Proxy Manager absichern](NginxProxyManagerVerschlüsseln) (für VPS/Root-Server)

### 4️⃣ Grundeinstellungen vornehmen
- [Erste Einstellungen nach Installation](../Admin/ersteeinstellungenimadminpanelnachderinstallation/index)
- Servername und Domain festlegen
- Adminbenachrichtigungen konfigurieren
- Berechtigungsgruppen anlegen

### 5️⃣ Stammdaten anlegen
- [Übersicht Stammdaten](../automatic_ordering/Übersichten/bersichtstammdatenanlegen/index)
- Organisation → Standort → Gruppen → Mitglieder → Benutzer

### 6️⃣ Alarmierung aktivieren
- [Übersicht Alarmierung](../automatic_ordering/Übersichten/bersichtalarmierungen/index)
- Alarmwege und -profile einrichten
- KatSys oder andere Alarmquelle anbinden

### 7️⃣ Wallboard einrichten (optional)
- [Übersicht Wallboard](../automatic_ordering/Übersichten/bersichtwallboard/index)
- Raspberry Pi Zero als Wallboard-Client
- Widgets konfigurieren

### 8️⃣ Spaceport Client (optional)
- [Übersicht Spaceport](../automatic_ordering/Übersichten/bersichtspaceport/index)
- Abgesetzte Standorte anbinden

## 🔄 Nach der Installation

### Regelmäßige Aufgaben
- [Backups erstellen](../Admin/backupdeinesserverserstellen/index)
- [Server aktualisieren](../updatedesalarmiatorservers/index)
- Logs überprüfen

### Erweiterte Konfiguration
- [Plugins aktivieren](../plugins/index)
- [Email-Vorlagen anpassen](../automatic_ordering/Seiten/email-verarbeitung/index)
- [Berechtigungen verwalten](../Admin/berechtigungsgruppenanlegen/index)

## 🆘 Probleme?

- [Docker Troubleshooting](../automatic_ordering/Docker/dockertroubleshooting/index)
- [Allgemeine Probleme](../externeipadressewirdnichtaktualisiert/index)
- [Community Forum](https://community.alarmiator.de)

## 📚 Weiterführende Links

- [ALARMiator ausprobieren (PROBIERiator)](../manual_ordering/ALARMiator%20Probieren/index)
- [Alternative Reverse Proxies](alternativereverseproxies/index)
- [Von nativer zu Docker Compose migrieren](../automatic_ordering/Docker/vonnativerinstallationzudockercomposemigrieren/index)

---

**Tipp**: Arbeite die Schritte nacheinander ab und teste nach jedem Schritt, ob alles funktioniert. Bei Fragen hilft dir unsere [Community](https://community.alarmiator.de) gerne weiter!