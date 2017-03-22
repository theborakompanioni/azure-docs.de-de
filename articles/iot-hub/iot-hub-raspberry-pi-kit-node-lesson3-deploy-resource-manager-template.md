---
title: "Verbinden von Raspberry Pi (Node) mit Azure IoT – Lektion 3: Bereitstellung von Vorlagen | Microsoft-Dokumentation"
description: Die Azure-Funktionen-App lauscht auf Azure IoT Hub-Ereignisse, verarbeitet eingehende Nachrichten und schreibt sie in Azure Table Storage.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Speichern von Daten in der Cloud, in der Cloud gespeicherte Daten, IoT-Clouddienst
ms.assetid: 6c58de85-c5c4-4989-bb5e-08c45c549966
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: a3a7ec4c81556e4cb530f32c9997d8701db68b2c
ms.lasthandoff: 01/24/2017


---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a>Erstellen einer Azure-Funktionen-App und eines Azure-Speicherkontos
[Azure Functions](../azure-functions/functions-overview.md) ist eine Lösung, mit der Sie ganz einfach *Funktionen* (kleinere Codeelemente) in der Cloud ausführen können. Eine Azure-Funktionen-App hostet die Ausführung Ihrer Funktionen in Azure.

## <a name="what-you-will-do"></a>Aufgaben
Verwenden Sie eine Azure Resource Manager-Vorlage zum Erstellen einer Azure-Funktionen-App und eines Azure-Speicherkontos. Die Azure-Funktionen-App lauscht auf Azure IoT Hub-Ereignisse, verarbeitet eingehende Nachrichten und schreibt sie in Azure Table Storage. Bei Problemen können Sie sich die Lösungen auf der [Seite zur Problembehandlung](iot-hub-raspberry-pi-kit-node-troubleshooting.md) ansehen.

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
In diesem Artikel lernen Sie Folgendes:

* Verwenden von [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) zum Bereitstellen von Azure-Ressourcen.
* Verwenden einer Azure-Funktionen-App für das Verarbeiten von IoT Hub-Nachrichten und zum Schreiben der Nachrichten in eine Tabelle in Azure Table Storage.

## <a name="what-you-need"></a>Erforderliches Element
Folgende Tutorials müssen erfolgreich abgeschlossen worden sein:
* [Get started with Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-get-started.md) (Erste Schritte mit Raspberry Pi 3)
* [Create your Azure IoT hub](iot-hub-raspberry-pi-kit-node-get-started.md) (Erstellen Ihres Azure IoT-Hubs)

## <a name="open-the-sample-app"></a>Öffnen der Beispiel-App
Öffnen Sie das Beispielprojekt in Visual Studio Code, indem Sie die folgenden Befehle ausführen:

```bash
cd Lesson3
code .
```

![Repo structure](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure.png)

* Die Datei `app.js` im Unterordner `app` ist die Hauptquelldatei. Diese Quelldatei enthält den Code, mit dem eine Nachricht 20-mal an die IoT Hub-Instanz gesendet wird. Dabei blinkt die LED bei jeder gesendeten Nachricht.
* Die Datei `arm-template.json` ist die Azure Resource Manager-Vorlage mit der Azure-Funktionen-App und einem Azure-Speicherkonto.
* Die Datei `arm-template-param.json` ist die Konfigurationsdatei, die von der Azure Resource Manager-Vorlage verwendet wird.
* Der Unterordner `ReceiveDeviceMessages` enthält den Node.js-Code für die Azure-Funktion.

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>Konfigurieren von Azure Resource Manager-Vorlagen und Erstellen von Ressourcen in Azure
Aktualisieren Sie die Datei `arm-template-param.json` in Visual Studio Code.

![Azure Resource Manager-Vorlagenparameter](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para.png)

* Ersetzen Sie **[your IoT Hub name]** (Ihr IoT Hub-Name) durch **{mein Hub-Name}**, den Sie beim [Erstellen des IoT-Hubs und Registrieren von Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md) angegeben haben.
* Ersetzen Sie **[prefix string for new resources]** durch ein beliebiges Präfix. Durch das Präfix wird sichergestellt, dass der Ressourcenname global eindeutig ist, um Konflikte zu vermeiden. Verwenden Sie beim Präfix keine Gedankenstriche oder Zahlen als erstes Zeichen.

Nach der Aktualisierung der Datei `arm-template-param.json` stellen Sie die Ressourcen in Azure bereit, indem Sie den folgenden Befehl ausführen:

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

Es dauert etwa fünf Minuten, diese Ressourcen zu erstellen. Sie können während der Ressourcenerstellung mit dem nächsten Artikel fortfahren.

## <a name="summary"></a>Zusammenfassung
Sie haben die Azure-Funktionen-App zum Verarbeiten von IoT Hub-Nachrichten sowie ein Azure-Speicherkonto zum Speichern dieser Nachrichten erstellt. Sie können nun das Beispiel bereitstellen und ausführen, um D2C-Nachrichten (Device-to-Cloud, Gerät-zu-Cloud) an Pi zu senden.

## <a name="next-steps"></a>Nächste Schritte
[Ausführen einer Beispielanwendung zum Senden von D2C-Nachrichten an Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)


