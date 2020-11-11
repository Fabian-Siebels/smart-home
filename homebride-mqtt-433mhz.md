# Homebride → MQTT → 433MHZ

#### In dieser Anleitung soll es darum gehen per Apple Homekit über MQTT 433MHZ Geräte fernsteuern zu können.

##### by Fabian S.
Datum: 11.11.2020.

### Übersicht
---
![alt text](http://i.epvpimg.com/yykPaab.png "Übersicht")

### Benötigte Software

- NodeJS
- Homebride
  - Plugin
    - MQTTTHING
    - Alexa
- Mosquitto
  
*Test-Software*
- mqttfx

### Installation

1. Raspberry Pi vorbereiten mit einem Update der Repositories und Upgraden auf neue Versionen von Software.
```
curl -sL https://deb.nodesource.com/setup_14.x | sudo bash -

sudo apt update && sudo apt full-upgrade -y
```

1. Um Homebride installieren zu können muss noch zusatz Software hinzugefügt werden. In diesem Fall wird der DNS Dienst Avahi zur Konnektivität der unterschiedlichen Software benötigt.
```
sudo apt-get install libavahi-compat-libdnssd-dev -y
```
3. NodeJS installieren
```
1. Installation
sudo apt install -y nodejs gcc g++ make python net-tools

2. Version Prüfen
node -v

3. NPM Update
sudo npm install -g npm

4. NodeJS Installation fertig!
```

4. Homebridge installieren
```
1. Homebride und das GUI für den Browser installieren
sudo npm install -g --unsafe-perm homebridge homebridge-config-ui-x

2. Damit Homebride auch nach einem Neustart des Systems automatisch startet, muss noch ein Service eingerichtet werden.
sudo hb-service install --user homebridge
```
5. Verbindung zum Webinterface aufbauen
Um auf das Webinterface zugreifen zu können, muss die IP des Raspberrys und der Port `8581` in die Browserzeile eingegeben werden.
`http://<ip_adresse>:8581`

Die Logindaten lauten:
Benutzer: admin
Passwort: admin

Wenn alles funktionert hat, sollte sich nach dem einloggen diese Seite öffnen:
![](http://i.epvpimg.com/5xpPfab.png "Startseite")


6. Mosquitto installieren
Um ein MQTT Netzwerk aufbauen zu können benötigt man eine Server oder sogg. Broker der die Nachrichten in den einzelnen Topics verwaltet. Als einfacher Server dient in diesem Fall Mosquitto
```
sudo apt install mysquitto mosquitto-clients -y
```


#Diese Anleitung befindet sich in Arbeit!!!