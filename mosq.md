# MQTT-Dokumentation

**Projektarbeit – IoT Kommunikation**  
**Gruppe 5:** Kilian, Leon, Louis, Luca, Jan  
**Datum:** 08.10.2025

---

## 1. Einleitung

Diese Dokumentation beschreibt die Installation, Konfiguration und Nutzung des Kommunikationsprotokolls **MQTT (Message Queuing Telemetry Transport)**.  
Sie richtet sich an Einsteiger und soll es ermöglichen, eine funktionierende MQTT-Umgebung mit Broker und Client auf unterschiedlichen Betriebssystemen (**Windows** und **Ubuntu**) eigenständig einzurichten.

**Zielsetzung:**  
Nach Abschluss dieser Anleitung sind alle Beteiligten in der Lage, eine funktionsfähige MQTT-Kommunikation aufzubauen und Nachrichten zwischen Geräten im Netzwerk zu übertragen.

---

## 1.1 Was ist MQTT?

**MQTT** ist ein leichtgewichtiges, auf TCP/IP basierendes Kommunikationsprotokoll, das speziell für den Einsatz im **Internet of Things (IoT)** entwickelt wurde.  
Es ermöglicht eine effiziente und zuverlässige Übertragung von Nachrichten zwischen Geräten – auch bei geringer Bandbreite oder instabiler Netzwerkverbindung.

Grundprinzip:
- **Broker:** Vermittelt Nachrichten zwischen Sender (Publisher) und Empfänger (Subscriber).
- **Client:** Gerät oder Anwendung, die Nachrichten sendet oder empfängt.
- **Topic:** Kanal, über den Nachrichten veröffentlicht und empfangen werden.

---

## 2. Durchführung

Im Folgenden wird der Aufbau einer MQTT-Umgebung beschrieben, bestehend aus:
- **PC2 (Broker):** Ubuntu unter Windows Subsystem for Linux (WSL)
- **PC1 (Client):** Windows-System mit Mosquitto-Client

---

## 2.1 PC2 – Einrichtung des Brokers unter WSL Ubuntu

### Schritt 1: Installation von WSL und Ubuntu

PowerShell als Administrator öffnen und ausführen:
```bash
wsl --install
```
Windows neu starten.

Ubuntu aus dem Microsoft Store installieren.

Ubuntu-Konsole starten.

---

### Schritt 2: Mosquitto installieren

```bash
sudo apt update
sudo apt install mosquitto mosquitto-clients -y
```

---

### Schritt 3: Konfiguration für externe Verbindungen

```bash
sudo nano /etc/mosquitto/mosquitto.conf
```

Folgende Zeilen am Ende der Datei ergänzen:

```bash
listener 1883
allow_anonymous true
```

---

### Schritt 4: Mosquitto neu starten

```bash
sudo pkill mosquitto
mosquitto -c /etc/mosquitto/mosquitto.conf -v
```

---

### Schritt 5: IP-Adresse von PC2 ermitteln

```bash
ip addr show
```

→ Notiere die IPv4-Adresse des Netzwerkadapters eth0(z. B. `192.168.1.50`).  
Diese wird später auf PC1 benötigt, um eine Verbindung aufzubauen.

---

### Schritt 6: Port Weiterleitung einrichten

```bash
netsh interface portproxy add v4tov4 listenport=1883 listenadress=0.0.0.0 connectport=1883 connectadress=<Notierte IP adresse aus Schritt 5>
```

---

## 3. Konfiguration der Windows-Firewall auf PC2

- Öffne Windows Defender Firewall über die Suche.
- Wähle **Erweiterte Einstellungen**.
- Gehe zu **Eingehende Regeln → Neue Regel…**
- **Regeltyp:** Port → Weiter.
- **Protokoll:** TCP → Port: 1883 → Weiter.
- **Aktion:** Verbindung zulassen → Weiter.
- **Profile:** Privat (ggf. Domäne/Öffentlich) → Weiter.
- **Name:** MQTT 1883 freigeben → Fertigstellen.

---

## 4. PC1 – Vorbereitung des MQTT-Clients (Windows)

- Lade Mosquitto für Windows herunter und installiere es:  
👉 [https://mosquitto.org/download/](https://mosquitto.org/download/)  
- Installation ausführen.
- Eingabeaufforderung (CMD) als Administrator öffnen.

---

## 5. Test der Kommunikation

### Schritt 1: Subscriber auf PC2 starten (Ubuntu)

```bash
mosquitto_sub -h 192.168.1.50 -t test/topic
```

### Schritt 2: Nachricht von PC1 senden (Windows)

```bash
mosquitto_pub -h 192.168.1.50 -t test/topic -m "Hallo von PC1"
```

✅ Ergebnis:  
Die Nachricht **"Hallo von PC1"** erscheint im Terminalfenster auf PC2.

---

## 6. Zusammenfassung

| Komponente | Aufgabe | Beschreibung |
|------------|--------|--------------|
| **PC2** | Broker | WSL Ubuntu mit Mosquitto |
| **PC1** | Client | Windows-System, verbindet sich mit Broker über Netzwerk-IP |
| **Port** | 1883/TCP | Muss in der Windows-Firewall auf PC2 freigegeben werden |

