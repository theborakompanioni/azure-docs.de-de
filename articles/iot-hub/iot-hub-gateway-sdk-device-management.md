<properties
	pageTitle="Geräteverwaltung mit dem Gateway SDK | Microsoft Azure"
	description="Exemplarische Vorgehensweise zum Azure IoT Hub Gateway SDK, mit der veranschaulicht wird, wie Sie die Geräteverwaltung bei Verwendung des Gateway SDK implementieren."
	services="iot-hub"
	documentationCenter=""
	authors="dominicbetts"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>


# IoT Gateway SDK (Beta) – Geräteverwaltung mit dem Gateway SDK

In diesem Tutorial wird gezeigt, wie Sie die Funktionen der [Geräteverwaltung][lnk-device-management] von IoT Hub zusammen mit dem [Azure IoT Hub Gateway SDK][lnk-gateway-sdk] verwenden. Im Tutorial wird ein Intel Edison Compute Module zum Hosten eines Gateways mit simulierten Geräten verwendet, das Telemetriedaten an Ihren IoT Hub sendet. Sie verwenden die Funktionen für die Geräteverwaltung in IoT Hub, um auf dem Edison Board ein Firmwareupdate per Remotezugriff durchzuführen.

In diesem Lernprogramm wird Folgendes behandelt:

- **Architektur**: Wichtige Architekturinformationen zum Beispiel für die Geräteverwaltung.

- **Erstellen und Ausführen**: die zum Erstellen und Ausführen des Beispiels erforderlichen Schritte.

## Architektur

In diesem Tutorial stellen Sie ein Intel Edison Board als IoT-Gerätegateway bereit, das mit IoT Hub verbunden ist. Außerdem konfigurieren Sie das Edison Board so, dass Sie es per IoT Hub verwalten können. Im folgenden Diagramm sind die wichtigsten Elemente der Lösung dargestellt, die Sie in diesem Tutorial erstellen:

![Architektur für Gateway und Geräteverwaltung][img-architecture]

### Geräte

In dieser Projektmappe sind drei IoT-Geräte mit IoT Hub verbunden:

- Das Edison Board ist ein Gerät mit dem Namen **GW-device**. Im Tutorial nutzen Sie die Geräteverwaltungsfunktionen von IoT Hub, um das Update für die Firmware dieses physischen Geräts durchzuführen.

- Zwei simulierte Geräte (**GW-ble1-demo** und **GW-ble2-demo**). Mit diesen Geräten werden Niedrigenergiegeräte mit Bluetooth-Verbindung (Bluetooth Low Energy, BLE) simuliert, bei denen die Verbindung mit IoT Hub über das Gateway hergestellt wird und Temperatur-Telemetriedaten an den Hub gesendet werden.

### Gatewaysoftware

Die Gatewaysoftware wird als Dienst auf dem Edison Board ausgeführt. Zwei simulierte Geräte generieren die Telemetriedaten für die Temperatur. Das Zuordnungsmodul ordnet diese simulierten Geräte den Geräten zu, die unter IoT Hub registriert sind, und das IoT Hub-Modul übernimmt die Kommunikation mit dem IoT Hub-Endpunkt. Im Artikel [IoT Gateway SDK – Senden von D2C-Nachrichten mit einem simulierten Gerät][lnk-gateway-scenario] wird dieses Szenario ausführlich beschrieben.

### Geräteverwaltungsclient

Der [Geräteverwaltungsclient][lnk-device-management] wird als Dienst auf dem Edison Board ausgeführt. Darüber können Sie Remoteaufträge auf dem Edison Board ausführen, z.B. [Firmwareupdates][lnk-dm-jobs], Neustarts und Konfigurationsupdates, und Geräteeigenschaften abfragen. In diesem Tutorial empfängt und verarbeitet der Geräteverwaltungsclient eine Anforderung, um für das Edison Board ein Firmwareupdate durchzuführen.

### IoT Hub

[Azure IoT Hub][lnk-iot-hub] ist ein vollständig verwalteter Dienst, der eine zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und einem Lösungs-Back-End ermöglicht. In diesem Tutorial wird per IoT Hub Folgendes durchgeführt:

- Ermöglichen der Verbindung mit der Cloud für die drei Geräte
- Empfangen der von den simulierten Geräten generierten Telemetriedaten vom Gateway
- Senden einer Firmwareupdate-Anforderung an das Edison Board
- Überwachen des Status des Firmwareupdate-Auftrags

### Beispiel-UI für die Geräteverwaltung

Die [Beispiel-UI für die Geräteverwaltung][lnk-dm-sample-ui] ist eine Beispielwebanwendung, mit der Sie die Geräteverwaltungsfunktionen von IoT Hub in einer interaktiven Web-UI nutzen können. Beispielsweise können Sie die Beispiel-UI zum Abfragen der Geräte verwenden, die mit IoT Hub verbunden sind, und Firmwareupdate-Aufträge an Ihre Geräte senden. In diesem Tutorial verwenden Sie die Beispiel-UI, um einen Firmwareupdate-Auftrag an das Edison Board zu senden und den Status des Auftrags bis zu seinem Abschluss zu überwachen.

## Erstellen und Ausführen

Zum Ausführen dieses Beispiels müssen Sie ein benutzerdefiniertes Image für das Edison Board erstellen, das die IoT Hub-Gatewaysoftware und den Geräteverwaltungsclient enthält.

Bevor Sie beginnen, sollten Sie sicherstellen, dass Sie Ihr Edison Board mit dem Drahtlosnetzwerk verbinden können. Um Ihr Edison Board einzurichten, müssen Sie es mit einem Hostcomputer verbinden. Später verwenden Sie den Hostcomputer, um das Edison Board mit dem erstellten benutzerdefinierten Image zu aktualisieren. Intel stellt eine Reihe von Anleitungen für die ersten Schritte bereit, z.B. auch für die folgenden Betriebssysteme:

- [Get Started with the Intel Edison Development Board on Windows 64-bit][lnk-setup-win64] \(Erste Schritte mit dem Intel Edison Development Board unter Windows 64-Bit).
- [Get Started with the Intel Edison Development Board on Windows 32-bit][lnk-setup-win32] \(Erste Schritte mit dem Intel Edison Development Board unter Windows 32-Bit).
- [Getting Started with the Intel® Edison Board on Linux][lnk-setup-linux] \(Erste Schritte mit dem Intel® Edison Board unter Linux).

Um das Edison Board einzurichten und sich damit vertraut zu machen, ist es ratsam, alle Schritte in diesen Artikeln auszuführen, jedoch mit Ausnahme von:

- Flashing the latest firmware (Aktualisieren der Firmware). Sie aktualisieren die Firmware im Rahmen dieses Tutorials, sodass Sie diesen Schritt nicht vorher ausführen müssen.
- Auch der letzte Schritt „Choose IDE“ (Auswählen der IDE) ist für das aktuelle Tutorial nicht erforderlich.

Nachdem Sie das Edison Board eingerichtet und die benötigten Treiber auf dem Hostcomputer installiert haben, sollten Sie sicherstellen, dass Sie mit einem seriellen Terminal eine Verbindung mit dem Edison Board herstellen können. Die Seite [Setting up a serial terminal][lnk-serial-connection] \(Einrichten eines seriellen Terminals) auf der Intel-Website enthält Links zu Einrichtungsanleitungen für Hostbetriebssysteme, z.B. Windows und Linux.

Außerdem müssen Sie folgende Aufgaben durchführen:

- [Erstellen Sie einen IoT Hub][lnk-create-hub] in Ihrem Azure-Abonnement. Sie benötigen für dieses Tutorial den Namen des Hubs. Wenn Sie nicht bereits über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto erstellen][lnk-free-trial].
- Fügen Sie Ihrem IoT Hub drei Geräte hinzu (**GW-ble1-demo**, **GW-ble2-demo** und **GW-device**), und notieren Sie sich die IDs und Geräteschlüssel. Sie können die Tools [Geräte-Explorer oder iothub-explorer][lnk-explorer-tools] verwenden, um diese Geräte dem im vorherigen Schritt erstellten IoT Hub hinzuzufügen und die zugehörigen Schlüssel abzurufen. Sie verwenden zwei dieser Geräte (**GW-ble1-demo** und **GW-ble2-demo**) als simulierte BLE-Geräte, die mit dem Gateway verbunden sind, und ein Gerät (**GW-device**) zum Identifizieren des Edison-Gatewaygeräts als Geräteverwaltungsclient, den Sie über Ihren IoT Hub verwalten können.

### Vorbereiten der Buildumgebung und Sicherstellen, dass ein benutzerdefiniertes Image erstellt werden kann

Zum Erstellen eines benutzerdefinierten Image für das Edison Board benötigen Sie eine Linux-Umgebung. Bei diesen Schritten wird vorausgesetzt, dass Sie Ubuntu 14.04 verwenden. Dies ist zum Zeitpunkt der Erstellung dieses Texts die empfohlene Plattform. Auf Ihrer home-Partition sollten mindestens 40 GB freier Speicherplatz vorhanden sein.

> [AZURE.NOTE] Die Ausführung des Skripts, mit dem das benutzerdefinierte Image erstellt wird, kann auf einem Computer mit vier CPU-Kernen bis zu sechs Stunden dauern. Sie können diesen Zeitraum verringern, indem Sie einen leistungsfähigeren Computer mit mehr CPU-Kernen verwenden.

Für die Schritte in diesem Abschnitt haben wir die folgenden Artikel herangezogen: [Intel Edison Board Support Package][lnk-inteledison-bsp] \(Intel Edison Board-Supportpaket), [Manually Building Yocto Images for the Intel Edison Board from Source][lnk-hackgnar] \(Manuelles Erstellen von Yocto-Images für das Intel Edison Board an der Quelle) und [Creating a Custom Linux Kernel for the Edison (release 2.1)][lnk-shawnhymel] \(Erstellen eines benutzerdefinierten Linux-Kernels für Edison (Version 2.1)).

1. Melden Sie sich an Ihrem Ubuntu 14.04-Computer an, und führen Sie den folgenden Befehl in Ihrem Ordner „home“ aus, um das Edison-Quellpaket herunterzuladen:
    
    ```
    curl -O http://downloadmirror.intel.com/25028/eng/edison-src-ww25.5-15.tgz
    ```

2. Dekomprimieren Sie das Quellpaket, um mit dem folgenden Befehl den Ordner **edison-src** in Ihrem Ordner „home“ zu erstellen, und navigieren Sie dann zum neuen Ordner **edison-src**:
    
    ```
    tar xfvz edison-src-ww25.5-15.tgz
    cd edison-src/
    ```

3. Installieren Sie die Pakete mit den erforderlichen Komponenten:
    
    ```
    sudo apt-get -y install build-essential git diffstat gawk chrpath texinfo libtool gcc-multilib libsdl1.2-dev u-boot-tools
    ```

4. Erstellen Sie die beiden neuen Verzeichnisse im Ordner **edison-src**:
    
    ```
    mkdir bitbake_download_dir
    mkdir bitbake_sstate_dir 
    ```

5. Führen Sie das folgende Skript aus, um Ihre Buildumgebung herunterzuladen. Wenn Sie über mehr als vier CPU-Kerne verfügen, sollten Sie die Skriptargumente **--parallel\_make** und **--bb\_number\_thread** auf die Anzahl von Kernen festlegen, die Ihnen zur Verfügung stehen:
    
    ```
    ./meta-intel-edison/setup.sh --dl_dir=bitbake_download_dir  --sstate_dir=bitbake_sstate_dir 
    ```

6. Aktualisieren Sie den Speicherort des Paho-Git-Repositorys. Bearbeiten Sie die Datei **~/edison-src/out/linux64/poky/meta-intel-iot-middleware/recipes-connectivity/paho-mqtt/paho-mqtt\_3.1.bb**, und ersetzen Sie die URL `git://git.eclipse.org/gitroot/paho/org.eclipse.paho.mqtt.c.git/` durch `git://github.com/eclipse/paho.mqtt.c.git`.

7. Initialisieren Sie die Buildumgebung mit den folgenden Befehlen:
    
    ```
    cd out/linux64/
    source poky/oe-init-build-env
    ```

8. Verwenden Sie den folgenden Befehl, um das benutzerdefinierte Image zu erstellen. Die erste Ausführung dieses Befehls kann auf einem Computer mit vier CPU-Kernen bis zu sechs Stunden dauern. Die nachfolgenden erneuten Erstellungen nach dem Hinzufügen Ihrer Anpassungen laufen dann deutlich schneller ab:
    
    ```
    bitbake edison-image
    ```

9. Schließen Sie den Buildvorgang ab, indem Sie die folgenden Befehle ausführen:
    
    ```
    cd ~/edison-src/
    ./meta-intel-edison/utils/flash/postBuild.sh ./out/linux64/build/
    ```

Die Dateien, die Sie zum Aktualisieren des Edison Boards benötigen, befinden sich jetzt im Ordner **~/edison-src/out/linux64/build/toFlash/**.

### Hinzufügen des Gateway-SDK zu Ihrem benutzerdefinierten Image

In den Schritten dieses Abschnitts wird das Hinzufügen des Gateway-SDK zu Ihrem benutzerdefinierten Image beschrieben, damit das Edison Board nach dem Starten als IoT-Gateway fungieren kann. Für dieses Tutorial enthält das Gateway ein Modul, mit dem zwei BLE-Geräte (Bluetooth Low Energy) simuliert werden, die Temperatur-Telemetriedaten für das Gateway zum Weiterleiten an Ihren IoT Hub generieren.

Führen Sie die folgenden Schritte auf demselben Ubuntu 14.04-Computer aus, den Sie im vorherigen Abschnitt zum Erstellen eines Edison-Image verwendet haben.

1. Klonen Sie das Gateway-SDK in Ihrem Ordner „home“:
    
    ```
    cd ~
    git clone https://github.com/Azure/azure-iot-gateway-sdk.git --recursive
    ```

2. Konfigurieren Sie das Gateway, um eine Verbindung mit Ihrem IoT Hub herzustellen und zwei Geräte zu simulieren. Bearbeiten Sie die Datei **~/azure-iot-gateway-sdk/samples/simulated\_device\_cloud\_upload/src/simulated\_device\_cloud\_upload\_intel\_edison.json**, und ersetzen Sie die Platzhalter **IoTHubName**, **IoTHubSuffix**, **deviceID** und **deviceKey** durch die Werte, die Sie sich beim Konfigurieren des IoT Hub notiert haben. Verwenden Sie die Geräte **GW-ble1-Demo** und **GW-ble2-Demo**, die Sie zuvor erstellt haben.

3. Erstellen Sie einen Ordner für Ihr neues Rezept:
    
    ```
    mkdir ~/edison-src/meta-intel-edison/meta-intel-edison-distro/recipes-support/azure-iot-field-gateway-sdk
    ```

4. Kopieren Sie die Rezeptdatei mit dem Namen **azure-iot-field-gateway-sdk.bb** aus dem Ordner **~/azure-iot-gateway-sdk/samples/simulated\_device\_cloud\_upload/src/** in den gerade erstellten Ordner **~/edison-src/meta-intel-edison/meta-intel-edison-distro/recipes-support/azure-iot-field-gateway-sdk/**. Bearbeiten Sie die Datei, und ersetzen Sie die beiden Vorkommen von `<<userName>>` durch Ihren derzeitigen Benutzernamen.

5. Bearbeiten Sie die Datei **~/edison-src/meta-intel-edison/meta-intel-edison-distro/recipes-core/images/edison-image.bb**, um einen Eintrag für Ihr neues Rezept hinzuzufügen. Fügen Sie am Ende der Datei die folgende Zeile hinzu:
    
    ```
    IMAGE_INSTALL += "azure-iot-field-gateway-sdk"
    ```

6. Sie können die Änderungen jetzt testen, indem Sie den Befehl **bitbake** ausführen, um nur das neue Rezept zu erstellen:
    
    ```
    cd ~/edison-src/out/linux64/
    source poky/oe-init-build-env
    bitbake azure-iot-field-gateway-sdk
    ```

### Hinzufügen des Azure IoT Hub-Geräteverwaltungsclients zum benutzerdefinierten Image

In den Schritten in diesem Abschnitt wird das Hinzufügen des IoT Hub-Geräteverwaltungsclients zum benutzerdefinierten Image beschrieben, damit Sie das Edison-Gatewaygerät über Ihren IoT Hub verwalten können. Für dieses Tutorial enthält der Geräteverwaltungsclient Beispielcode zum Aktivieren eines Firmwareupdates auf Ihrem Edison-Gatewaygerät.

Führen Sie die folgenden Schritte auf demselben Ubuntu 14.04-Computer aus, den Sie im vorherigen Abschnitt zum Hinzufügen des Gateways zum Edison-Image verwendet haben.

1. Klonen Sie die Verzweigung **dmpreview** des IoT Hub-SDK-Repositorys in Ihrem Ordner „home“:
    
    ```
    cd ~
    git clone https://github.com/Azure/azure-iot-sdks -b dmpreview --recursive
    ```

2. Erstellen Sie die folgenden Ordner für Ihr neues Rezept:
    
    ```
    mkdir ~/edison-src/meta-intel-edison/meta-intel-edison-distro/recipes-support/iotdm-edison-sample
    mkdir ~/edison-src/meta-intel-edison/meta-intel-edison-distro/recipes-support/iotdm-edison-sample/files
    ```

3. Kopieren Sie die Datei **iotdm-edison-sample.bb** aus dem Ordner **~/azure-iot-sdks/c/iotdm\_client/samples/iotdm\_edison\_sample/bitbake/** in den Ordner **~/edison-src/meta-intel-edison/meta-intel-edison-distro/recipes-support/iotdm-edison-sample**.

4. Bearbeiten Sie die Datei **~/edison-src/meta-intel-edison/meta-intel-edison-distro/recipes-support/iotdm-edison-sample/iotdm-edison-sample.bb**, und ersetzen Sie `-Duse_http:BOOL=OFF` durch `-Duse_http:BOOL=ON`.

5. Kopieren Sie die Datei **iotdm\_edison\_sample.service** aus dem Ordner **~/azure-iot-sdks/c/iotdm\_client/samples/iotdm\_edison\_sample/bitbake/** in den Ordner **~/edison-src/meta-intel-edison/meta-intel-edison-distro/recipes-support/iotdm-edison-sample/files**.

6. Bearbeiten Sie die Datei **~/edison-src/meta-intel-edison/meta-intel-edison-distro/recipes-core/images/edison-image.bb**, um einen Eintrag für Ihr neues Rezept hinzuzufügen. Fügen Sie am Ende der Datei die folgende Zeile hinzu:
    
    ```
    IMAGE_INSTALL += "iotdm-edison-sample"
    ```

7. Da das Gateway-SDK und der Geräteverwaltungsclient einige Bibliotheken gemeinsam nutzen, müssen Sie die Datei **~/edison-src/out/linux64/poky/meta/classes/sstate.bbclass** bearbeiten. Fügen Sie am Ende der Datei die folgenden Zeilen hinzu. Achten Sie darauf, dass Sie `<your user>` durch Ihren derzeitigen Benutzernamen ersetzen:
    
    ```
    SSTATE_DUPWHITELIST += "/home/<your user>/edison-src/out/linux64/build/tmp/sysroots/edison/usr/lib/libaziotsharedutil.a"
    SSTATE_DUPWHITELIST += "/home/<your user>/edison-src/out/linux64/build/tmp/sysroots/edison/usr/include/azureiot"
    ```

8. Führen Sie die Konfiguration so durch, dass WLAN auf dem Edison Board automatisch gestartet wird, indem Sie die Datei **~/edison-src/meta-intel-edison/meta-intel-edison-distro/recipes-connectivity/wpa\_supplicant/wpa-supplicant/wpa\_supplicant.conf-sane** bearbeiten und am Ende der Datei die folgenden Zeilen hinzufügen. Achten Sie darauf, dass Sie `<your wifi ssid>` und `<your wifi password>` durch die richtigen Werte für Ihr WLAN ersetzen:
    
    ```
    network={
      ssid="<your wifi ssid>"
      key_mgmt=WPA-PSK
      pairwise=CCMP TKIP
      group=CCMP TKIP WEP104 WEP40
      eap=TTLS PEAP TLS
      psk="<your wifi password>"
    }
    ```

9. Sie können nun das Image für Ihr Edison Board erstellen, in dem das Gateway-SDK und der Geräteverwaltungsclient enthalten sind. Der Befehl **bitbake** wird jetzt deutlich schneller als vorher ausgeführt, da nur das neue Rezept erstellt und dem Image hinzugefügt werden muss:
    
    ```
    cd ~/edison-src/out/linux64/
    source poky/oe-init-build-env
    bitbake edison-image
    ```

10. Schließen Sie den Buildvorgang ab, indem Sie die folgenden Befehle ausführen:
  
    ```
    cd ~/edison-src/
    ./meta-intel-edison/utils/flash/postBuild.sh ./out/linux64/build/
    ```

Die Dateien, die Sie zum Aktualisieren des Edison Boards benötigen, befinden sich jetzt im Ordner **~/edison-src/out/linux64/build/toFlash/**.

### Aktualisieren des Intel Edison Boards mit dem benutzerdefinierten Image

Sie können Ihr Edison Board jetzt mit dem benutzerdefinierten Image aktualisieren, in dem sowohl der IoT Hub-Geräteverwaltungsclient als auch die Gatewaysoftware enthalten sind.

Kopieren Sie die Dateien aus dem Ordner **toFlash** auf dem Ubuntu-Computer, den Sie zum Erstellen des benutzerdefinierten Image verwendet haben, auf den Computer, der mit Ihrem Edison Board per USB-Kabel verbunden ist.

Wenn Sie einen Windows-Computer zum Verbinden des Edison Boards per USB-Kabel verwenden, sollten Sie das Skript **flashall.bat** ausführen, um das Edison Board zu aktualisieren. Wenn Sie einen Linux-Computer zum Verbinden des Edison Boards per USB-Kabel verwenden, sollten Sie das Skript **flashall.bat** ausführen, um das Edison Board zu aktualisieren.

Stellen Sie die Verbindung mit dem Edison Board nach Abschluss des Aktualisierungsprozesses mit einer [seriellen Verbindung][lnk-serial-connection] von Ihrem Hostcomputer her, und melden Sie sich als **root** an. Stellen Sie sicher, dass das Edison Board jetzt über eine Verbindung mit Ihrem WLAN verfügt.

### Ausführen des Beispiels

Sie müssen den Geräteverwaltungsclient auf dem Edison Board konfigurieren, um die Verbindung mit dem IoT Hub über das Gerät **GW-device** herzustellen. Verwenden Sie einen Text-Editor (z.B. **vi** oder **nano**), um eine Datei mit der Endung **.cs** im Ordner „/home/root“ auf dem Edison Board zu erstellen. Diese Datei sollte nur die Verbindungszeichenfolge von **GW-device** enthalten. Wenn Sie sich diese Verbindungszeichenfolge zuvor nicht notiert haben, können Sie den [Geräte-Explorer oder iothub-explorer][lnk-explorer-tools] verwenden, um diese Geräteverbindungszeichenfolge aus der IoT Hub-Geräteregistrierung abzurufen.

Nach dem Erstellen der **CS**-Datei müssen Sie das Edison Board mit dem folgenden Befehl neu starten:

```
reboot -h now
```

Überprüfen Sie beim Starten des Edison Boards, ob sowohl der Geräteverwaltungsdienst als auch der Gatewaydienst mit dem Status **OK** gestartet werden:

```
[  OK  ] Started Daemon to receive the wpa_supplicant event.
         Starting PulseAudio Sound System...
[  OK  ] Started WPA supplicant service.
[  OK  ] Started Azure IoT DM as a service..
[  OK  ] Started Azure Iot Gateway as a service..
         Stopping Daemon to receive the wpa_supplicant event...
[  OK  ] Stopped Daemon to receive the wpa_supplicant event.

```

Zum Durchführen der Problembehandlung für den IoT-Gatewaydienst können Sie sich die Einträge in der Datei **/deviceCloudUploadGatewaylog.log** auf dem Edison Board ansehen. Sie können auch den folgenden Befehl verwenden, um die Ausgabe des Diensts anzuzeigen:

```
systemctl status simulated_device_cloud_upload.service
```

Verwenden Sie den folgenden Befehl zum Anzeigen der Ausgabe, um etwaige Probleme mit dem IoT-Geräteverwaltungsdienst zu behandeln:

```
systemctl status iotdm_edison_sample.service
```

### Starten des Auftrags für das Firmwareupdate

Bei einem Firmwareupdate auf dem Edison Board, das vom IoT-Geräteverwaltungsdienst angefordert wird, wird normalerweise über eine URL eine ZIP-Datei heruntergeladen, die die Firmware enthält. Zur Vereinfachung dieses Tutorials können Sie die ZIP-Datei manuell auf das Edison Board kopieren und beim Anfordern des Updates dann eine **file://**-URL verwenden, anstatt eine **http://**-URL.

Ebenfalls zur Vereinfachung des Tutorials wird beim Firmwareupdate dasselbe Firmwareimage erneut angewendet, anstatt ein ganz neues Image zu verwenden. Sie können verfolgen, dass dieses Image auf das Edison Board angewendet wird.

Erstellen Sie eine ZIP-Datei mit dem Namen **edison.zip**, die alle Dateien und Unterordner des Ordners **toFlash** auf dem Ubuntu-Computer enthält, den Sie zum Erstellen des benutzerdefinierten Image verwendet haben. Stellen Sie sicher, dass sich die Dateien aus dem Ordner **toFlash** im Stamm der ZIP-Datei befinden. Verwenden Sie ein Tool wie SCP (oder PSCP bei Verwendung von PuTTY), um die Datei **edison.zip** in den Ordner „/home/root“ auf dem Edison Board zu kopieren.

Verwenden Sie die [Beispiel-UI für die Geräteverwaltung][lnk-dm-sample-ui] von Node.js , um den Auftrag für das Firmwareupdate zu übermitteln und den Status zu überwachen. Sie können dieses Beispiel entweder unter Windows oder Linux ausführen, und Sie benötigen [Node.js][lnk-nodejs] 6.1.0 oder höher. Führen Sie die folgenden Schritte aus, um die Beispiel-UI für die Geräteverwaltung abzurufen, zu erstellen und auf Ihrem Desktopcomputer auszuführen:

1. Öffnen Sie eine **Eingabeaufforderung**.

2. Vergewissern Sie sich, das Sie Node.js 6.1.0 oder höher installiert haben, indem Sie `node --version` eingeben.

3. Klonen Sie das GitHub-Repository zur Azure IoT-UI für die Gerätebereitstellung, indem Sie den folgenden Befehl ausführen:

    ```
    git clone https://github.com/Azure/azure-iot-device-management.git
    ```
    
4. Führen Sie im Stammordner Ihrer geklonten Kopie des Repositorys der Azure IoT-UI für die Gerätebereitstellung den folgenden Befehl aus, um die abhängigen Pakete abzurufen:

    ```
    npm install
    ```

5. Führen Sie nach Abschluss des Installationsbefehls „npm“ den folgenden Befehl aus, um den Code zu erstellen:

    ```
    npm run build
    ```

6. Verwenden Sie einen Text-Editor, um die Datei „user-config.json“ im Stamm des geklonten Ordners zu öffnen. Ersetzen Sie den Text „&lt;YOUR CONNECTION STRING HERE&gt;“ durch Ihre IoT Hub-Verbindungszeichenfolge. Sie finden diese Verbindungszeichenfolge im Azure-[Portal][lnk-azure-portal].

7. Führen Sie an der Eingabeaufforderung den folgenden Befehl aus, um die UX-App für die Geräteverwaltung zu starten:

    ```
    npm run start
    ```

8. Wenn in der Eingabeaufforderung die Meldung „Services have started“ angezeigt wird, können Sie einen Webbrowser öffnen und unter der folgenden URL zur Geräteverwaltungs-App navigieren, um die Geräte anzuzeigen: <http://127.0.0.1:3003>.

    ![Benutzeroberfläche für die Geräteverwaltung][img-dm-ui]

9. Wählen Sie das Gerät **GW-device** in der Dropdownliste **Device Jobs** (Geräteaufträge) aus, wählen Sie **Firmwareupdate**, und klicken Sie dann auf **Starten**.

10. Geben Sie im Feld **Package URI** (Paket-URI) **file:///home/root/edison.zip** ein, um die Imagedatei zu verwenden, die Sie zuvor auf das Edison Board kopiert haben. Klicken Sie auf **Absenden**, klicken Sie auf **Ja**, und klicken Sie dann auf den Link **Auftragsverlauf**, um die neuen ausgeführten übergeordneten und untergeordneten Aufträge anzuzeigen:

    ![Link „Auftragsverlauf“][img-history-link]

11. Nach einigen Minuten sehen Sie im seriellen Terminal, das mit Ihrem Edison Board verbunden ist, wie das Edison Board neu gestartet wird und die Firmwareänderungen anwendet:

    ```
    reading ota_update.scr
    14747 bytes read in 17 ms (846.7 KiB/s)
    ## Executing script at 00100000
    === OTA update script ===
    Validating Ota package
    part find mmc 0 label:update u_part_num;
    ota drive is mmc 0:9

    Validating edison_ifwi-dbg-00-dfu.bin hash for boot0 and boot1 partitions

    fatload mmc 0:9 0x6400000 edison_ifwi-dbg-00-dfu.bin;
    reading edison_ifwi-dbg-00-dfu.bin
    ...
    ```

12. Nachdem das Edison Board den Neustart abgeschlossen hat, können Sie die Seite in der Beispiel-UI für die Geräteverwaltung aktualisieren. Sie sehen, dass der Auftragsstatus für die beiden Firmwareupdate-Aufträge jetzt **completed** (Abgeschlossen) lautet:

    ![Auftragsstatus „Abgeschlossen“][img-job-status]

Sie haben das Tutorial jetzt abgeschlossen. Es wurde gezeigt, wie Sie die IoT Hub-Gatewaysoftware und den Geräteverwaltungsclient auf einem Intel Edison Board verwenden. Sie haben in diesem Tutorial Folgendes durchgeführt:

- Erstellen eines benutzerdefinierten Intel Edison-Image, in dem der IoT Hub-Geräteverwaltungsclient und die Gatewaysoftware enthalten und so konfiguriert sind, dass sie bei jedem Startvorgang des Edison Boards gestartet werden
- Konfigurieren des Edison Boards und Geräteverwaltungsclients für die Verbindungsherstellung mit Ihrem IoT Hub
- Starten eines Geräteverwaltungsauftrags über die Beispiel-UI, der bewirkt, dass das Edison Board neu gestartet wird und ein neues Firmwareimage anwendet

## Nächste Schritte

Weitere Informationen zur Geräteverwaltung mit IoT Hub und der Beispiel-UI finden Sie im Artikel [Überblick über die Azure IoT Hub-Geräteverwaltung][lnk-device-management].

Wenn Sie noch mehr über das Gateway SDK wissen und mit einigen Codebeispielen experimentieren möchten, siehe den Artikel zum [Azure IoT Gateway SDK][lnk-gateway-sdk].

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

- [Entwerfen Ihrer Lösung][lnk-design]
- [Entwicklerhandbuch][lnk-devguide]
- [Erkunden der Geräteverwaltung mithilfe der Beispielbenutzeroberfläche][lnk-dmui]
- [Verwenden des Azure-Portals zur Verwaltung von IoT Hub][lnk-portal]



<!-- Links and images -->

[img-dm-ui]: media/iot-hub-gateway-sdk-device-management/image1.png
[img-history-link]: media/iot-hub-gateway-sdk-device-management/image2.png
[img-job-status]: media/iot-hub-gateway-sdk-device-management/image3.png
[img-architecture]: media/iot-hub-gateway-sdk-device-management/architecture.png

[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-device-management]: iot-hub-device-management-overview.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-setup-win64]: https://software.intel.com/get-started-edison-windows
[lnk-setup-win32]: https://software.intel.com/get-started-edison-windows-32
[lnk-setup-osx]: https://software.intel.com/get-started-edison-osx
[lnk-setup-linux]: https://software.intel.com/get-started-edison-linux
[lnk-serial-connection]: https://software.intel.com/setting-up-serial-terminal-intel-edison-board
[lnk-inteledison-bsp]: http://www.intel.com/content/dam/support/us/en/documents/edison/sb/edisonbsp_ug_331188007.pdf
[lnk-hackgnar]: http://www.hackgnar.com/2016/01/manually-building-yocto-images-for.html
[lnk-shawnhymel]: http://shawnhymel.com/724/creating-a-custom-linux-kernel-for-the-edison-yocto-2-1/
[lnk-create-hub]: iot-hub-manage-through-portal.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-nodejs]: https://nodejs.org/
[lnk-azure-portal]: https://portal.azure.com
[lnk-dm-sample-ui]: iot-hub-device-management-ui-sample.md
[lnk-gateway-physical]: iot-hub-gateway-sdk-physical-device.md
[lnk-gateway-scenario]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-dm-jobs]: iot-hub-device-management-device-jobs.md

[lnk-design]: iot-hub-guidance.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-dmui]: iot-hub-device-management-ui-sample.md
[lnk-portal]: iot-hub-manage-through-portal.md

<!---HONumber=AcomDC_0928_2016-->