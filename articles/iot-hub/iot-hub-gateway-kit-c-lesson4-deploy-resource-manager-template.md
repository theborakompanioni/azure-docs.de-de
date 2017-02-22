---
title: "SensorTag-Gerät und Azure IoT Gateway – Lektion 4: Erstellen der Funktionen-App | Microsoft-Dokumentation"
description: Speichern Sie Nachrichten von Intel NUC in Ihrer IoT Hub-Instanz, schreiben Sie sie in Azure Table Storage, und lesen Sie diese aus der Cloud.
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Speichern von Daten in der Cloud, in der Cloud gespeicherte Daten, IoT-Clouddienst
ms.assetid: f84f9a85-e2c4-4a92-8969-f65eb34c194e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 61e9a9fc7876094c04238c61cfc38efdd97b05f7
ms.openlocfilehash: 3672804218dbbe3a545536b8c4baccb2b9171cdc


---
# <a name="create-an-azure-function-app-and-storage-account"></a>Erstellen einer Azure-Funktionen-App und eines Speicherkontos

Azure Functions ist eine Lösung, mit der Sie _Funktionen_ (kleinere Codeelemente) problemlos in der Cloud ausführen können. Eine Azure-Funktionen-App hostet die Ausführung Ihrer Funktionen in Azure. 

## <a name="what-you-will-do"></a>Aufgaben

- Verwenden Sie eine Azure Resource Manager-Vorlage zum Erstellen einer Azure-Funktionen-App und eines Azure-Speicherkontos. Die Azure-Funktionen-App lauscht auf Azure IoT Hub-Ereignisse, verarbeitet eingehende Nachrichten und schreibt sie in Azure Table Storage.

Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-gateway-kit-c-troubleshooting.md).


## <a name="what-you-will-learn"></a>Sie lernen Folgendes

In dieser Lektion lernen Sie Folgendes:

- Verwenden von Azure Resource Manager zum Bereitstellen von Azure-Ressourcen
- Verwenden einer Azure-Funktionen-App für das Verarbeiten von IoT Hub-Nachrichten und zum Schreiben der Nachrichten in eine Tabelle in Azure Table Storage

## <a name="what-you-need"></a>Erforderliches Element

Sie müssen bereits die vorhergehenden Lektionen erfolgreich ausgeführt haben:

- [Lektion 1: Einrichten von Intel NUC als IoT Gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
- [Lektion 2: Vorbereiten des Hostcomputers und von Azure IoT Hub](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
- [Lektion 3: Empfangen von Nachrichten von SensorTag und Lesen von Nachrichten von IoT Hub](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)

## <a name="open-a-sample-app"></a>Öffnen einer Beispiel-App

Wechseln Sie zu Ihrem Repositoryordner `iot-hub-c-intel-nuc-gateway-getting-started`, initialisieren Sie die Konfigurationsdateien, und öffnen Sie dann das Beispielprojekt in Visual Studio Code, indem Sie den folgenden Befehl ausführen:

```bash
cd Lesson4
npm install
gulp init
code .
```

![repo structure](media/iot-hub-gateway-kit-lessons/lesson4/arm_template.png)

- Die Datei `arm-template.json` ist die Azure Resource Manager-Vorlage mit der Azure-Funktionen-App und einem Azure-Speicherkonto.
- Die Datei `arm-template-param.json` ist die Konfigurationsdatei, die von der Azure Resource Manager-Vorlage verwendet wird.
- Der Unterordner `ReceiveDeviceMessages` enthält den Node.js-Code für die Azure-Funktion.

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>Konfigurieren von Azure Resource Manager-Vorlagen und Erstellen von Ressourcen in Azure

Aktualisieren Sie die Datei `arm-template-param.json` in Visual Studio Code.

![arm template json](media/iot-hub-gateway-kit-lessons/lesson4/arm_template_param.png)

- Ersetzen Sie `[your IoT Hub name]` durch `{my hub name}`, den Sie in Lektion 2 angegeben haben.

Nach der Aktualisierung der Datei `arm-template-param.json` stellen Sie die Ressourcen in Azure bereit, indem Sie den folgenden Befehl ausführen:

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-gateway
```

Verwenden Sie `iot-gateway` als Wert von `{resource group name}`, sofern Sie ihn nicht in Lektion 2 geändert haben.

## <a name="summary"></a>Zusammenfassung

Sie haben die Azure-Funktionen-App zum Verarbeiten von IoT Hub-Nachrichten sowie ein Azure-Speicherkonto zum Speichern dieser Nachrichten erstellt. Sie können jetzt Nachrichten lesen, die durch das Gateway auf Ihre IoT Hub-Instanz gesendet werden.

## <a name="next-steps"></a>Nächste Schritte
[Lesen von persistent gespeicherten Nachrichten in Azure Storage](iot-hub-gateway-kit-c-lesson4-read-table-storage.md)



<!--HONumber=Jan17_HO4-->


