<properties
	pageTitle="Erste Schritte mit dem IoT Hub Gateway SDK | Microsoft Azure"
	description="Exemplarische Vorgehensweise für das Azure IoT Hub Gateway SDK unter Verwendung von Linux zum Veranschaulichen der grundlegenden Konzepte, die Sie beim Verwenden des Azure IoT Hub Gateway SDK kennen sollten."
	services="iot-hub"
	documentationCenter=""
	authors="chipalost"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# IoT-Gateway SDK (Beta) – erste Schritte mit Linux

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## Erstellen des Beispiels

Bevor Sie beginnen, müssen Sie die [Entwicklungsumgebung einrichten][lnk-setupdevbox], um mit dem SDK unter Linux arbeiten zu können.

1. Öffnen Sie eine Shell.
2. Navigieren Sie in Ihrer lokalen Kopie des Repositorys **azure-iot-gateway-sdk** zum Stammordner.
3. Führen Sie das Skript **tools/build.sh** aus. Dieses Skript verwendet das Hilfsprogramm **cmake**, um einen Ordner namens **build** im Stammordner Ihrer lokalen Kopie des **azure-iot-gateway-sdk**-Repositorys zu erstellen und ein Makefile zu generieren. Das Skript erstellt dann die Lösung und führt die Tests aus.

> [AZURE.NOTE]  Bei jeder Ausführung löscht das Skript **build.sh** den Ordner **build** im Stammordner Ihrer lokalen Kopie des **azure-iot-gateway-sdk**-Repositorys und erstellt ihn neu.

## Ausführen des Beispiels

1. Das **build.sh**-Skript generiert die Ausgabe im **build**-Ordner Ihrer lokalen Kopie des **azure-iot-gateway-sdk**-Repositorys. Hierzu gehören die beiden in diesem Beispiel verwendeten Module.

    Das Buildskript platziert **liblogger\_hl.so** im Ordner **build/modules/logger/** und **libhello\_world\_hl.so** im Ordner **build/modules/hello\_world/**. Verwenden Sie diese Pfade für den Wert **module path**, wie in der JSON-Einstellungsdatei unten gezeigt.

2. Die Datei **hello\_world\_lin.json** im Ordner **samples/hello\_world/src** ist eine JSON-Beispieleinstellungsdatei für Linux, die Sie zur Ausführung des Beispiels verwenden können. Bei den unten gezeigten beispielhaften JSON-Einstellungen wird angenommen, dass Sie das Beispiel aus dem Stammordner Ihrer lokalen Kopie des **azure-iot-gateway-sdk**-Repositorys ausführen.

3. Legen Sie im Modul **logger\_hl** im Abschnitt **args** den Wert **filename** auf den Namen und Pfad der Datei fest, in der die Protokolldaten gespeichert werden.

    Dies ist ein Beispiel für eine JSON-Einstellungsdatei für Linux, die in die Datei **log.txt** in dem Ordner schreibt, in dem Sie das Beispiel ausführen.

    ```
    {
      "modules" :
      [ 
        {
          "module name" : "logger_hl",
          "module path" : "./build/modules/logger/liblogger_hl.so",
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "./build/modules/hello_world/libhello_world_hl.so",
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

3. Navigieren Sie in der Shell zum Ordner **azure-iot-gateway-sdk**.
4. Führen Sie den folgenden Befehl aus:
  
  ```
  ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
  ``` 

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md

<!---HONumber=AcomDC_0928_2016-->