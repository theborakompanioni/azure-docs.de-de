---
title: Erstellen Ihres ersten Azure IoT-Gateway-Moduls | Microsoft-Dokumentation
description: "Erstellen Sie ein Modul und fügen Sie es einer Beispiel-App hinzu, um das Modulverhalten anzupassen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: 
ms.assetid: cd7660f4-7b8b-4091-8d71-bb8723165b0b
ms.service: iot-hub
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/17/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 6b12d39ce8c6062df5fb78de654bf22183b6ecd9
ms.openlocfilehash: 42c22192bc3cccdea97f7d8dac575fa7cbc9a6c6
ms.lasthandoff: 02/21/2017


---
# <a name="lesson-5-create-your-first-azure-iot-gateway-module"></a>Lektion 5: Erstellen Ihres ersten Azure IoT-Gateway-Moduls

## <a name="what-you-will-do"></a>Aufgaben

- Kompilieren Sie, und führen Sie die Beispiel-App „hello_world“ auf Intel NUC aus.
- Erstellen Sie ein Modul, und kompilieren Sie es auf Intel NUC.
- Fügen Sie das neue Modul der Beispiel-App „hello_world“ hinzu, und führen Sie das Beispiel auf Intel NUC aus. Das neue Modul gibt „hello_world“-Meldungen mit einem Zeitstempel aus.

## <a name="what-you-will-learn"></a>Sie lernen Folgendes

- Kompilieren und Ausführen einer Beispiel-App auf Intel NUC.
- Erstellen eines Moduls.
- Hinzufügen eines Moduls zu einer Beispiel-App.

## <a name="what-you-need"></a>Erforderliches Element

Das Azure IoT-Gateway-SDK, das auf dem Hostcomputer installiert wurde.

## <a name="folder-structure"></a>Ordnerstruktur

In Lektion 5 befinden sich im Unterordner des Beispielcodes, den Sie in Lektion 1 geklont haben, ein Ordner `module` und ein Ordner `sample`.

![my_module](media/iot-hub-gateway-kit-lessons/lesson5/my_module.png)

- Ordner `module/my_module` enthält den Quellcode und das Skript zum Erstellen des Moduls.
- Ordner `sample` enthält den Quellcode und das Skript zum Erstellen der Beispiel-App.

## <a name="compile-and-run-the-helloworld-sample-app-on-intel-nuc"></a>Kompilieren und Ausführen der Beispiel-App „hello_world“ auf Intel NUC

Das Beispiel `hello_world` erstellt ein Gateway auf der Grundlage der Datei `hello_world.json`, die die zwei vordefinierten Module angibt, die der App zugeordnet sind. Das Gateway protokolliert alle 5 Sekunden eine „hello world“-Meldung in einer Datei. In diesem Abschnitt kompilieren Sie die `hello_world`-App mit ihrem Standardmodul und führen sie aus.

Führen Sie zum Kompilieren und Ausführen der `hello_world`-App diese Schritte auf Ihrem Hostcomputer aus:

1. Initialisieren Sie die Konfigurationsdateien mit folgenden Befehlen:

   ```bash
   cd iot-hub-c-intel-nuc-gateway-getting-started
   cd Lesson5
   npm install
   gulp init
   ```

1. Aktualisieren Sie die Gatewaykonfigurationsdatei mit der MAC-Adresse von Intel NUC. Überspringen Sie diesen Schritt, wenn Sie die Lektion zum [Konfigurieren und Ausführen einer BLE-Beispielanwendung][config_ble] durchgearbeitet haben.

   1. Öffnen Sie die Gatewaykonfigurationsdatei mit folgendem Befehl:

      ```bash
      # For Windows command prompt
      code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json

      # For MacOS or Ubuntu
      code ~/.iot-hub-getting-started/config-gateway.json
      ```

   1. Aktualisieren Sie die MAC-Adresse des Gateways, wenn Sie [Intel NUC als IoT Gateway einrichten][setup_nuc], und speichern Sie die Datei.

1. Kompilieren Sie den Beispielquellcode mit folgendem Befehl:

   ```bash
   gulp compile
   ```

   Der Befehl überträgt den Beispielquellcode an Intel NUC und führt `build.sh` aus, um ihn zu kompilieren.

1. Führen Sie die `hello_world`-App auf Intel NUC mit folgendem Befehl aus:

   ```bash
   gulp run
   ```

   Der Befehl führt `../Tools/run-hello-world.js` aus, was in `config.json` angegeben ist, um die Beispiel-App auf Intel NUC zu starten.

   ![run_sample](media/iot-hub-gateway-kit-lessons/lesson5/run_sample.png)

## <a name="create-a-new-module-and-compile-it-on-intel-nuc"></a>Erstellen eines neuen Moduls und dessen Kompilierung auf Intel NUC

Die folgenden Schritte führen Sie durch das Erstellen eines neuen Moduls und dessen Kompilierung auf Intel NUC. Das Modul gibt Meldungen mit einem Zeitstempel aus, sobald es sie empfängt. In diesem Abschnitt erstellen Sie Ihr erstes benutzerdefiniertes Gatewaymodul.

Jedes Azure IoT-Gateway-SDK-Modul muss die folgenden Schnittstellen implementieren:

   ```C
   pfModule_ParseConfigurationFromJson Module_ParseConfigurationFromJson
   pfModule_FreeConfiguration Module_FreeConfiguration
   pfModule_Create Module_Create
   pfModule_Destroy Module_Destroy
   pfModule_Receive Module_Receive
   ```

Optional können Sie die folgende Schnittstelle implementieren:

   ```C
   pfModule_Start Module_Start
   ```

Das folgende Diagramm zeigt die wichtigen Zustandspfade eines Moduls. Die quadratischen Rechtecke stellen Methoden dar, die Sie implementieren, um Vorgänge auszuführen, wenn das Modul sich zwischen den Zuständen bewegt. Die Ovale sind Hauptzustände, in denen sich das Modul befinden kann.

![state_path](media/iot-hub-gateway-kit-lessons/lesson5/state_path.png)

Jetzt erstellen wir ein auf der Vorlage basierendes Modul:

1. Öffnen Sie den Vorlagenordner mit folgendem Befehl:

   ```bash
   code module/my_module
   ```

   ![code_module](media/iot-hub-gateway-kit-lessons/lesson5/code_module.png)

   - `src/my_module.c` dient als Vorlage, die die Erstellung eines Moduls erleichtert. Die Vorlage deklariert die Schnittstellen. Sie müssen lediglich der Funktion `MyModule_Receive` Code hinzufügen.
   - `build.sh` ist das Buildskript zum Kompilieren des Moduls auf Intel NUC.
1. Öffnen Sie die Datei `src/my_module.c`, und beziehen Sie zwei Headerdateien ein:

   ```C
   #include <stdio.h>
   #include "azure_c_shared_utility/xlogging.h"
   ```

1. Fügen Sie der Funktion `MyModule_Receive` den folgenden Code hinzu:

   ```C
   if (message == NULL)
   {
      LogError("invalid arg message");
   }
   else
   {
      // get the message content
      const CONSTBUFFER * content = Message_GetContent(message);
      // get the local time and format it
      time_t temp = time(NULL);
      if (temp == (time_t)-1)
      {
          LogError("time function failed");
      }
      else
      {
          struct tm* t = localtime(&temp);
          if (t == NULL)
          {
              LogError("localtime failed");
          }
          else
          {
              char timetemp[80] = { 0 };
              if (strftime(timetemp, sizeof(timetemp) / sizeof(timetemp[0]), "%c", t) == 0)
              {
                  LogError("unable to strftime");
              }
              else
              {
                  printf("[%s] %.*s\r\n", timetemp, (int)content->size, content->buffer);
              }
          }
      }
   }
   ```

1. Aktualisieren Sie die Datei `config.json`, sodass der Ordner `workspace` auf Ihrem Hostcomputer und der Bereitstellungspfad auf Intel NUC angegeben werden. Während der Kompilierung werden die Dateien in Ordner `workspace` in den Bereitstellungspfad übertragen.

   1. Öffnen Sie die Datei `config.json` mit folgendem Befehl:

      ```bash
      code config.json
      ```

   1. Aktualisieren Sie `config.json` mit folgender Konfiguration:

      ```json
      "workspace": "./module/my_module",
      "deploy_path": "module/my_module"
      ```

      ![config_json](media/iot-hub-gateway-kit-lessons/lesson5/config_json.png)

1. Kompilieren Sie das Modul mit folgendem Befehl:

   ```bash
   gulp compile
   ```

   Der Befehl überträgt den Quellcode an Intel NUC und führt `build.sh` aus, um das Modul zu kompilieren.

## <a name="add-the-module-to-the-helloworld-sample-app-and-run-the-app-on-intel-nuc"></a>Fügen Sie das Modul der Beispiel-App „hello_world“ hinzu, und führen Sie die App auf Intel NUC aus.

Führen Sie diese Aufgabe in folgenden Schritten aus:

1. Listen Sie alle verfügbaren Modulbinärdateien (SO-Dateien) auf Intel NUC mit folgendem Befehl auf:

   ```bash
   gulp modules --list
   ```

   Der Binärpfad des von Ihnen kompilierten `my_module` sollte folgendermaßen aufgelistet werden:

   ```path
   /root/gateway_sample/module/my_module/build/libmy_module.so
   ```

   Wenn Sie den Standardbenutzernamen für die Anmeldung in `config-gateway.json` ändern, beginnt der binäre Pfad mit `home/<your username>` anstelle von `root`.

1. Fügen Sie `my_module` der Beispiel-App `hello_world` hinzu:

   1. Öffnen Sie die Datei `hello_world.json` mit folgendem Befehl:

      ```bash
      code sample/hello_world/src/hello_world.json
      ```

   1. Fügen Sie den folgenden Code dem Abschnitt `modules` hinzu:

      ```json
      {
       "name": "my_module",
       "loader": {
       "name": "native",
       "entrypoint": {
       "module.path": "/root/gateway_sample/module/my_module/build/libmy_module.so"
         }
        },
       "args": null
      }
      ```

      Der Wert von `module.path` sollte `/root/gateway_sample/module/my_module/build/libmy_module.so` sein. Der Code deklariert sowohl, dass `my_module` dem Gateway zugeordnet wird, als auch den Speicherort der in `module.path` angegebenen Modulbinärdatei.
   1. Fügen Sie den folgenden Code dem Abschnitt `links` hinzu:

      ```json
      {
        "source": "hello_world",
        "sink": "my_module"
      }
      ```

      Dieser Code gibt an, dass Meldungen vom Modul `hello_world` an `my_module` übertragen werden.

      ![hello_world_json](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_json.png)

1. Führen Sie die Beispiel-App `hello_world` mit folgendem Befehl aus:

   ```bash
   gulp run --config sample/hello_world/src/hello_world.json
   ```

   Der Parameter `--config` erzwingt, dass das Skript `run-hello-world.js` mit der Datei `hello_world.json` ausgeführt wird.

   ![hello_world_new](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_new.png)

Herzlichen Glückwunsch. Sie können nun das Verhalten dieses neuen Moduls beobachten: Es gibt einfach „hello world“-Meldungen mit einem Zeitstempel aus, ein anderes Ergebnis als beim ursprünglichen „hello_world“-Modul.

## <a name="next-steps"></a>Nächste Schritte

Sie haben ein neues Modul erstellt, es dem „hello_world“-Beispiel hinzugefügt und die Beispiel-App mit dem neuen Modul auf Ihrem Gateway ausgeführt. Wenn Sie weitere Informationen zu Azure IoT-Gateway-Modulen wünschen, finden Sie hier weitere Modulbeispiele: [https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules).

<!-- Images and links -->

[config_ble]: iot-hub-gateway-kit-c-lesson3-configure-ble-app.md
[setup_nuc]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md

