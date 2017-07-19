---
title: Erste Schritte mit Azure IoT Edge (Windows) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein Azure IoT Edge-Gateway auf einem Windows-Computer erstellen, und lernen Sie wichtige Konzepte in Azure IoT Edge kennen, zum Beispiel Module und JSON-Konfigurationsdateien.
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
ms.date: 06/07/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 245ce9261332a3d36a36968f7c9dbc4611a019b2
ms.openlocfilehash: 5db39bab8e31a8e7026b34e72b4614b0f6f57772
ms.contentlocale: de-de
ms.lasthandoff: 06/09/2017


---
# <a name="explore-azure-iot-edge-architecture-on-windows"></a>Untersuchen der Azure IoT Edge-Architektur unter Windows

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-to-run-the-sample"></a>Ausführen des Beispiels

Das Skript **build.cmd** generiert die Ausgabe im Ordner **build** Ihrer lokalen Kopie des Repositorys **iot-edge**. Diese Ausgabe beinhaltet die beiden IoT Edge-Module aus diesem Beispiel.

Das Buildskript platziert **logger.dll** im Ordner **build\\modules\\logger\\Debug** und **hello\_world.dll** im Ordner **build\\modules\\hello_world\\Debug**. Verwenden Sie diese Pfade für die Werte von **module path**, wie in der folgenden JSON-Einstellungsdatei gezeigt.

Der Prozess „hello\_world\_sample“ akzeptiert den Pfad zu einer JSON-Konfigurationsdatei als Befehlszeilenargument. Die folgende JSON-Beispieldatei wird im SDK-Repository unter **samples\\hello\_world\\src\\hello\_world\_win.json** bereitgestellt. Diese Konfigurationsdatei kann in der vorliegenden Form verwendet werden, wenn Sie das Buildskript nicht geändert haben, um die IoT Edge-Module oder ausführbare Beispieldateien an benutzerdefinierten Speicherorten zu speichern.

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

1. Navigieren Sie zum **build**-Ordner im Stamm Ihrer lokalen Kopie des Repositorys **iot-edge**.

1. Führen Sie den folgenden Befehl aus:

    ```cmd
    samples\hello_world\Debug\hello_world_sample.exe ..\samples\hello_world\src\hello_world_win.json
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]

