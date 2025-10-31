---
title: "Übersicht Alarmierung"
slug: /uebersichtalarmierungen
---

Um die Alarmierungsfunktion von ALARMiator nutzen zu können, müssen mehrere Komponenten eingerichtet werden. Diese Anleitung führt dich Schritt für Schritt durch den Prozess.

## 📋 Voraussetzungen

Bevor du mit der Alarmierungseinrichtung beginnst, sollten bereits vorhanden sein:
- ✅ Mindestens eine [Organisation](../eineorganisationanlegen/index)
- ✅ Mindestens ein [Standort](../einenstandortanlegen/index)
- ✅ Mindestens eine [Gruppe](../gruppenanlegen/index)
- ✅ Mindestens ein [Mitglied](../mitgliederanlegen/index)
- ✅ Zugehöriger [Benutzer](../benutzeranlegen/index) für das Mitglied

→ Falls noch nicht geschehen: [Stammdaten-Übersicht](../automatic_ordering/Übersichten/bersichtstammdatenanlegen/index)

## 🔧 Schritt-für-Schritt Einrichtung

### 1. Plugins aktivieren

Aktiviere im Admin-Bereich folgende Plugins:

- **REST-API Plugin** - Grundlegende API-Funktionen
- **ALARMiator Mobile Plugin** - Alarmierung über Mobile Apps

→ [Plugins verwalten](../plugins/index)

### 2. Schleifen anlegen

Schleifen verbinden Alarmcodes mit lesbaren Bezeichnungen und steuern das Wallboard.

→ [Schleifen anlegen](../schleifenanlegen/index)

**Beispiel:**
- Schleifencode: `12345`
- Bezeichner: `FFW Musterstadt - Vollalarm`

### 3. Alarmwege konfigurieren

Lege fest, welche Mitglieder über welche Wege alarmiert werden können.

→ [Alarmwegematrix einstellen](../Alarmierung/alarmwegematrixeinstellen/index)

**Verfügbare Alarmwege:**
- Mobile App (Push-Benachrichtigung)
- SMS
- E-Mail
- Webhook

**Wichtig:** Für jeden Alarmweg muss das entsprechende Plugin aktiviert sein!

### 4. Alarmdaten konfigurieren

Bestimme, welche Einsatzinformationen an welche Gruppen übermittelt werden.

→ [Alarmdatenmatrix einstellen](../Alarmierung/alarmdatenmatrixeinstellen/index)

**Beispiel-Datenfelder:**
- Einsatzstichwort
- Einsatzort
- Mitteilung Disponent
- Alarmierte Einsatzmittel
- Karte

### 5. Alarmierungsprofile erstellen

Alarmierungsprofile legen fest, welche Gruppen bei welchen Schleifen alarmiert werden.

→ [Alarmierungsprofile anlegen](../Alarmierung/alarmierungsprofileanlegen/index)

**Beispiel-Profil:**
```
Wenn Schleife "12345" empfangen wird:
  → Alarmiere Gruppe "Einsatzabteilung"
  → In Organisation "FFW Musterstadt"
```

### 6. Endgeräte freischalten

Neue Endgeräte müssen vom Administrator freigegeben werden, bevor sie Alarmierungen empfangen können.

→ [Endgeräte aktivieren](../endgerteaktivieren/index)

**Ablauf:**
1. Mitglied meldet sich erstmalig in der App an
2. Endgerät erscheint in der Endgeräte-Liste
3. Administrator aktiviert das Endgerät
4. Mitglied kann nun Alarmierungen empfangen

## 📥 Alarmquellen einrichten

Wähle eine oder mehrere Alarmquellen:

### KatSys Cloud Connector (empfohlen)
→ [KatSys Cloud Connector einrichten](../katsyscloudconnector/index)

**Vorteile:**
- Direktanbindung an Leitstelle
- Automatische Alarmübernahme
- Statusmeldungen

**Voraussetzungen:**
- KatSys-Zugang (Master Token, Sub Token, PEM-Datei)
- Freischaltung durch Leitstelle

### BosMon
→ [Alarmierung mit BosMon](../Alarmierung/alarmierungmitbosmon/index)

**Vorteile:**
- Dekodierung von ZVEI, FMS, POCSAG
- Lokale Funkmelder-Anbindung

**Voraussetzungen:**
- Kostenpflichtige BosMon-Version
- API-Token von ALARMiator

### Manuelle Alarmierung
→ [Manuelle Alarmierung auslösen](../Alarmierung/manuellealarmierungauslsen/index)

**Anwendungsfälle:**
- Keine Leitstellenanbindung
- Werksfeuerwehren
- Störung der Leitstellenanbindung
- Tests und Übungen

## ✅ Installation testen

### 1. Probealarm konfigurieren
→ [Zeitgesteuerte Probealarme](../Alarmierung/zeitgesteuerteprobealarmeanlegen/index)

### 2. Test-Alarmierung auslösen
- [Manuelle Alarmierung](../Alarmierung/manuellealarmierungauslsen/index) nutzen
- Oder Probealarm über Mobile App anfordern

### 3. Prüfen
- ✅ Alarm kommt auf Mobile App an?
- ✅ Rückmeldung funktioniert?
- ✅ Richtige Daten werden angezeigt?
- ✅ Navigation zum Einsatzort möglich?

## 🔍 Troubleshooting

### Alarm kommt nicht an
→ [Es erfolgt keine Alarmierung](../Alarmierung/eserfolgtkeinealarmierung/index)

**Häufige Ursachen:**
- Internetverbindung unterbrochen
- Endgerät nicht freigegeben
- Alarmierungsregeln falsch konfiguriert
- Akkuoptimierung nicht deaktiviert (Android)
- Kritische Hinweise nicht aktiviert (iOS)

### Weitere Probleme
- [Endgeräte aktivieren](../endgerteaktivieren/index)
- [FCM Tokens prüfen](../fcmtokens/index)
- [Community Forum](https://community.alarmiator.de)

## 📱 Mobile App einrichten

Nach erfolgreicher Server-Konfiguration:

1. **Android**: [Android-Guide](../docs-mobile/androidguidefrbenutzer/index)
2. **iOS**: [iOS-Guide](../docs-mobile/iosguidefrbenutzer/index)

## 🎓 Weiterführende Themen

- [Alarmdatenmatrix Details](../Alarmierung/alarmdatenmatrixeinstellen/index)
- [Alarmwegematrix Details](../Alarmierung/alarmwegematrixeinstellen/index)
- [KatSys Cloud Connector](../katsyscloudconnector/index)
- [API-Tokens verwalten](../Alarmierung/alarmierungmitbosmon/index#einrichtung-api-token)

---

**Tipp:** Teste die Alarmierung zunächst mit einer kleinen Testgruppe, bevor du sie produktiv nutzt!