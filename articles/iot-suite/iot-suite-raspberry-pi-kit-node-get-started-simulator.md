---
title: "Verbinden eines Raspberry Pi-Geräts mit Azure IoT Suite mithilfe von Node.js mit simulierten Telemetriedaten | Microsoft-Dokumentation"
description: "Verwenden Sie das Microsoft Azure IoT Starter Kit für Raspberry Pi 3 und Azure IoT Suite. Verbinden Sie Ihr Raspberry Pi mithilfe von Node.js mit der Remoteüberwachungslösung. Senden Sie simulierte Telemetriedaten an die Cloud, und antworten Sie auf Methoden, die über das Dashboard der Lösung aufgerufen werden."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: node.js
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 3d41a3665ffb2217ecbe1ad53853f0e9d30fac34
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017

---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-simulated-telemetry-using-nodejs"></a>Verbinden Sie Ihr Raspberry Pi 3 mit der Remoteüberwachungslösung, und senden Sie mithilfe von Node.js simulierte Telemetriedaten.

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

In diesem Tutorial wird gezeigt, wie Sie das Raspberry Pi 3 nutzen, um Temperatur- und Luftfeuchtigkeitsdaten zu simulieren, die in die Cloud gesendet werden sollen. In diesem Tutorial wird Folgendes verwendet:

- Raspbian OS, die Programmiersprache Node.js und das Microsoft Azure IoT SDK für Node.js, um ein Beispielgerät zu implementieren.
- Die von IoT Suite vorkonfigurierte Remoteüberwachungslösung als cloudbasiertes Back-End.

[!INCLUDE [iot-suite-raspberry-pi-kit-overview-simulator](../../includes/iot-suite-raspberry-pi-kit-overview-simulator.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> Die Remoteüberwachungslösung stellt eine Gruppe von Azure-Diensten im Azure-Abonnement bereit. Die Bereitstellung gibt eine echte Unternehmensarchitektur wieder. Um unnötige Azure-Nutzungsgebühren zu vermeiden, löschen Sie Ihre Instanz der vorkonfigurierten Lösung auf „azureiotsuite.com“, sobald Sie damit fertig sind. Wenn Sie die vorkonfigurierte Lösung erneut benötigen, können Sie sie problemlos wiederherstellen. Weitere Informationen zur Senkung der Nutzung während der Ausführung der Remoteüberwachungslösung finden Sie unter [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config] (Konfigurieren vorkonfigurierter Azure IoT Suite-Lösungen zu Demonstrationszwecken).

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi-simulator](../../includes/iot-suite-raspberry-pi-kit-prepare-pi-simulator.md)]

## <a name="download-and-configure-the-sample"></a>Herunterladen und Konfigurieren des Beispiels

Sie können jetzt Sie die Clientanwendung für die Remoteüberwachung auf Ihr Raspberry Pi herunterladen und anschließend konfigurieren.

### <a name="install-nodejs"></a>Installieren von Node.js

Falls noch nicht geschehen, installieren Sie Node.js auf Ihrem Raspberry Pi. Das IoT SDK für Node.js erfordert Version 0.11.5 von Node.js oder höher. In den folgenden Schritten wird gezeigt, wie Sie Node.js 6.10.2 auf Ihrem Raspberry Pi installieren:

1. Verwenden Sie den folgenden Befehl aus, um Ihr Raspberry Pi zu aktualisieren:

    ```sh
    sudo apt-get update
    ```

1. Verwenden Sie den folgenden Befehl zum Herunterladen der Node.js-Binärdateien auf Ihr Raspberry Pi:

    ```sh
    wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz
    ```

1. Verwenden Sie den folgenden Befehl, um die Binärdateien zu installieren:

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz
    ```

1. Führen Sie den folgenden Befehl aus, um zu überprüfen, ob Sie Node.js 6.10.2 erfolgreich installiert haben:

    ```sh
    node --version
    ```

### <a name="clone-the-repositories"></a>Klonen der Repositorys

Falls nicht bereits erfolgt, klonen Sie die erforderlichen Repositorys durch Ausführen der folgenden Befehle in einem Terminal auf Ihrem Pi:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git
```

### <a name="update-the-device-connection-string"></a>Aktualisieren der Verbindungszeichenfolge des Geräts

Öffnen Sie die Beispielquelldatei im **Nano**-Editor mithilfe des folgenden Befehls:

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

Suchen Sie die folgende Zeile:

```javascript
var connectionString = 'HostName=[Your IoT hub name].azure-devices.net;DeviceId=[Your device id];SharedAccessKey=[Your device key]';
```

Ersetzen Sie die Platzhalterwerte durch die Geräte- und IoT Hub-Informationen, die Sie am Anfang dieses Tutorials erstellt und gespeichert haben. Speichern Sie Ihre Änderungen (**STRG+O**, **EINGABETASTE**), und beenden Sie den Editor (**STRG+X**).

## <a name="run-the-sample"></a>Ausführen des Beispiels

Führen Sie die folgenden Befehle zum Installieren der erforderlichen Pakete für das Beispiel aus:

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator
npm install
```

Sie können nun das Beispielprogramm auf dem Raspberry Pi ausführen. Geben Sie den folgenden Befehl ein:

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

Die folgende Beispielausgabe ist ein Beispiel der Ausgabe, die an der Eingabeaufforderung auf dem Raspberry Pi angezeigt wird:

![Ausgabe der Raspberry Pi-App][img-raspberry-output]

Drücken Sie **STRG+C**, um das Programm zu einem beliebigen Zeitpunkt zu beenden.

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-simulator](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-simulator.md)]

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie das [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/), in dem Sie weitere Beispiele und Dokumentation zu Azure IoT finden.

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-node-get-started-simulator/app-output.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

