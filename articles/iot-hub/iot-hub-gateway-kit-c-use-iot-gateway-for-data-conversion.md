---
title: Datenkonvertierung auf IoT-Gateway mit Azure IoT Edge | Microsoft-Dokumentation
description: "Verwenden Sie IoT-Gateway, um das Format der Sensordaten über ein benutzerdefiniertes Modul aus Azure IoT Edge zu konvertieren."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IoT-Gateway-Datenkonvertierung, IoT-Gateway-Datentransformation
ms.assetid: 75f2573d-500b-4405-bff7-61021c4c3500
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.translationtype: Human Translation
ms.sourcegitcommit: cb4d075d283059d613e3e9d8f0a6f9448310d96b
ms.openlocfilehash: 5f5151c9e250fb8a19a953c6212dd2675004dc55
ms.contentlocale: de-de
ms.lasthandoff: 06/26/2017

---
# <a name="use-iot-gateway-for-sensor-data-transformation-with-azure-iot-edge"></a>Verwenden des IoT-Gateways für die Transformation von Sensordaten mit Azure IoT Edge

> [!NOTE]
> Stellen Sie vor Beginn dieses Tutorials sicher, dass Sie die folgenden Lektionen der Reihe nach ausgeführt haben:
> * [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) (Einrichten von Intel NUC als IoT Gateway)
> * [Verwenden von IoT-Gateway zum Verbinden von Geräten mit der Cloud: SensorTag mit Azure IoT Hub](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

Ein Zweck eines Iot-Gateways ist das Verarbeiten gesammelter Daten vor dem Senden an die Cloud. Azure IoT Edge führt Module ein, die erstellt und zusammengesetzt werden können, um den Workflow der Datenverarbeitung zu bilden. Ein Modul empfängt eine Nachricht, führt eine Aktion daran aus und gibt sie zur Verarbeitung an andere Module weiter.

## <a name="what-you-learn"></a>Lerninhalt

Sie erfahren, wie Sie ein Modul erstellen, um Nachrichten von SensorTag in ein anderes Format zu konvertieren.

## <a name="what-you-do"></a>Aufgaben

* Erstellen Sie ein Modul, um eine empfangene Nachricht in das JSON-Format zu konvertieren.
* Kompilieren Sie das Modul.
* Fügen Sie das Modul der BLE-Beispielanwendung aus Azure IoT Edge hinzu.
* Führen Sie die Beispielanwendung aus.

## <a name="what-you-need"></a>Erforderliches Element

* Sie müssen die folgenden Tutorials der Reihe nach ausgeführt haben:
  * [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) (Einrichten von Intel NUC als IoT Gateway)
  * [Verwenden von IoT-Gateway zum Verbinden von Geräten mit der Cloud: SensorTag mit Azure IoT Hub](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)
* Ein SSH-Client, der auf Ihrem Hostcomputer ausgeführt wird. Unter Windows wird PuTTY empfohlen. Linux und macOS verfügen bereits über einen SSH-Client.
* IP-Adresse und Benutzername samt Kennwort für den Zugriff auf das Gateway vom SSH-Client.
* Eine Internetverbindung.

## <a name="create-a-module"></a>Erstellen eines Moduls

1. Führen Sie auf dem Hostcomputer den SSH-Client aus, und stellen Sie eine Verbindung mit dem IoT-Gateway her.
1. Klonen Sie mithilfe des folgenden Befehls die Quelldateien des Konvertierungsmoduls von GitHub in das Stammverzeichnis des IoT-Gateways:

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-nuc-gateway-customized-module.git
   ```

   Dies ist ein in der Programmiersprache C geschriebenes natives Azure IoT Edge-Modul. Das Modul konvertiert das Format der empfangenen Nachrichten in das folgende Format:

   ```json
   {"deviceId": "Intel NUC Gateway", "messageId": 0, "temperature": 0.0}
   ```

## <a name="compile-the-module"></a>Kompilieren des Moduls

Kompilieren Sie das Modul mit folgendem Befehl:

```bash
cd iot-hub-c-intel-nuc-gateway-customized-module/my_module
# change the build script runnable
chmod 777 build.sh
# remove the invalid windows character
sed -i -e "s/\r$//" build.sh
# run the build shell script
./build.sh
```

Nach Abschluss der Kompilierung erhalten Sie eine `libmy_module.so`-Datei. Notieren Sie sich den absoluten Pfad dieser Datei.

## <a name="add-the-module-to-the-ble-sample-application"></a>Hinzufügen des Moduls zur BLE-Beispielanwendung

1. Wechseln Sie mit folgendem Befehl zum Beispieleordner:

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples
   ```

1. Öffnen Sie mit folgendem Befehl die Konfigurationsdatei:

   ```bash
   vi ble_gateway.json
   ```

1. Fügen Sie ein Modul hinzu, indem Sie den folgenden Code in den `modules`-Abschnitt einfügen.

   ```json
   {
       "name": "MyModule",
       "loader": {
           "name": "native",
           "entrypoint":{
               "module.path": "[Your libmy_module.so path]"
               }
            },
        "args": null
    },
    ```

1. Ersetzen Sie `[Your libmy_module.so path]` im Code durch den absoluten Pfad der libmy_module.so`-Datei.
1. Ersetzen Sie den Code im `links`-Abschnitt durch den folgenden Code:

   ```json
   {
       "source": "SensorTag",
       "sink": "MyModule"
   },
   {
       "source": "MyModule",
       "sink": "mapping"
   }
   ```

1. Drücken Sie `ESC`, und geben Sie `:wq` zum Speichern der Datei ein.

## <a name="run-the-sample-application"></a>Ausführen der Beispielanwendung

1. Schalten Sie SensorTag ein.
1. Legen Sie die Umgebungsvariable SSL_CERT_FILE mit folgendem Befehl fest:

   ```bash
   export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
   ```

1. Führen Sie die Beispielanwendung mit dem hinzugefügten Modul mit folgendem Befehl aus:

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a>Nächste Schritte

Sie haben das IoT-Gateway erfolgreich verwendet, um die Nachricht von SensorTag in das JSON-Format zu konvertieren.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

