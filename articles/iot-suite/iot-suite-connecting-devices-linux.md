---
title: "Anschließen eines Geräts mit C unter Linux | Microsoft Docs"
description: "Es wird beschrieben, wie Sie ein Gerät mit der vorkonfigurierten Remoteüberwachungslösung von Azure IoT Suite verbinden, indem Sie eine in C geschriebene Anwendung unter Linux ausführen."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0c7c8039-0bbf-4bb5-9e79-ed8cff433629
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/05/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: c844f9a84878f12fbdf7a5b4eaf7bbf19f4ce666
ms.openlocfilehash: 4a1615c4bea8c54d506c3252e2de42642bb55e46
ms.contentlocale: de-de
ms.lasthandoff: 02/27/2017


---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-linux"></a>Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung (Linux)
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-a-sample-c-client-linux"></a>Erstellen und Ausführen eines in C geschriebenen Beispielclients für Linux
Die folgenden Schritte zeigen, wie Sie eine Clientanwendung erstellen, die mit der vorkonfigurierten Lösung für die Remoteüberwachung kommuniziert. Diese Anwendung wird in C geschrieben und erstellt und unter Ubuntu Linux ausgeführt.

Für diese Schritte benötigen Sie ein Gerät mit der Ubuntu-Version 15.04 oder 15.10. Ehe Sie fortfahren, installieren Sie die erforderlichen Pakete mit dem folgenden Befehl auf Ihrem Ubuntu-Gerät:

```
sudo apt-get install cmake gcc g++
```

## <a name="install-the-client-libraries-on-your-device"></a>Installieren der Clientbibliotheken auf dem Gerät
Die Clientbibliotheken für Azure IoT Hub stehen als ein Paket bereit, das Sie mit dem Befehl **apt-Get** auf Ihrem Ubuntu-Gerät installieren können. Führen Sie die folgenden Schritte aus, um das Paket zu installieren, das die IoT Hub-Clientbibliotheks- und Headerdateien auf Ihrem Ubuntu-Computer enthält:

1. Fügen Sie in einer Shell das AzureIoT-Repository Ihrem Computer hinzu:
   
    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```
2. Installieren Sie das Paket „azure-iot-sdk-c-dev“:
   
    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## <a name="install-the-parson-json-parser"></a>Installieren des Parson JSON-Parsers
Die IoT Hub-Clientbibliotheken verwenden den Parson JSON-Parser, um Nachrichtennutzlasten zu analysieren. Klonen Sie in einem geeigneten Ordner auf Ihrem Computer das Parson GitHub-Repository mit dem folgenden Befehl:

```
git clone https://github.com/kgabis/parson.git
```

## <a name="prepare-your-project"></a>Vorbereiten Ihres Projekts
Erstellen Sie auf Ihrem Ubuntu-Computer den Ordner **remote\_monitoring**. Im Ordner **remote\_monitoring**:

- Erstellen Sie die vier Dateien **main.c**, **remote\_monitoring.c**, **remote\_monitoring.h** und **CMakeLists.txt**.
- Erstellen Sie einen Ordner mit dem Namen **parson**.

Kopieren Sie die Dateien **parson.c** und **parson.h** aus der lokalen Kopie des Parson-Repositorys in den Ordner **remote\_monitoring/parson**.

Öffnen Sie in einem Text-Editor die Datei **remote\_monitoring.c**. Fügen Sie die folgenden `#include` -Anweisungen ein:
   
```
#include "iothubtransportmqtt.h"
#include "schemalib.h"
#include "iothub_client.h"
#include "serializer_devicetwin.h"
#include "schemaserializer.h"
#include "azure_c_shared_utility/threadapi.h"
#include "azure_c_shared_utility/platform.h"
#include "parson.h"
```

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="call-the-remotemonitoringrun-function"></a>Aufrufen der Funktion remote\_monitoring\_run
Öffnen Sie in einem Text-Editor die Datei **remote_monitoring.h**. Fügen Sie den folgenden Code hinzu:

```
void remote_monitoring_run(void);
```

Öffnen Sie in einem Text-Editor die Datei **main.c** . Fügen Sie den folgenden Code hinzu:

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## <a name="build-and-run-the-application"></a>Erstellen und Ausführen der Anwendung
Die folgenden Schritte beschreiben, wie Sie *CMake* verwenden, um Ihre Clientanwendung zu erstellen.

1. Öffnen Sie in einem Text-Editor die Datei **CMakeLists.tx**t im Ordner **remote_monitoring**.

1. Fügen Sie die folgenden Anweisungen hinzu, um zu definieren, wie Sie Ihre Clientanwendung erstellen:
   
    ```
    macro(compileAsC99)
      if (CMAKE_VERSION VERSION_LESS "3.1")
        if (CMAKE_C_COMPILER_ID STREQUAL "GNU")
          set (CMAKE_C_FLAGS "--std=c99 ${CMAKE_C_FLAGS}")
          set (CMAKE_CXX_FLAGS "--std=c++11 ${CMAKE_CXX_FLAGS}")
        endif()
      else()
        set (CMAKE_C_STANDARD 99)
        set (CMAKE_CXX_STANDARD 11)
      endif()
    endmacro(compileAsC99)

    cmake_minimum_required(VERSION 2.8.11)
    compileAsC99()

    set(AZUREIOT_INC_FOLDER "${CMAKE_SOURCE_DIR}" "${CMAKE_SOURCE_DIR}/parson" "/usr/include/azureiot" "/usr/include/azureiot/inc")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./parson/parson.c
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./parson/parson.h
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
        serializer
        iothub_client
        iothub_client_mqtt_transport
        aziotsharedutil
        umqtt
        pthread
        curl
        ssl
        crypto
        m
    )
    ```
1. Erstellen Sie im Ordner **remote_monitoring** einen Ordner zum Speichern der *make*-Dateien, die CMake generiert, und führen Sie dann die Befehle **cmake** und **make** wie folgt aus:
   
    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. Führen Sie die Clientanwendung aus, und senden Sie die Telemetriedaten an IoT Hub:
   
    ```
    ./sample_app
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


