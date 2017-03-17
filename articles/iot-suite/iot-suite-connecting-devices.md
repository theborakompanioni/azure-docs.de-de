---
title: "Verbinden eines Geräts mit C unter Windows | Microsoft Docs"
description: "Beschreibt, wie Sie ein Gerät mit der vorkonfigurierten Remoteüberwachungslösung von Azure IoT Suite verbinden, indem Sie eine in C geschriebene Anwendung unter Windows ausführen."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 34e39a58-2434-482c-b3fa-29438a0c05e8
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/17/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: df9772796796f7383aafc583b01f299a53679d88
ms.openlocfilehash: 77e05bf2b7a4ab6c6e6d3d82773cc03628f5342c
ms.lasthandoff: 02/27/2017


---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a>Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung (Windows)
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a>Erstellen einer C-Beispielprojektmappe unter Windows
Die folgenden Schritte zeigen, wie Sie eine Clientanwendung erstellen, die mit der vorkonfigurierten Lösung für die Remoteüberwachung kommuniziert. Diese Anwendung wird in C geschrieben und erstellt und unter Windows ausgeführt.

Erstellen Sie ein Startprojekt in Visual Studio 2015 und fügen Sie die NuGet-Pakete für den IoT Hub-Geräteclienthinzu.

1. Erstellen Sie eine C-Konsolenanwendung mit Visual C++ in der **Win32-Konsolenanwendungsvorlage** in Visual Studio 2015. Geben Sie dem Projekt den Namen **RMDevice**.
2. Stellen Sie auf der Seite **Anwendungseinstellungen** im **Win32-Anwendungsassistenten** sicher, dass **Konsolenanwendung** ausgewählt ist, und deaktivieren Sie die Kontrollkästchen **Vorkompilierter Header** und Prüfungen für den **Security Development Lifecycle (SDL)**.
3. Löschen Sie im **Projektmappen-Explorer**die Dateien „stdafx.h“, „targetver.h“ und „stdafx.cpp“.
4. Benennen Sie im **Projektmappen-Explorer**die Datei „RMDevice.cpp“ in „RMDevice.c“ um.
5. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **RMDevice**, und klicken Sie dann auf **NuGet-Pakete verwalten**. Klicken Sie auf **Durchsuchen**, suchen Sie dann die folgenden NuGet-Pakete, und installieren Sie diese in das Projekt:
   
   * Microsoft.Azure.IoTHub.Serializer
   * Microsoft.Azure.IoTHub.IoTHubClient
   * Microsoft.Azure.IoTHub.MqttTransport
6. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **RMDevice**, und klicken Sie dann auf **Eigenschaften**, um das Dialogfeld **Eigenschaftenseiten** des Projekts zu öffnen. Weitere Informationen hierzu finden Sie unter [Festlegen von Visual C++-Projekteigenschaften][lnk-c-project-properties]. 
7. Klicken Sie auf den Ordner **Linker** und dann auf die Eigenschaftenseite **Eingabe**.
8. Fügen Sie **crypt32.lib** zur Eigenschaft **Zusätzliche Abhängigkeiten** hinzu. Klicken Sie auf **OK** und dann erneut auf **OK**, um die Eigenschaftenwerte für das Projekt zu speichern.

Fügen Sie die Parson JSON-Bibliothek dem **RMDevice**-Projekt hinzu, und fügen Sie die erforderlichen `#include`-Anweisungen hinzu:

1. Klonen Sie in einem geeigneten Ordner auf Ihrem Computer das Parson GitHub-Repository mit dem folgenden Befehl:

    ```
    git clone https://github.com/kgabis/parson.git
    ```

1. Kopieren Sie die Dateien „parson.h“ und „parson.c“ aus der lokalen Kopie des Parson-Repositorys in Ihren **RMDevice**-Projektordner.

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt **RMDevice**. Klicken Sie auf **Hinzufügen** und dann auf **Vorhandenes Element**.

1. Wählen Sie im Dialogfeld **Vorhandenes Element hinzufügen** die Dateien „parson.h“ und „parson.c“ im **RMDevice**-Projektordner aus. Klicken Sie dann auf **Hinzufügen**, um diese zwei Dateien Ihrem Projekt hinzuzufügen.

1. Öffnen Sie in Visual Studio die Datei „RMDevice.c“. Ersetzen Sie die vorhandenen `#include`-Anweisungen durch folgenden Code:
   
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

    > [!NOTE]
    > Jetzt können Sie Ihr Projekt erstellen und dabei überprüfen, ob die richtigen Abhängigkeiten eingerichtet sind.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

Fügen Sie Code zum Aufrufen der Funktion **remote\_monitoring\_run** hinzu, erstellen Sie anschließend die Geräteanwendung, und führen Sie sie aus.

1. Ersetzen Sie die **main**-Funktion durch folgenden Code, um die **remote\_monitoring\_run**-Funktion aufzurufen:
   
    ```
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Klicken Sie auf **Erstellen** und dann auf **Projektmappe erstellen**, um die Geräteanwendung zu erstellen.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **RMDevice**, klicken Sie auf **Debuggen**, und klicken Sie dann auf **Neue Instanz starten**, um das Beispiel auszuführen. Die Konsole zeigt Meldungen an, während die Anwendung Beispieltelemetriedaten an die vorkonfigurierte Lösung sendet, empfängt die im Lösungsdashboard festgelegten gewünschten Eigenschaftswerte und antwortet auf Methoden, die über das Lösungsdashboard aufgerufen werden.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx

