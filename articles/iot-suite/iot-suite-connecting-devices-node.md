---
title: "Herstellen einer Geräteverbindung mit Node.js | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie ein Gerät mit der vorkonfigurierten Remoteüberwachungslösung von Azure IoT Suite verbinden, indem Sie eine in Node.js geschriebene Anwendung ausführen."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: fc50a33f-9fb9-42d7-b1b8-eb5cff19335e
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 02247d679c9bffa01b8d2827ea57eba38894107e
ms.openlocfilehash: 698384bb5079feb937f423f9b6365dff20cfa3be


---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung (Node.js)
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-nodejs-sample-solution"></a>Erstellen und Ausführen der node.js-Beispiellösung
1. Um das GitHub-Repository *Microsoft Azure IoT SDK für Node.js* zu klonen und zu installieren, führen Sie in Ihrer Desktopumgebung die Anweisungen unter [Vorbereiten Ihrer Entwicklungsumgebung][lnk-github-prepare] aus.
2. Kopieren Sie in Ihrer lokalen Kopie des Repositorys [azure-iot-sdk-node][lnk-github-repo] die folgenden beiden Dateien aus dem Ordner „device/samples“ in einen Ordner auf Ihrem Gerät:
   
   * package.json
   * remote_monitoring.js
3. Öffnen Sie die Datei „remote_monitoring.js“, und suchen Sie nach der folgenden Variablen:
   
    ```
    var connectionString = "[IoT Hub device connection string]";
    ```
4. Ersetzen Sie **[IoT Hub device connection string]** durch Ihre Geräteverbindungszeichenfolge. Die Werte für IoT Hub-Hostname, Geräte-ID und Geräteschlüssel finden Sie auf dem Dashboard der Remoteüberwachungslösung. Eine Geräteverbindungszeichenfolge hat folgendes Format:
   
    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```
   
    Wenn der IoT Hub-Hostname **contoso** und die Geräte-ID **mydevice** lautet, sieht Ihre Verbindungszeichenfolge wie folgt aus:
   
    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```
5. Speichern Sie die Datei . Führen Sie die folgenden Befehle in der Eingabeaufforderung im Ordner mit diesen Dateien ein, um die erforderlichen Pakete zu installieren. Führen Sie dann die Beispielanwendung aus:
   
    ```
    npm install --save
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdk-node
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md



<!--HONumber=Dec16_HO3-->


