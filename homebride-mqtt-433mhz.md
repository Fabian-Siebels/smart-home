# Homebride → MQTT → 433MHZ

#### In dieser Anleitung soll es darum gehen per Apple Homekit über MQTT 433MHZ Geräte fernsteuern zu können.

##### by Fabian S.
Datum: 11.11.2020.

### Übersicht
---
![alt text](http://i.epvpimg.com/yykPaab.png "Übersicht")

### Benötigte Hardware
- Raspberry Pi
- 433MHZ Sender/Empfänger
- Jumper Kabel

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
4. NodeRED installieren
NodeRED per npm installieren
```
sudo npm install -g node-red
```
Dies dauert einige Zeit. Als nächstes wird soch ein Service erstellt, damit auch nach einem Neustart alles geladen wird.
```
sudo wget https://raw.githubusercontent.com/node-red/raspbian-deb-package/master/resources/nodered.service -O /lib/systemd/system/nodered.service

sudo systemctl enable nodered.service

sudo systemctl start nodered.service

sudo systemctl status nodered.service
```

Bei `nodered.service` muss `aktive(running)` stehen, damit alles geklappt hat.

5. Homebridge installieren
```
1. Homebride und das GUI für den Browser installieren
sudo npm install -g --unsafe-perm homebridge homebridge-config-ui-x

2. Damit Homebride auch nach einem Neustart des Systems automatisch startet, muss noch ein Service eingerichtet werden.
sudo hb-service install --user homebridge
```
6. Verbindung zum Webinterface aufbauen
Um auf das Webinterface zugreifen zu können, muss die IP des Raspberrys und der Port `8581` in die Browserzeile eingegeben werden.
`http://<ip_adresse>:8581`

Die Logindaten lauten:
Benutzer: admin
Passwort: admin

Wenn alles funktionert hat, sollte sich nach dem einloggen diese Seite öffnen:
![](http://i.epvpimg.com/5xpPfab.png "Startseite")

6.1 Plugins installieren
In den Tab Plugins wechseln und Homebride Mqttthing installieren:
Einstellungen -> Hinzufügen -> Type=Light buldb - on/off -> Name=Rolladen -> MQTT Topics -> Set On=`Später der CODE`


7. Mosquitto installieren
Um ein MQTT Netzwerk aufbauen zu können benötigt man eine Server oder sogg. Broker der die Nachrichten in den einzelnen Topics verwaltet. Als einfacher Server dient in diesem Fall Mosquitto
```
sudo apt install mysquitto mosquitto-clients -y
```

8. MQTT testen (Windows)
Mit dem kostenlosen Tool mqtt.fx kann man über einen Windows Computer den MQTT Server einfach testen.
`Download: https://mqttfx.jensd.de/`

9. NodeRED im Browser öffnen

IP: `http://<ip_adresse>:1880`

Oben Rechts in der Ecke auf die 3 Striche Tippen und folgenden Code importieren
Nachdem wieder in das Menu und auf Palette verwalten tippen und das Modul `node-red-contrib-rpi-433` installieren

```
[{"id":"2a1f30bf.3e0b78","type":"tab","label":"Flow 1","disabled":false,"info":""},{"id":"b0cab10e.bd4808","type":"mqtt in","z":"2a1f30bf.3e0b78","name":"","topic":"/home/rollade","qos":"2","datatype":"auto","broker":"cca811e2.2c2c08","x":110,"y":120,"wires":[["4d99d146.c97d58","9a137cb0.799038"]]},{"id":"4d99d146.c97d58","type":"rpi-433-emitter","z":"2a1f30bf.3e0b78","pin":17,"pulseLength":350,"protocol":1,"x":320,"y":120,"wires":[]},{"id":"798a9e37.b50b08","type":"rpi-433-sniffer","z":"2a1f30bf.3e0b78","pin":17,"pulseLength":0,"debounceDelay":500,"x":110,"y":240,"wires":[["9a137cb0.799038"]]},{"id":"9a137cb0.799038","type":"debug","z":"2a1f30bf.3e0b78","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","statusVal":"","statusType":"auto","x":310,"y":240,"wires":[]},{"id":"83c5bc39.e3ffb8","type":"comment","z":"2a1f30bf.3e0b78","name":"MQTT -> 433MHZ Sender","info":"MQTT -> 433MHZ Sender","x":210,"y":80,"wires":[]},{"id":"64894053.d81938","type":"comment","z":"2a1f30bf.3e0b78","name":"433MHZ Empfänger -> Debug Ausgabe","info":"","x":210,"y":200,"wires":[]},{"id":"cca811e2.2c2c08","type":"mqtt-broker","name":"","broker":"172.16.0.253","port":"1883","clientid":"","usetls":false,"compatmode":false,"keepalive":"60","cleansession":true,"birthTopic":"","birthQos":"0","birthPayload":"","closeTopic":"","closeQos":"0","closePayload":"","willTopic":"","willQos":"0","willPayload":""}]
```
Im nächsten Schritt muss der MQTT Broker bei `home/rollade` eingegeben werden, hierbei Doppelklickt man zweimal auf dem Baustein und gibt bei der Server Adresse `127.0.0.1:1883` ein. 

#Diese Anleitung befindet sich in Arbeit!!! 29.11.2020