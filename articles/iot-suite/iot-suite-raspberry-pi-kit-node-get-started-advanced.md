---
title: "Verbinden eines Raspberry Pi mit Azure IoT Suite mithilfe von Node.js zum Unterstützen von Firmwareupdates | Microsoft-Dokumentation"
description: "Verwenden Sie das Microsoft Azure IoT Starter Kit für Raspberry Pi 3 und Azure IoT Suite. Verbinden Sie Ihr Raspberry Pi mithilfe von Node.js mit der Remoteüberwachungslösung. Senden Sie Telemetriedaten von Sensoren an die Cloud, und führen Sie ein Firmwareupdate remote aus."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.translationtype: HT
ms.sourcegitcommit: bfd49ea68c597b109a2c6823b7a8115608fa26c3
ms.openlocfilehash: 54503d5d6a636239d240509d7d09cf334234bac7
ms.contentlocale: de-de
ms.lasthandoff: 07/25/2017

---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-enable-remote-firmware-updates-using-nodejs"></a>Verbinden Ihres Raspberry Pi 3 mit der Remoteüberwachungslösung und Ermöglichen von Remoteupdates der Firmware mithilfe von Node.js

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

In diesem Tutorial wird gezeigt, wie Sie das Microsoft Azure IoT Starter Kit für Raspberry Pi 3 für folgende Zwecke nutzen:

* Entwickeln eines Temperatur- und Luftfeuchtigkeitslesers, der mit der Cloud kommunizieren kann.
* Aktivieren und Ausführen eines Remoteupdates der Firmware zum Aktualisieren der Clientanwendung auf dem Raspberry Pi.

In diesem Tutorial wird Folgendes verwendet:

- Raspbian OS, die Programmiersprache Node.js und das Microsoft Azure IoT SDK für Node.js, um ein Beispielgerät zu implementieren.
- Die von IoT Suite vorkonfigurierte Remoteüberwachungslösung als cloudbasiertes Back-End.

## <a name="overview"></a>Übersicht

In diesem Tutorial führen Sie folgende Schritte aus:

- Bereitstellen einer Instanz der vorkonfigurierten Remoteüberwachungslösung für Ihr Azure-Abonnement. Durch diesen Schritt werden automatisch mehrere Azure-Dienste bereitgestellt und konfiguriert.
- Einrichten Ihres Geräts und von Sensoren für die Kommunikation mit Ihrem Computer und der Remoteüberwachungslösung.
- Aktualisieren des Beispielgerätecodes für die Verbindungsherstellung mit der Remoteüberwachungslösung und Senden von Telemetriedaten, die auf dem Lösungsdashboard angezeigt werden können.
- Verwenden Sie den Beispielgerätecode, um die Clientanwendung zu aktualisieren.

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> Die Remoteüberwachungslösung stellt eine Gruppe von Azure-Diensten im Azure-Abonnement bereit. Die Bereitstellung gibt eine echte Unternehmensarchitektur wieder. Um unnötige Azure-Nutzungsgebühren zu vermeiden, löschen Sie Ihre Instanz der vorkonfigurierten Lösung auf „azureiotsuite.com“, sobald Sie damit fertig sind. Wenn Sie die vorkonfigurierte Lösung erneut benötigen, können Sie sie problemlos wiederherstellen. Weitere Informationen zur Senkung der Nutzung während der Ausführung der Remoteüberwachungslösung finden Sie unter [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config] (Konfigurieren vorkonfigurierter Azure IoT Suite-Lösungen zu Demonstrationszwecken).

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

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

Falls nicht bereits erfolgt, klonen Sie die erforderlichen Repositorys durch Ausführen der folgenden Befehle auf Ihrem Pi:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git
```

### <a name="update-the-device-connection-string"></a>Aktualisieren der Verbindungszeichenfolge des Geräts

Öffnen Sie die Beispielkonfigurationsdatei im **Nano**-Editor mithilfe des folgenden Befehls:

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advanced/config/deviceinfo
```

Ersetzen Sie die Platzhalterwerte durch die Geräte-ID und IoT Hub-Informationen, die Sie am Anfang dieses Tutorials erstellt und gespeichert haben.

Wenn Sie fertig sind, sollte der Inhalt der Datei „deviceinfo“ wie im folgenden Beispiel aussehen:

```conf
yourdeviceid
HostName=youriothubname.azure-devices.net;DeviceId=yourdeviceid;SharedAccessKey=yourdevicekey
```

Speichern Sie Ihre Änderungen (**STRG+O**, **EINGABETASTE**), und beenden Sie den Editor (**STRG+X**).

## <a name="run-the-sample"></a>Ausführen des Beispiels

Führen Sie die folgenden Befehle zum Installieren der erforderlichen Pakete für das Beispiel aus:

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advance/1.0
npm install
```

Sie können nun das Beispielprogramm auf dem Raspberry Pi ausführen. Geben Sie den folgenden Befehl ein:

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advanced/1.0/remote_monitoring.js
```

Die folgende Beispielausgabe ist ein Beispiel der Ausgabe, die an der Eingabeaufforderung auf dem Raspberry Pi angezeigt wird:

![Ausgabe der Raspberry Pi-App][img-raspberry-output]

Drücken Sie **STRG+C**, um das Programm zu einem beliebigen Zeitpunkt zu beenden.

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-advanced](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-advanced.md)]

1. Klicken Sie im Lösungsdashboard auf **Geräte**, um die Seite **Geräte** aufzurufen. Wählen Sie in der **Geräteliste** Ihren Raspberry Pi aus. Wählen Sie anschließend **Methoden** aus:

    ![Auflisten von Geräten im Dashboard][img-list-devices]

1. Wählen Sie auf der Seite **Methode aufrufen** in der Dropdownliste **Methode** die Option **InitiateFirmwareUpdate** aus.

1. Geben Sie in das Feld **FWPackageURI** Folgendes ein: **https://raw.githubusercontent.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit/master/advanced/2.0/raspberry.js**. Diese Datei enthält die Implementierung der Version 2.0 der Firmware.

1. Wählen Sie **InvokeMethod** aus. Die App auf dem Raspberry Pi gibt eine Bestätigung an das Lösungsdashboard zurück. Anschließend wird das Update der Firmware gestartet, indem die neue Version der Firmware heruntergeladen wird:

    ![Anzeigen des Methodenverlaufs][img-method-history]

## <a name="observe-the-firmware-update-process"></a>Verfolgen des Firmwareupdates

Sie können das Update der Firmware während der Ausführung auf dem Gerät und durch Anzeigen der gemeldeten Eigenschaften im Lösungsdashboard verfolgen:

1. Sie können den Status des Updatevorgangs auf dem Raspberry Pi anzeigen:

    ![Anzeigen des Status des Updates][img-update-progress]

    > [!NOTE]
    > Die Remoteüberwachungs-App wird automatisch neu gestartet, sobald das Update abgeschlossen ist. Verwenden Sie den Befehl `ps -ef`, um zu überprüfen, ob sie ausgeführt wird. Wenn Sie den Prozess beenden möchten, verwenden Sie den Befehl `kill` mit der Prozess-ID.

1. Sie können den vom Gerät gemeldeten Status des Updates der Firmware im Lösungsportal anzeigen. Der folgende Screenshot zeigt den Status und die Dauer der einzelnen Phasen des Updatevorgangs und die neue Firmwareversion:

    ![Anzeigen des Auftragsstatus][img-job-status]

    Wenn Sie zurück zum Dashboard navigieren, können Sie überprüfen, ob das Gerät nach dem Update der Firmware weiter Telemetriedaten sendet.

> [!WARNING]
> Wenn Sie die Ausführung der Remoteüberwachungslösung in Ihrem Azure-Konto nicht beenden, wird Ihnen die Ausführungszeit in Rechnung gestellt. Weitere Informationen zur Senkung der Nutzung während der Ausführung der Remoteüberwachungslösung finden Sie unter [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config] (Konfigurieren vorkonfigurierter Azure IoT Suite-Lösungen zu Demonstrationszwecken). Löschen Sie die vorkonfigurierte Lösung aus Ihrem Azure-Konto, wenn Sie sie nicht mehr benötigen.

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie das [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/), in dem Sie weitere Beispiele und Dokumentation zu Azure IoT finden.


[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/app-output.png
[img-update-progress]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/updateprogress.png
[img-job-status]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/jobstatus.png
[img-list-devices]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/listdevices.png
[img-method-history]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

