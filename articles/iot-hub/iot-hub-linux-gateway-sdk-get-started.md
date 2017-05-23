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
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: e7da3c6d4cfad588e8cc6850143112989ff3e481
ms.openlocfilehash: c401052d65cb9f06398fcf6597dc1de5e7451734
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="explore-azure-iot-edge-architecture-on-linux"></a>Untersuchen der Azure IoT Edge-Architektur unter Linux

[!INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Erstellen des Beispiels

Bevor Sie beginnen, müssen Sie die [Entwicklungsumgebung einrichten][lnk-setupdevbox], um mit dem SDK unter Linux arbeiten zu können.

1. Öffnen Sie eine Shell.
1. Navigieren Sie in Ihrer lokalen Kopie des Repositorys **iot-edge** zum Stammordner.
1. Führen Sie das Skript **tools/build.sh** aus. Dieses Skript verwendet das Hilfsprogramm **cmake**, um einen Ordner namens **build** im Stammordner Ihrer lokalen Kopie des Repositorys **iot-edge** zu erstellen und ein Makefile zu generieren. Das Skript erstellt dann die Lösung und überspringt die Komponententests und End-to-End-Tests. Fügen Sie den Parameter **--run-unittests** hinzu, wenn Sie die Komponententests erstellen und ausführen möchten. Fügen Sie **--run-e2e-tests** hinzu, wenn Sie die End-to-End-Tests erstellen und ausführen möchten.

> [!NOTE]
> Bei jeder Ausführung löscht das Skript **build.sh** den Ordner **build** im Stammordner Ihrer lokalen Kopie des Repositorys **iot-edge** und erstellt ihn neu.

## <a name="how-to-run-the-sample"></a>Ausführen des Beispiels

1. Das Skript **build.sh** generiert die Ausgabe im Ordner **build** Ihrer lokalen Kopie des Repositorys **iot-edge**. Diese Ausgabe beinhaltet die beiden IoT Edge-Module aus diesem Beispiel.

    Das Buildskript platziert **liblogger.so** im Ordner **build/modules/logger/** und **libhello\_world.so** im Ordner **build/modules/hello_world/**. Verwenden Sie diese Pfade für den Wert **module path**, wie in der folgenden JSON-Beispieleinstellungsdatei gezeigt.
1. Der Prozess „hello\_world\_sample“ akzeptiert den Pfad zu einer JSON-Konfigurationsdatei als Befehlszeilenargument. Die folgende JSON-Beispieldatei wird im SDK-Repository unter **samples/hello\_world/src/hello\_world\_lin.json** bereitgestellt. Diese Konfigurationsdatei kann in der vorliegenden Form verwendet werden – es sei denn, Sie haben das Buildskript geändert, um IoT Edge-Module oder ausführbare Beispieldateien an benutzerdefinierten Speicherorten zu speichern.

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
1. Navigieren Sie zum Ordner **build**.
1. Führen Sie den folgenden Befehl aus:

   `./samples/hello_world/hello_world_sample ./../samples/hello_world/src/hello_world_lin.json`

[!INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/iot-edge/blob/master/doc/devbox_setup.md

