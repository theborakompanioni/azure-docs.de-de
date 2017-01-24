---
title: Erste Schritte mit dem Azure IoT Gateway SDK (Windows) | Microsoft Docs
description: Hier finden Sie Informationen zum Erstellen eines Gateways auf einem Windows-Computer und erfahren mehr zu wichtigen Konzepten im Azure IoT Gateway SDK wie Modulen und JSON-Konfigurationsdateien.
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 9aff3724-5e4e-40ec-b95a-d00df4f590c5
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: andbuc
translationtype: Human Translation
ms.sourcegitcommit: e223d0613cd48994315451da87e6b7066585bdb6
ms.openlocfilehash: 3493afbebf2a75a190d344ac1e66b00b9ddfd582


---
# <a name="get-started-with-the-azure-iot-gateway-sdk-windows"></a>Erste Schritte mit dem Azure IoT Gateway SDK (Windows)
[!INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Erstellen des Beispiels
Bevor Sie beginnen, müssen Sie die [Entwicklungsumgebung einrichten][lnk-setupdevbox], um mit dem SDK unter Windows arbeiten zu können.

1. Öffnen Sie eine **Developer-Eingabeaufforderung für VS2015** .
2. Navigieren Sie in Ihrer lokalen Kopie des Repositorys **azure-iot-gateway-sdk** zum Stammordner.
3. Führen Sie das Skript **tools\\build.cmd** aus. Dieses Skript erstellt eine Visual Studio-Projektmappendatei, erstellt die Projektmappe und führt die Tests aus. Sie finden die Visual Studio-Projektmappe im **build**-Ordner Ihrer lokalen Kopie des **azure-iot-gateway-sdk**-Repositorys.

## <a name="how-to-run-the-sample"></a>Ausführen des Beispiels
1. Das **build.cmd**-Skript erstellt einen Ordner mit dem Namen **build** in Ihrer lokalen Kopie des Repositorys. Dieser Ordner enthält die beiden in diesem Beispiel verwendeten Module.
   
    Das Buildskript platziert **logger.dll** im Ordner **build\\modules\\logger\\Debug** und **hello_world.dll** im Ordner **build\\modules\\hello_world\\Debug**. Verwenden Sie diese Pfade für den Wert **module path**, wie in der folgenden JSON-Einstellungsdatei gezeigt.
2. Der Prozess „hello_world_sample“ akzeptiert den Pfad zu einer JSON-Konfigurationsdatei als Argument in der Befehlszeile. Die folgende JSON-Beispieldatei steht im Rahmen des Repositorys unter **azure-iot-gateway-sdk\samples\hello_world\src\hello_world_win.json** zur Verfügung. Sie kann in der vorliegenden Form verwendet werden, es sei denn, Sie haben das Buildskript geändert, um Module oder ausführbare Beispieldateien an benutzerdefinierten Orten zu speichern. 

   > [!NOTE]
   > Die Modulpfade gelten relativ zum Verzeichnis, in dem sich „hello_world_sample.exe“ befindet. Die JSON-Beispielkonfigurationsdatei schreibt standardmäßig „log.txt“ in Ihr aktuelles Arbeitsverzeichnis.
   
    ```
    {
      "modules": [
        {
          "name": "logger",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "..\\..\\..\\modules\\logger\\Debug\\logger.dll"
            }
          },
          "args": { "filename": "log.txt" }
        },
        {
          "name": "hello_world",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "..\\..\\..\\modules\\hello_world\\Debug\\hello_world.dll"
            }
          },
          "args": null
          }
      ],
      "links": [
        {
          "source": "hello_world",
          "sink": "logger"
        }
      ]
    }
    ```
3. Navigieren Sie in Ihrer lokalen Kopie des Repositorys **azure-iot-gateway-sdk** zum Stammordner.

4. Führen Sie den folgenden Befehl aus:
   
   ```
   build\samples\hello_world\Debug\hello_world_sample.exe samples\hello_world\src\hello_world_win.json
   ```

[!INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md



<!--HONumber=Dec16_HO1-->


