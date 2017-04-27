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
ms.date: 03/28/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: f3cc32daac5059e816c885c88f4a7d36b6fc897e
ms.lasthandoff: 03/31/2017


---
# <a name="explore-the-iot-gateway-sdk-architecture-on-windows"></a>Untersuchen der IoT Gateway SDK-Architektur unter Windows

[!INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Erstellen des Beispiels

Bevor Sie beginnen, müssen Sie die [Entwicklungsumgebung einrichten][lnk-setupdevbox], um mit dem SDK unter Windows arbeiten zu können.

1. Öffnen Sie eine **Entwicklereingabeaufforderung für VS 2015** oder eine **Entwicklereingabeaufforderung für VS 2017**.
1. Navigieren Sie in Ihrer lokalen Kopie des Repositorys **azure-iot-gateway-sdk** zum Stammordner.
1. Führen Sie das Skript **tools\\build.cmd** aus. Dieses Skript erstellt eine Visual Studio-Projektmappendatei und erstellt die Projektmappe. Sie finden die Visual Studio-Projektmappe im **build**-Ordner Ihrer lokalen Kopie des **azure-iot-gateway-sdk**-Repositorys. Dem Skript können zur Erstellung und Ausführung von Unit- und End-to-End-Tests weitere Parameter hinzugefügt werden. Diese Parameter lauten **--run-unittests** bzw. **--run-e2e-tests**.

## <a name="how-to-run-the-sample"></a>Ausführen des Beispiels

1. Das **build.cmd**-Skript erstellt einen Ordner mit dem Namen **build** in Ihrer lokalen Kopie des Repositorys. Dieser Ordner enthält die beiden in diesem Beispiel verwendeten Module.

    Das Buildskript platziert **logger.dll** im Ordner **build\\modules\\logger\\Debug** und **hello\_world.dll** im Ordner **build\\modules\\hello_world\\Debug**. Verwenden Sie diese Pfade für die Werte von **module path**, wie in der folgenden JSON-Einstellungsdatei gezeigt.
1. Der Prozess „hello\_world\_sample“ akzeptiert den Pfad zu einer JSON-Konfigurationsdatei als Befehlszeilenargument. Die folgende JSON-Beispieldatei wird im SDK-Repository unter **samples\\hello\_world\\src\\hello\_world\_win.json** bereitgestellt. Diese Konfigurationsdatei kann in der vorliegenden Form verwendet werden – es sei denn, Sie haben das Buildskript geändert, um Module oder ausführbare Beispieldateien an benutzerdefinierten Orten zu speichern.

   > [!NOTE]
   > Die Modulpfade gelten relativ zum Verzeichnis, in dem sich „hello\_world\_sample.exe“ befindet. Die JSON-Beispielkonfigurationsdatei schreibt standardmäßig „log.txt“ in Ihr aktuelles Arbeitsverzeichnis.

    ```json
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

1. Navigieren Sie in Ihrer lokalen Kopie des Repositorys **azure-iot-gateway-sdk** zum Stammordner.

1. Führen Sie den folgenden Befehl aus:

   `build\samples\hello_world\Debug\hello_world_sample.exe samples\hello_world\src\hello_world_win.json`

[!INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md

