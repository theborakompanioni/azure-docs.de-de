---
title: Erste Schritte mit Azure IoT Edge (Linux) | Microsoft-Dokumentation
description: Hier finden Sie Informationen zum Erstellen eines Gateways auf einem Linux-Computer und erfahren mehr zu wichtigen Konzepten in Azure IoT Edge wie Module und JSON-Konfigurationsdateien.
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: cf537bdd-2352-4bb1-96cd-a283fcd3d6cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 4f77c7a615aaf5f87c0b260321f45a4e7129f339
ms.openlocfilehash: e2f26c1abe09feff77e1e2633d8bfcf4ca527aee
ms.contentlocale: de-de
ms.lasthandoff: 09/22/2017

---
# <a name="explore-azure-iot-edge-architecture-on-linux"></a>Untersuchen der Azure IoT Edge-Architektur unter Linux

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-to-run-the-sample"></a>Ausführen des Beispiels

Das Skript **build.sh** generiert die Ausgabe im Ordner **build** Ihrer lokalen Kopie des Repositorys **iot-edge**. Diese Ausgabe beinhaltet die beiden IoT Edge-Module aus diesem Beispiel.

Das Buildskript platziert **liblogger.so** im Ordner **build/modules/logger/** und **libhello\_world.so** im Ordner **build/modules/hello_world/**. Verwenden Sie diese Pfade für die**module path**-Werte, wie in der folgenden JSON-Beispieleinstellungsdatei gezeigt.

Der Prozess „hello\_world\_sample“ akzeptiert den Pfad zu einer JSON-Konfigurationsdatei als Befehlszeilenargument. Die folgende JSON-Beispieldatei wird im SDK-Repository unter **samples/hello\_world/src/hello\_world\_lin.json** bereitgestellt. Diese Konfigurationsdatei kann in der vorliegenden Form verwendet werden, wenn Sie das Buildskript nicht geändert haben, um die IoT Edge-Module oder ausführbare Beispieldateien an benutzerdefinierten Speicherorten zu speichern.

> [!NOTE]
> Die Modulpfade sind relativ zum aktuellen Arbeitsverzeichnis, in dem die ausführbare Datei für „hello\_world\_sample“ gestartet wird (nicht relativ zu dem Verzeichnis, in dem sich die ausführbare Datei befindet). Die JSON-Beispielkonfigurationsdatei schreibt standardmäßig „log.txt“ in Ihr aktuelles Arbeitsverzeichnis.

```json
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

1. Navigieren Sie zum **build**-Ordner im Stamm Ihrer lokalen Kopie des Repositorys **iot-edge**.

1. Führen Sie den folgenden Befehl aus:

    ```sh
    ./samples/hello_world/hello_world_sample ../samples/hello_world/src/hello_world_lin.json`
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]


