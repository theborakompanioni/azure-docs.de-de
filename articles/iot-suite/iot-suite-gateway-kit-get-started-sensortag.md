---
title: "Verbinden eines Gateways mit Azure IoT Suite über einen Intel NUC | Microsoft-Dokumentation"
description: "Verwenden Sie das Microsoft IoT Commercial Gateway Kit und die vorkonfigurierte Remoteüberwachungslösung. Verwenden Sie das Azure IoT Edge-Gateway, um es einem SensorTag-Gerät zu ermöglichen, eine Verbindung mit der Remoteüberwachungslösung herzustellen, Telemetriedaten an die Cloud zu senden sowie auf Methoden zu reagieren, die über das Lösungsdashboard aufgerufen wurden."
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
ms.date: 05/05/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: e7da3c6d4cfad588e8cc6850143112989ff3e481
ms.openlocfilehash: 03c8e18636e1182b301e1bcf418c727265d8b478
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="connect-your-azure-iot-edge-gateway-to-the-remote-monitoring-preconfigured-solution-and-send-telemetry-from-a-sensortag"></a>Verbinden Ihres Azure IoT Edge-Gateways mit der vorkonfigurierten Remoteüberwachungslösung und Senden von Telemetriedaten von einem SensorTag

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

In diesem Tutorial wird gezeigt, wie Azure IoT Edge verwendet wird, um Temperatur- und Feuchtigkeitsdaten von einem SensorTag-Gerät an die vorkonfigurierte Remoteüberwachungslösung zu senden. Das SensorTag-Gerät wird über Bluetooth mit dem Intel NUC-Gateway verbunden. In diesem Tutorial wird Folgendes verwendet:

- Azure IoT Edge, um ein Beispielgateway zu implementieren.
- Die von IoT Suite vorkonfigurierte Remoteüberwachungslösung als cloudbasiertes Back-End.

## <a name="overview"></a>Übersicht

In diesem Tutorial führen Sie folgende Schritte aus:

- Bereitstellen einer Instanz der vorkonfigurierten Remoteüberwachungslösung für Ihr Azure-Abonnement. Durch diesen Schritt werden automatisch mehrere Azure-Dienste bereitgestellt und konfiguriert.
- Einrichten Ihres Intel NUC-Gatewaygeräts für die Kommunikation mit Ihrem Computer und der Remoteüberwachungslösung.
- Einrichten Ihres Intel NUC-Gateways so, dass es Telemetriedaten von einem SensorTag-Gerät empfängt und diese Daten an das Dashboard für die Remoteüberwachung sendet.

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

[Texas Instruments BLE SensorTag][lnk-sensortag]. In diesem Tutorial werden über Bluetooth Telemetriedaten empfangen, die vom SensorTag-Gerät stammen.

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> Die Remoteüberwachungslösung stellt eine Gruppe von Azure-Diensten im Azure-Abonnement bereit. Die Bereitstellung gibt eine echte Unternehmensarchitektur wieder. Um unnötige Azure-Nutzungsgebühren zu vermeiden, löschen Sie Ihre Instanz der vorkonfigurierten Lösung auf „azureiotsuite.com“, sobald Sie damit fertig sind. Wenn Sie die vorkonfigurierte Lösung erneut benötigen, können Sie sie problemlos wiederherstellen. Weitere Informationen zur Senkung der Nutzung während der Ausführung der Remoteüberwachungslösung finden Sie unter [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config] (Konfigurieren vorkonfigurierter Azure IoT Suite-Lösungen zu Demonstrationszwecken).

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

## <a name="configure-bluetooth-connectivity"></a>Konfigurieren der Bluetooth-Konnektivität

Konfigurieren Sie Bluetooth auf dem Intel NUC, damit das SensorTag-Gerät eine Verbindung herstellen und Telemetriedaten senden kann.

### <a name="find-the-mac-address-of-the-sensortag"></a>Suchen nach der MAC-Adresse des SensorTag-Geräts

1. Führen Sie in der Shell auf dem Intel NUC den folgenden Befehl aus, um den Bluetooth-Diensts zu entsperren:

    ```bash
    sudo rfkill unblock bluetooth
    ```

1. Führen Sie die folgenden Befehle aus, um den Bluetooth-Dienst auf dem Intel NUC zu starten und die Bluetooth-Shell zu öffnen:

    ```bash
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. Führen Sie den folgenden Befehl aus, um den Bluetooth-Controller zu aktivieren:

    ```bash
    power on
    ```

    Wenn der Controller aktiviert ist, sehen Sie die Meldung **Changing power on succeeded**.

1. Führen Sie den folgenden Befehl aus, um nach Bluetooth-Geräten in der Nähe zu suchen:

    ```bash
    scan on
    ```

1. Drücken Sie den Netzschalter auf dem SensorTag-Gerät, damit es entdeckt werden kann. Die grüne LED blinkt.

1. Wird in der Shell in einer Meldung mitgeteilt, dass der Controller das SensorTag-Gerät entdeckt hat, notieren Sie sich die MAC-Adresse des Geräts. Die MAC-Adresse sieht wie folgt aus: **A0:E6:F8:B5:F6:00**. Sie benötigen die MAC-Adresse später in diesem Tutorial, wenn Sie das Gateway konfigurieren.

1. Führen Sie den folgenden Befehl aus, um die Bluetooth-Suche zu deaktivieren:

    ```bash
    scan off
    ```

1. Führen Sie den folgenden Befehl aus, um sich zu vergewissern, dass eine Verbindung mit dem SensorTag-Gerät hergestellt werden kann:

    ```bash
    connect <SensorTag MAC address>
    ```

    Wurde die Verbindung erfolgreich hergestellt, zeigt die Shell die Meldung **Connection successful** und Informationen über das SensorTag-Gerät an. Konnte keine Verbindung hergestellt werden, überprüfen Sie, ob die SensorTag-Gerät noch eingeschaltet ist.

1. Sie können nun die folgenden Befehle ausführen, um die Verbindung mit dem SensorTag-Gerät zu trennen und die Bluetooth-Shell zu beenden:

    ```bash
    disconnect
    exit
    ```

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
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

Das Buildskript platziert das benutzerdefinierte IoT Edge-Modul „libsensor2remotemonitoring.so“ im Ordner für Builds.

## <a name="configure-and-run-the-iot-edge-gateway"></a>Konfigurieren und Ausführen des IoT Edge-Gateways

Sie können nun das IoT Edge-Gateway so konfigurieren, dass es Telemetriedaten von Ihrem SensorTag-Gerät an Ihr Remoteüberwachungsdashboard sendet. Weitere Informationen zum Konfigurieren eines Gateways und von IoT Edge-Modulen finden Sie unter [Azure IoT Edge-Konzepte][lnk-gateway-concepts].

> [!TIP]
> In diesem Tutorial verwenden Sie den Standard-Text-Editor `vi` auf dem Intel NUC. Wenn Sie noch nie mit `vi` gearbeitet haben, sollten Sie ein einführendes Tutorial, z. B. [Unix - The vi Editor Tutorial][lnk-vi-tutorial] durcharbeiten, um sich mit diesem Editor vertraut zu machen. Alternativ können Sie den benutzerfreundlicheren Editor [nano](https://www.nano-editor.org/) mit dem Befehl `smart install nano -y` installieren.

Öffnen Sie die Beispielkonfigurationsdatei im Editor **vi** mit dem folgenden Befehl:

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic/remote_monitoring.json
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
    "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>>"
  }
]
```

Ersetzen Sie den Platzhalter **macAddress** durch die MAC-Adresse Ihres SensorTag-Geräts, die Sie zuvor notiert haben. Ersetzen Sie die Platzhalter **deviceID** und **deviceKey** durch die IDs und Schlüssel für die beiden Geräte, die Sie zuvor in der Remoteüberwachungslösung erstellt haben.

Suchen Sie nach den folgenden Zeilen in der Konfiguration für das Modul „SensorTag“:

```json
"args": {
  "controller_index": 0,
  "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
  ...
}
```

Ersetzen Sie den Platzhalter **device\_mac\_address** durch die MAC-Adresse Ihres SensorTag-Geräts, die Sie zuvor notiert haben.

Speichern Sie die Änderungen.

Sie können das Gateway jetzt mit den folgenden Befehlen ausführen:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
/usr/share/azureiotgatewaysdk/samples/ble_gateway/ble_gateway remote_monitoring.json
```

Das IoT Edge-Gateway wird auf dem Intel NUC gestartet und sendet Telemetriedaten vom SensorTag-Gerät an die Remoteüberwachungslösung:

![IoT Edge-Gateway sendet Telemetriedaten vom SensorTag-Gerät.][img-telemetry]

Drücken Sie **STRG+C**, um das Programm zu einem beliebigen Zeitpunkt zu beenden.

## <a name="view-the-telemetry"></a>Anzeigen der Telemetrie

Das Gateway sendet jetzt Telemetriedaten vom SensorTag-Gerät an die Remoteüberwachungslösung. Die Telemetriedaten können auf dem Lösungsdashboard angezeigt werden. Sie können auch Befehle vom Lösungsdashboard über das Gateway an Ihr SensorTag-Gerät senden.

- Navigieren Sie zum Lösungsdashboard.
- Wählen Sie in der Dropdownliste **Anzuzeigendes Gerät** das Gerät aus, das Sie im Gateway als Entsprechung des SensorTag-Geräts konfiguriert haben.
- Die Telemetriedaten vom SensorTag-Gerät werden auf dem Dashboard angezeigt.

![Anzeigen der Telemetriedaten von den SensorTag-Geräten][img-telemetry-display]

> [!WARNING]
> Wenn Sie die Ausführung der Remoteüberwachungslösung in Ihrem Azure-Konto nicht beenden, wird Ihnen die Ausführungszeit in Rechnung gestellt. Weitere Informationen zur Senkung der Nutzung während der Ausführung der Remoteüberwachungslösung finden Sie unter [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config] (Konfigurieren vorkonfigurierter Azure IoT Suite-Lösungen zu Demonstrationszwecken). Löschen Sie die vorkonfigurierte Lösung aus Ihrem Azure-Konto, wenn Sie sie nicht mehr benötigen.


## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie das [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/), in dem Sie weitere Beispiele und Dokumentation zu Azure IoT finden.

[img-telemetry]: ./media/iot-suite-gateway-kit-get-started-sensortag/appoutput.png


[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-sensortag/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-sensortag]: http://www.ti.com/ww/en/wireless_connectivity/sensortag/
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started
