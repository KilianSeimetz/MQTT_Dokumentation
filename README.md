# Grundlagen und Funktionsweise von Mosquitto (MQTT)

## 1. Voraussetzungen und Installation von Mosquitto

Bevor Mosquitto installiert werden kann, müssen einige grundlegende Voraussetzungen erfüllt sein.  
Zunächst sollte das verwendete Betriebssystem auf dem neuesten Stand sein, damit alle benötigten Systemkomponenten vorhanden und kompatibel sind. Eine stabile Internetverbindung ist ebenfalls wichtig, da Mosquitto in der Regel aus den offiziellen Softwarequellen heruntergeladen wird.  

Darüber hinaus sind **Administratorrechte** erforderlich, um die Installation durchführen zu können. Der Mosquitto-Broker kann auf verschiedenen Betriebssystemen wie **Windows**, **Linux** oder **macOS** installiert werden. Besonders häufig wird er auf Linux-Systemen oder auf kleinen Einplatinencomputern wie dem **Raspberry Pi** verwendet, da er sehr ressourcenschonend arbeitet.  

Vor der Installation sollte überprüft werden, ob die notwendigen **Netzwerkbedingungen** erfüllt sind. Der Standard-Kommunikationsport für MQTT, über den Mosquitto Daten austauscht, ist **Port 1883**. Wenn dieser durch eine Firewall blockiert ist, muss er freigegeben werden, damit Geräte miteinander kommunizieren können.  

Nach der Installation kann der Dienst so eingerichtet werden, dass er automatisch startet, sobald das System hochgefahren wird. Dadurch steht der Broker jederzeit zur Verfügung, ohne dass er manuell gestartet werden muss.  
Optional lässt sich Mosquitto auch um **Sicherheitsfunktionen** erweitern, etwa durch die Verwendung von Benutzername und Passwort oder durch die Verschlüsselung der Kommunikation.  

---

## 2. Was ist ein Broker und welche Funktion hat er?

Der sogenannte **Broker** ist das zentrale Element im MQTT-System. Seine Hauptaufgabe besteht darin, Nachrichten zwischen verschiedenen Geräten oder Anwendungen zu vermitteln. Diese Geräte werden als **Clients** bezeichnet und können entweder Nachrichten senden oder empfangen.  

Man unterscheidet zwei Arten von Clients:  

- **Publisher:** Geräte oder Anwendungen, die Nachrichten zu bestimmten Themen senden.  
- **Subscriber:** Geräte oder Anwendungen, die Nachrichten zu bestimmten Themen empfangen möchten.  

Der Broker empfängt die Nachrichten der Publisher und leitet sie an alle Subscriber weiter, die sich für das jeweilige Thema angemeldet haben.  
Er übernimmt also die Rolle eines Vermittlers, der sicherstellt, dass Informationen zuverlässig an die richtigen Empfänger weitergegeben werden.  

Ein großer Vorteil dieses Systems ist, dass sich die Geräte untereinander **nicht kennen müssen**. Sie kommunizieren ausschließlich über den Broker.  
Das macht das System sehr **flexibel und skalierbar**, da neue Geräte jederzeit hinzugefügt oder entfernt werden können, ohne dass bestehende Verbindungen geändert werden müssen.  

---

## 3. Publish, Subscribe und Topics

Das MQTT-Protokoll basiert auf dem sogenannten **Publish/Subscribe-Prinzip**.  
Dieses Kommunikationsmodell unterscheidet sich deutlich von herkömmlichen Punkt-zu-Punkt-Verbindungen.  

- **Publish (Veröffentlichen):**  
  Ein Gerät oder eine Anwendung sendet eine Nachricht an den Broker. Diese Nachricht ist immer mit einem bestimmten Thema, dem sogenannten *Topic*, verknüpft.  

- **Subscribe (Abonnieren):**  
  Ein anderes Gerät meldet beim Broker sein Interesse an einem bestimmten Thema an. Immer wenn zu diesem Thema eine neue Nachricht veröffentlicht wird, erhält es automatisch die Information.  

- **Topics (Themen):**  
  Topics sind hierarchisch aufgebaute Bezeichnungen, die ähnlich wie Dateipfade funktionieren.  
  Sie ermöglichen es, Nachrichten gezielt zu kategorisieren und zu strukturieren.  
  Ein Beispiel wäre ein Topic wie `haus/wohnzimmer/temperatur`.  

Durch dieses System können viele Geräte gleichzeitig miteinander kommunizieren, ohne dass jedes Gerät genau wissen muss, an wen es Nachrichten sendet.  
Diese Art der Kommunikation ist besonders im Bereich des **Internets der Dinge (IoT)** sehr beliebt, weil sie einfach, effizient und ressourcenschonend ist.  

Der MQTT-Broker **Mosquitto** sorgt also dafür, dass Informationen zuverlässig zwischen Geräten ausgetauscht werden können – egal, ob es sich um Sensoren, Steuerungen oder Anwendungen handelt.  
Er bildet damit die Grundlage für eine flexible und moderne **Datenkommunikation** im vernetzten Umfeld.  
