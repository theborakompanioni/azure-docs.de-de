---
title: "Verbinden eines Raspberry Pi-Geräts mit Azure IoT Suite mithilfe von C mit echten Sensoren | Microsoft-Dokumentation"
description: "Verwenden Sie das Microsoft Azure IoT Starter Kit für Raspberry Pi 3 und Azure IoT Suite. Verbinden Sie Ihr Raspberry Pi mithilfe von C mit der Remoteüberwachungslösung. Senden Sie Telemetriedaten von Sensoren an die Cloud, und antworten Sie auf Methoden, die über das Dashboard der Lösung aufgerufen werden."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/24/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: ce8809a8e464e5a6815ced6e5609e5196a896057
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017


---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-telemetry-from-a-real-sensor-using-c"></a>Verbinden Sie Ihr Raspberry Pi 3 mit der Remoteüberwachungslösung, und senden Sie mithilfe von C Telemetriedaten von einem echten Sensor.

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

In diesem Tutorial wird gezeigt, wie Sie mit dem Microsoft Azure IoT Starter Kit für Raspberry Pi 3 einen Temperatur- und Luftfeuchtigkeitsleser entwickeln können, der mit der Cloud kommunizieren kann. In diesem Tutorial wird Folgendes verwendet:

- Raspbian OS, die Programmiersprache C und das Microsoft Azure IoT SDK für C, um ein Beispielgerät zu implementieren.
- Die von IoT Suite vorkonfigurierte Remoteüberwachungslösung als cloudbasiertes Back-End.

## <a name="overview"></a>Übersicht

In diesem Tutorial führen Sie folgende Schritte aus:

- Bereitstellen einer Instanz der vorkonfigurierten Remoteüberwachungslösung für Ihr Azure-Abonnement. Durch diesen Schritt werden automatisch mehrere Azure-Dienste bereitgestellt und konfiguriert.
- Einrichten Ihres Geräts und von Sensoren für die Kommunikation mit Ihrem Computer und der Remoteüberwachungslösung.
- Aktualisieren des Beispielgerätecodes für die Verbindungsherstellung mit der Remoteüberwachungslösung und Senden von Telemetriedaten, die auf dem Lösungsdashboard angezeigt werden können.

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> Die Remoteüberwachungslösung stellt eine Gruppe von Azure-Diensten im Azure-Abonnement bereit. Die Bereitstellung gibt eine echte Unternehmensarchitektur wieder. Um unnötige Azure-Nutzungsgebühren zu vermeiden, löschen Sie Ihre Instanz der vorkonfigurierten Lösung auf „azureiotsuite.com“, sobald Sie damit fertig sind. Wenn Sie die vorkonfigurierte Lösung erneut benötigen, können Sie sie problemlos wiederherstellen. Weitere Informationen zur Senkung der Nutzung während der Ausführung der Remoteüberwachungslösung finden Sie unter [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config] (Konfigurieren vorkonfigurierter Azure IoT Suite-Lösungen zu Demonstrationszwecken).

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-the-sample"></a>Herunterladen und Konfigurieren des Beispiels

Sie können jetzt Sie die Clientanwendung für die Remoteüberwachung auf Ihr Raspberry Pi herunterladen und anschließend konfigurieren.

### <a name="clone-the-repositories"></a>Klonen der Repositorys

Falls nicht bereits erfolgt, klonen Sie die erforderlichen Repositorys durch Ausführen der folgenden Befehle in einem Terminal auf Ihrem Pi:

`cd ~`

`git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git`

`git clone --recursive https://github.com/WiringPi/WiringPi.git`

### <a name="update-the-device-connection-string"></a>Aktualisieren der Verbindungszeichenfolge des Geräts

Öffnen Sie die Beispielquelldatei im **Nano**-Editor mithilfe des folgenden Befehls:

`nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/remote_monitoring/remote_monitoring.c`

Suchen Sie die folgenden Zeilen:

```c
static const char* deviceId = "[Device Id]";
static const char* connectionString = "HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]";
```

Ersetzen Sie die Platzhalterwerte durch die Geräte- und IoT Hub-Informationen, die Sie am Anfang dieses Tutorials erstellt und gespeichert haben. Speichern Sie Ihre Änderungen (**STRG+O**, **EINGABETASTE**), und beenden Sie den Editor (**STRG+X**).

## <a name="build-the-sample"></a>Erstellen des Beispiels

Installieren Sie die erforderlichen Pakete für das Microsoft Azure IoT-Geräte-SDK für C durch Ausführen der folgenden Befehle in einem Terminal auf dem Raspberry Pi:

`sudo apt-get update`

`sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev`

Sie können die aktualisierte Beispiellösung auf dem Raspberry Pi erstellen:

`chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/build.sh`

`~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/build.sh`

Sie können nun das Beispielprogramm auf dem Raspberry Pi ausführen. Geben Sie den folgenden Befehl ein:

  `sudo ~/cmake/remote_monitoring/remote_monitoring`

Die folgende Beispielausgabe ist ein Beispiel der Ausgabe, die an der Eingabeaufforderung auf dem Raspberry Pi angezeigt wird:

![Ausgabe der Raspberry Pi-App][img-raspberry-output]

Drücken Sie **STRG+C**, um das Programm zu einem beliebigen Zeitpunkt zu beenden.

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry](../../includes/iot-suite-raspberry-pi-kit-view-telemetry.md)]

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie das [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/), in dem Sie weitere Beispiele und Dokumentation zu Azure IoT finden.


[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-basic/appoutput.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

