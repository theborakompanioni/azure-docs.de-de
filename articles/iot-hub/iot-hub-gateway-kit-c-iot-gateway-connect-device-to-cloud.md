---
title: "Verbinden eines Geräts mit Azure IoT Hub mithilfe eines IoT-Gateways | Microsoft-Dokumentation"
description: Informationen zum Verwenden von Intel NUC als IoT-Gateway zum Herstellen einer Verbindung mit einem TI SensorTag und Senden von Sensordaten an Azure IoT Hub in der Cloud.
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-Gateway, Gerät mit der Cloud verbinden"
ms.assetid: cb851648-018c-4a7e-860f-b62ed3b493a5
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/06/2017
ms.author: xshi
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 2b772afc946f1637cb8d61c198bc9cf18f42e1ba
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="use-iot-gateway-to-connect-things-to-the-cloud---sensortag-to-azure-iot-hub"></a>Verwenden von IoT-Gateway zum Verbinden von Geräten mit der Cloud: SensorTag mit Azure IoT Hub

> [!NOTE]
> Stellen Sie vor Beginn dieses Tutorials sicher, dass Sie die Schritte unter [Einrichten von Intel NUC als IoT Gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) ausgeführt haben. In [Einrichten von Intel NUC als IoT Gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) richten Sie das Intel NUC Gerät als IoT-Gateway ein.

## <a name="what-you-will-learn"></a>Lernziele

Sie erfahren, wie Sie mithilfe eines IoT-Gateways ein Texas Instruments SensorTag (CC2650STK) mit Azure IoT Hub verbinden. Das IoT-Gateway sendet vom SensorTag gesammelte Temperatur- und Feuchtigkeitsdaten an Azure IoT Hub.

## <a name="what-you-will-do"></a>Aufgaben

- Erstellen Sie einen IoT Hub.
- Registrieren Sie ein Gerät im IoT Hub für das SensorTag.
- Aktivieren Sie die Verbindung zwischen dem IoT-Gateway und SensorTag.
- Führen Sie eine BLE-Beispielanwendung aus, um SensorTag-Daten an Ihren IoT Hub zu senden.

## <a name="what-you-need"></a>Voraussetzungen

- Sie haben das Tutorial [Einrichten von Intel NUC als IoT Gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) abgeschlossen, in dem Sie Intel NUC als IoT-Gateway eingerichtet haben.
- SSH-Client, der auf dem Hostcomputer ausgeführt wird. Unter Windows wird PuTTY empfohlen. Linux und macOS verfügen bereits über einen SSH-Client.
- IP-Adresse und Benutzername samt Kennwort für den Zugriff auf das Gateway vom SSH-Client.
- Internetverbindung.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

> [!NOTE]
> Hier registrieren Sie dieses neue Gerät für Ihr SensorTag

## <a name="enable-the-connection-between-the-iot-gateway-and-the-sensortag"></a>Aktivieren der Verbindung zwischen IoT-Gateway und SensorTag

In diesem Abschnitt führen Sie die folgenden Aufgaben aus:

- Rufen Sie die MAC-Adresse des SensorTag für die Bluetooth-Verbindung ab.
- Stellen Sie eine Bluetooth-Verbindung zwischen IoT-Gateway und SensorTag her.

### <a name="get-the-mac-address-of-the-sensortag-for-bluetooth-connection"></a>Abrufen der MAC-Adresse des SensorTag für die Bluetooth-Verbindung

1. Führen Sie auf dem Hostcomputer den SSH-Client aus, und stellen Sie eine Verbindung mit dem IoT-Gateway her.
1. Heben Sie die Blockierung von Bluetooth auf, indem Sie den folgenden Befehl ausführen:

   ```bash
   sudo rfkill unblock bluetooth
   ```

1. Starten Sie den Bluetooth-Dienst auf dem IoT-Gateway, und öffnen Sie eine Bluetooth-Shell, um Bluetooth durch Ausführen der folgenden Befehle zu konfigurieren:

   ```bash
   sudo systemctl start bluetooth
   bluetoothctl
   ```

1. Schalten Sie den Bluetooth-Controller durch Ausführen des folgenden Befehls an der Bluetooth-Shell ein:

   ```bash
   power on
   ```

   ![Einschalten des Bluetooth-Controllers auf dem IoT-Gateway mit „bluetoothctl“](./media/iot-hub-iot-gateway-connect-device-to-cloud/8_power-on-bluetooth-controller-at-bluetooth-shell-bluetoothctl.png)

1. Starten Sie die Suche nach Bluetooth-Geräten in der Nähe mithilfe des folgenden Befehls:

   ```bash
   scan on
   ```

   ![Suchen von Bluetooth-Geräten in der Nähe mit „bluetoothctl“](./media/iot-hub-iot-gateway-connect-device-to-cloud/9_start-scan-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

1. Drücken Sie die Kopplungstaste auf dem SensorTag. Die grüne LED auf dem SensorTag blinkt.
1. An der Bluetooth-Shell sollte angezeigt werden, dass das SensorTag gefunden wurde. Notieren Sie die MAC-Adresse des SensorTag. Bei diesem Beispiel ist die MAC-Adresse des SensorTag `24:71:89:C0:7F:82`.
1. Beenden Sie die Suche, indem Sie den folgenden Befehl ausführen:

   ```bash
   scan off
   ```

   ![Beenden der Suche von Bluetooth-Geräten in der Nähe mit „bluetoothctl“](./media/iot-hub-iot-gateway-connect-device-to-cloud/10_stop-scanning-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

### <a name="initiate-a-bluetooth-connection-from-the-iot-gateway-to-the-sensortag"></a>Herstellen einer Bluetooth-Verbindung zwischen IoT-Gateway und SensorTag

1. Stellen Sie eine Verbindung mit dem SensorTag her, indem Sie folgenden Befehl ausführen:

   ```bash
   connect <MAC address>
   ```

   ![Herstellen einer Verbindung mit SensorTag mit „bluetoothctl“](./media/iot-hub-iot-gateway-connect-device-to-cloud/11_connect-to-sensortag-at-bluetooth-shell-bluetoothctl.png)

1. Trennen Sie die Verbindung mit dem SensorTag, und beenden Sie die Bluetooth-Shell mithilfe des folgenden Befehls:

   ```bash
   disconnect
   exit
   ```

   ![Trennen der Verbindung mit SensorTag mit „bluetoothctl“](./media/iot-hub-iot-gateway-connect-device-to-cloud/12_disconnect-from-sensortag-at-bluetooth-shell-bluetoothctl.png)

Sie haben die Verbindung zwischen IoT-Gateway und SensorTag erfolgreich aktiviert.

## <a name="run-a-ble-sample-application-to-send-sensortag-data-to-your-iot-hub"></a>Führen Sie eine BLE-Beispielanwendung aus, um SensorTag-Daten an Ihren IoT Hub zu senden.

Die Beispielanwendung „Bluetooth Low Energy (BLE)“ wird von Azure IoT Edge bereitgestellt. Die Beispielanwendung erfasst Daten zur BLE-Verbindung und sendet Daten zu Ihrem IoT Hub. Zum Ausführen der Beispielanwendung müssen Sie wie folgt vorgehen:

1. Konfigurieren Sie die Beispielanwendung.
1. Führen Sie die Beispielanwendung auf dem IoT-Gateway aus.

### <a name="configure-the-sample-application"></a>Konfigurieren der Beispielanwendung

1. Wechseln Sie durch Ausführen des folgenden Befehls zum Ordner der Beispielanwendung:

   ```bash
   cd /user/share/azureiotgatewaysdk/samples
   ```

1. Öffnen Sie die Konfigurationsdatei, indem Sie den folgenden Befehl ausführen:

   ```bash
   vi ble_gateway.json
   ```

1. Geben Sie in der Konfigurationsdatei die folgenden Werte ein:

   **IoTHubName**: Der Name Ihres IoT Hubs.

   **IoTHubSuffix**: Rufen Sie den Wert von „IoTHubSuffix“ aus dem Primärschlüssel der Verbindungszeichenfolge des Geräts ab, die Sie notiert haben. Stellen Sie sicher, dass Sie den Primärschlüssel aus der Verbindungszeichenfolge des Geräts und nicht den Primärschlüssel aus der IoT Hub-Verbindungszeichenfolge abrufen. Der Primärschlüssel der Verbindungszeichenfolge des Geräts hat das Format `HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY`.

   **Transport**: Der Standardwert ist `amqp`. Dieser Wert zeigt das Protokoll während des Transports. Möglich sind `http`, `amqp` oder `mqtt`.

   **MacAddress**: Die MAC-Adresse des SensorTag, die Sie notiert haben.

   **deviceID**: ID des Geräts, das Sie in Ihrem IoT Hub erstellt haben.

   **deviceKey**: Der Primärschlüssel der Verbindungszeichenfolge des Geräts.

   ![Vervollständigen der Konfigurationsdatei der BLE-Beispielanwendung](./media/iot-hub-iot-gateway-connect-device-to-cloud/13_edit-config-file-of-ble-sample.png)

1. Drücken Sie `ESC`, und geben Sie zum Speichern der Datei `:wq` ein.

### <a name="run-the-sample-application"></a>Ausführen der Beispielanwendung

1. Stellen Sie sicher, dass das SensorTag eingeschaltet ist.
1. Führen Sie den folgenden Befehl aus:

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a>Nächste Schritte

[Verwenden des IoT-Gateways für die Transformation von Sensordaten mit Azure IoT Edge](iot-hub-gateway-kit-c-use-iot-gateway-for-data-conversion.md)

