---
title: "Verbinden eines Gateways mit Azure IoT Suite über einen Intel NUC | Microsoft-Dokumentation"
description: "Verwenden Sie das Microsoft IoT Commercial Gateway Kit und die vorkonfigurierte Remoteüberwachungslösung. Verwenden Sie das Azure IoT Edge-Gateway, um eine Verbindung mit der Remoteüberwachungslösung herzustellen, simulierte Telemetriedaten an die Cloud zu senden sowie auf Methoden zu reagieren, die über das Lösungsdashboard aufgerufen wurden."
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
ms.date: 07/25/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: 9edcaee4d051c3dc05bfe23eecc9c22818cf967c
ms.openlocfilehash: 42e82793b73a941df08e438d9e04669a386a8368
ms.contentlocale: de-de
ms.lasthandoff: 06/08/2017

---
# <a name="connect-your-azure-iot-edge-gateway-to-the-remote-monitoring-preconfigured-solution-and-send-simulated-telemetry"></a>Verbinden Ihres Azure IoT Edge-Gateways mit der vorkonfigurierten Remoteüberwachungslösung und Senden von simulierten Telemetriedaten

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

In diesem Tutorial wird gezeigt, wie Azure IoT Edge verwendet wird, um ein Senden von Temperatur- und Feuchtigkeitsdaten an die vorkonfigurierte Remoteüberwachungslösung zu simulieren. In diesem Tutorial wird Folgendes verwendet:

- Azure IoT Edge, um ein Beispielgateway zu implementieren.
- Die von IoT Suite vorkonfigurierte Remoteüberwachungslösung als cloudbasiertes Back-End.

## <a name="overview"></a>Übersicht

In diesem Tutorial führen Sie folgende Schritte aus:

- Bereitstellen einer Instanz der vorkonfigurierten Remoteüberwachungslösung für Ihr Azure-Abonnement. Durch diesen Schritt werden automatisch mehrere Azure-Dienste bereitgestellt und konfiguriert.
- Einrichten Ihres Intel NUC-Gatewaygeräts für die Kommunikation mit Ihrem Computer und der Remoteüberwachungslösung.
- Konfigurieren des IoT Edge-Gateways, sodass es simulierte Telemetriedaten sendet, die Sie auf dem Lösungsdashboard anzeigen können.

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> Die Remoteüberwachungslösung stellt eine Gruppe von Azure-Diensten im Azure-Abonnement bereit. Die Bereitstellung gibt eine echte Unternehmensarchitektur wieder. Um unnötige Azure-Nutzungsgebühren zu vermeiden, löschen Sie Ihre Instanz der vorkonfigurierten Lösung auf „azureiotsuite.com“, sobald Sie damit fertig sind. Wenn Sie die vorkonfigurierte Lösung erneut benötigen, können Sie sie problemlos wiederherstellen. Weitere Informationen zur Senkung der Nutzung während der Ausführung der Remoteüberwachungslösung finden Sie unter [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config] (Konfigurieren vorkonfigurierter Azure IoT Suite-Lösungen zu Demonstrationszwecken).

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

Wiederholen Sie die vorherigen Schritte, um ein zweites Gerät hinzuzufügen, dessen Geräte-ID beispielsweise **device02** lautet. In dem Beispiel werden Daten von zwei simulierten Geräten im Gateway an die Remoteüberwachungslösung gesendet.

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-software](../../includes/iot-suite-gateway-kit-prepare-nuc-software.md)]

## <a name="build-the-custom-iot-edge-module"></a>Erstellen des benutzerdefinierten IoT Edge-Moduls

Sie können nun das benutzerdefinierte IoT Edge-Modul erstellen, das es dem Gateway ermöglicht, Nachrichten an die Remoteüberwachungslösung zu senden. Weitere Informationen zum Konfigurieren eines Gateways und von IoT Edge-Modulen finden Sie unter [Azure IoT Edge-Konzepte][lnk-gateway-concepts].

Laden Sie den Quellcode für die benutzerdefinierten IoT Edge-Module von GitHub mit den folgenden Befehlen herunter:

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

Führen Sie die folgenden Befehle aus, um das benutzerdefinierte IoT Edge-Modul zu erstellen:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

Das Buildskript platziert das benutzerdefinierte IoT Edge-Modul „libsimulator.so“ im Ordner für Builds.

## <a name="configure-and-run-the-iot-edge-gateway"></a>Konfigurieren und Ausführen des IoT Edge-Gateways

Sie können nun das IoT Edge-Gateway so konfigurieren, dass es simulierte Telemetriedaten an Ihr Remoteüberwachungsdashboard sendet. Weitere Informationen zum Konfigurieren eines Gateways und von IoT Edge-Modulen finden Sie unter [Azure IoT Edge-Konzepte][lnk-gateway-concepts].

> [!TIP]
> In diesem Tutorial verwenden Sie den Standard-Text-Editor `vi` auf dem Intel NUC. Wenn Sie noch nie mit `vi` gearbeitet haben, sollten Sie ein einführendes Tutorial, z. B. [Unix - The vi Editor Tutorial][lnk-vi-tutorial] durcharbeiten, um sich mit diesem Editor vertraut zu machen. Alternativ können Sie den benutzerfreundlicheren Editor [nano](https://www.nano-editor.org/) mit dem Befehl `smart install nano -y` installieren.

Öffnen Sie die Beispielkonfigurationsdatei im Editor **vi** mit dem folgenden Befehl:

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator/remote_monitoring.json
```

Suchen Sie nach den folgenden Zeilen in der Konfiguration für das Modul „IoTHub“:

```json
"args": {
  "IoTHubName": "<<Azure IoT Hub Name>>",
  "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
  "Transport": "http"
}
```

Ersetzen Sie die Platzhalterwerte durch die IoT Hub-Informationen, die Sie am Anfang dieses Tutorials erstellt und gespeichert haben. Der Wert für „IoTHubName“ sieht wie **yourrmsolution37e08** aus, und „IoTSuffix“ hat üblicherweise den Wert **azure-devices.net**.

Suchen Sie nach den folgenden Zeilen in der Konfiguration für das Zuordnungsmodul:

```json
args": [
  {
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  },
  {
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

Ersetzen Sie die Platzhalter **deviceID** und **deviceKey** durch die IDs und Schlüssel für die beiden Geräte, die Sie zuvor in der Remoteüberwachungslösung erstellt haben.

Speichern Sie die Änderungen.

Sie können das IoT Edge-Gateway jetzt mit den folgenden Befehlen ausführen:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
/usr/share/azureiotgatewaysdk/samples/simulated_device_cloud_upload/simulated_device_cloud_upload remote_monitoring.json
```

Das Gateway wird auf dem Intel NUC gestartet und sendet simulierte Telemetriedaten an die Remoteüberwachungslösung:

![IoT Edge-Gateway generiert simulierte Telemetriedaten.][img-simulated telemetry]

Drücken Sie **STRG+C**, um das Programm zu einem beliebigen Zeitpunkt zu beenden.

## <a name="view-the-telemetry"></a>Anzeigen der Telemetrie

Der IoT Edge-Gateway sendet nun simulierte Telemetriedaten an die Remoteüberwachungslösung. Die Telemetriedaten können auf dem Lösungsdashboard angezeigt werden.

- Navigieren Sie zum Lösungsdashboard.
- Wählen Sie in der Dropdownliste **Anzuzeigendes Gerät** eines der beiden Geräte aus, die Sie im Gateway konfiguriert haben.
- Die Telemetriedaten von den Gatewaygeräten werden auf dem Dashboard angezeigt.

![Anzeigen der Telemetriedaten von den simulierten Gatewaygeräten][img-telemetry-display]

> [!WARNING]
> Wenn Sie die Ausführung der Remoteüberwachungslösung in Ihrem Azure-Konto nicht beenden, wird Ihnen die Ausführungszeit in Rechnung gestellt. Weitere Informationen zur Senkung der Nutzung während der Ausführung der Remoteüberwachungslösung finden Sie unter [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config] (Konfigurieren vorkonfigurierter Azure IoT Suite-Lösungen zu Demonstrationszwecken). Löschen Sie die vorkonfigurierte Lösung aus Ihrem Azure-Konto, wenn Sie sie nicht mehr benötigen.

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie das [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/), in dem Sie weitere Beispiele und Dokumentation zu Azure IoT finden.

[img-simulated telemetry]: ./media/iot-suite-gateway-kit-get-started-simulator/appoutput.png

[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-simulator/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started
