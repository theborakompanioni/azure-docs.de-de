---
title: Erste Schritte mit dem IoT Hub Gateway SDK | Microsoft Docs
description: "Diese exemplarische Vorgehensweise für das Azure IoT Gateway SDK veranschaulicht anhand von Linux die grundlegenden Konzepte, mit denen Sie in Verbindung mit dem Azure IoT Gateway SDK vertraut sein sollten."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: cf537bdd-2352-4bb1-96cd-a283fcd3d6cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2016
ms.author: andbuc
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 23176a9251a90a985a5d2fbce23ceeb9d0925234


---
# <a name="azure-iot-gateway-sdk-beta-get-started-using-linux"></a>Azure IoT Gateway SDK (Beta) – erste Schritte mit Linux
[!INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Erstellen des Beispiels
Bevor Sie beginnen, müssen Sie die [Entwicklungsumgebung einrichten][lnk-setupdevbox], um mit dem SDK unter Linux arbeiten zu können.

1. Öffnen Sie eine Shell.
2. Navigieren Sie in Ihrer lokalen Kopie des Repositorys **azure-iot-gateway-sdk** zum Stammordner.
3. Führen Sie das Skript **tools/build.sh** aus. Dieses Skript verwendet das Hilfsprogramm **cmake**, um einen Ordner namens **build** im Stammordner Ihrer lokalen Kopie des **azure-iot-gateway-sdk**-Repositorys zu erstellen und ein Makefile zu generieren. Das Skript erstellt dann die Lösung und führt die Tests aus.

> [!NOTE]
> Bei jeder Ausführung löscht das Skript **build.sh** den Ordner **build** im Stammordner Ihrer lokalen Kopie des **azure-iot-gateway-sdk**-Repositorys und erstellt ihn neu.
> 
> 

## <a name="how-to-run-the-sample"></a>Ausführen des Beispiels
1. Das **build.sh**-Skript generiert die Ausgabe im **build**-Ordner Ihrer lokalen Kopie des **azure-iot-gateway-sdk**-Repositorys. Hierzu gehören die beiden in diesem Beispiel verwendeten Module.
   
    Das Buildskript platziert **liblogger_hl.so** im Ordner **build/modules/logger/** und **libhello_world_hl.so** im Ordner **build/modules/hello_world/**. Verwenden Sie diese Pfade für den Wert **module path** , wie in der JSON-Einstellungsdatei unten gezeigt.
2. Die Datei **hello_world_lin.json** im Ordner **samples/hello_world/src** ist eine JSON-Beispieleinstellungsdatei für Linux, die Sie zur Ausführung des Beispiels verwenden können. Bei den unten gezeigten beispielhaften JSON-Einstellungen wird angenommen, dass Sie das Beispiel aus dem Stammordner Ihrer lokalen Kopie des **azure-iot-gateway-sdk** -Repositorys ausführen.
3. Legen Sie im Modul **logger_hl** im Abschnitt **args** den Wert **filename** auf den Namen und Pfad der Datei fest, in der die Protokolldaten gespeichert werden.
   
    Dies ist ein Beispiel für eine JSON-Einstellungsdatei für Linux, die in die Datei **log.txt** in dem Ordner schreibt, in dem Sie das Beispiel ausführen.
   
    ```
    {
      "modules" :
      [ 
        {
          "module name" : "logger_hl",
          "loading args": {
            "module path" : "./build/modules/logger/liblogger_hl.so"
          },
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "loading args": {
            "module path" : "./build/modules/hello_world/libhello_world_hl.so"
          },
          "args" : null
        }
      ],
      "links" :
      [
        {
          "source": "hello_world",
          "sink": "logger_hl"
        }
      ]
    }
    ```
4. Navigieren Sie in der Shell zum Ordner **azure-iot-gateway-sdk** .
5. Führen Sie den folgenden Befehl aus:
   
   ```
   ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
   ``` 

[!INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md



<!--HONumber=Nov16_HO2-->


