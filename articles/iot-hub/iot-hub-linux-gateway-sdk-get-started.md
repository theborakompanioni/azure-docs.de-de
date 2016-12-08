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
ms.date: 11/23/2016
ms.author: andbuc
translationtype: Human Translation
ms.sourcegitcommit: a76320718f0cefa015728cb79df944e0d34bbf74
ms.openlocfilehash: cbb909adc2d29f9b80a4c97d06176fe74b64a75a


---
# <a name="azure-iot-gateway-sdk---get-started-using-linux"></a>Azure IoT-Gateway SDK – erste Schritte mit Linux
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
   
    Das Buildskript platziert **liblogger.so** im Ordner **build/modules/logger/** und **libhello_world.so** im Ordner **build/modules/hello_world/**. Verwenden Sie diese Pfade für den Wert **module path** , wie in der JSON-Einstellungsdatei unten gezeigt.
2. Der Prozess „hello_world_sample“ akzeptiert den Pfad zu einer JSON-Konfigurationsdatei als Argument in der Befehlszeile. Eine JSON-Beispieldatei steht im Rahmen des Repositorys unter **azure-iot-gateway-sdk/samples/hello_world/src/hello_world_win.json** zur Verfügung und wurde im Anschluss kopiert. Sie kann in der vorliegenden Form verwendet werden, es sei denn, Sie haben das Buildskript geändert, um Module oder ausführbare Beispieldateien an benutzerdefinierten Orten zu speichern.

   > [!NOTE]
   > Die Modulpfade sind relativ zum aktuellen Arbeitsverzeichnis, in dem die ausführbare Datei für „hello_world_sample“ gestartet wird (nicht relativ zu dem Verzeichnis, in dem sich die ausführbare Datei befindet). Die JSON-Beispielkonfigurationsdatei schreibt standardmäßig „log.txt“ in Ihr aktuelles Arbeitsverzeichnis.
   
    ```
    {
        "modules" :
        [
            {
              "name" : "logger",
              "loader": {
                "name": "native",
                "entrypoint": {
                  "module.path": "./modules/logger/liblogger.so"
                }
              },
              "args" : {"filename":"log.txt"}
            },
            {
                "name" : "hello_world",
              "loader": {
                "name": "native",
                "entrypoint": {
                  "module.path": "./modules/hello_world/libhello_world.so"
                }
              },
                "args" : null
            }
        ],
        "links": 
        [
            {
                "source": "hello_world",
                "sink": "logger"
            }
        ]
    }
    ```
3. Navigieren Sie zum Ordner **azure-iot-gateway-sdk**.
4. Führen Sie den folgenden Befehl aus:
   
   ```
   ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
   ``` 

[!INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md



<!--HONumber=Nov16_HO4-->


