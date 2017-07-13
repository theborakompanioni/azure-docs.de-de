---
title: "SensorTag-Gerät und Azure IoT Gateway – Lektion 4: Table Storage | Microsoft-Dokumentation"
description: Speichern Sie Nachrichten von Intel NUC in Ihrer IoT Hub-Instanz, schreiben Sie sie in Azure Table Storage, und lesen Sie diese aus der Cloud.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Daten aus der Cloud abrufen, IoT-Clouddienst
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 8ca78045-ad92-4a6a-90f1-05f9668e6f0e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.translationtype: Human Translation
ms.sourcegitcommit: 61e9a9fc7876094c04238c61cfc38efdd97b05f7
ms.openlocfilehash: 293343e5db58bebc8c2e27d54d6396212fe6d3a1
ms.contentlocale: de-de
ms.lasthandoff: 01/25/2017

---

<a id="read-messages-persisted-in-azure-table-storage" class="xliff"></a>

# Lesen von Nachrichten in Azure Table Storage

<a id="what-you-will-do" class="xliff"></a>

## Aufgaben

- Führen Sie die Gatewaybeispielanwendung auf dem Gateway aus, über das Nachrichten an Ihren IoT Hub gesendet werden.
- Führen Sie dann einen Beispielcode auf Ihrem Hostcomputer aus, um die Nachrichten in Ihrem Azure-Tabellenspeicher zu lesen. 

Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-gateway-kit-c-troubleshooting.md).

<a id="what-you-will-learn" class="xliff"></a>

## Sie lernen Folgendes

Verwenden des Gulp-Tools zum Ausführen des Beispielcodes, um Nachrichten in Ihrem Azure-Tabellenspeicher Storage zu lesen.

<a id="what-you-need" class="xliff"></a>

## Erforderliches Element

Sie haben die folgenden Aufgaben erfolgreich durchgeführt:

- [Erstellen der Azure-Funktionen-App und des Azure Storage-Kontos](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)
- [Ausführen der Gatewaybeispielanwendung](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)
- [Lesen von Nachrichten von Ihrem IoT Hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)

<a id="get-your-azure-storage-connection-strings" class="xliff"></a>

## Abrufen von Azure-Speicherverbindungszeichenfolgen

Am Anfang dieser Lektion haben Sie erfolgreich ein Azure Storage-Konto erstellt. Führen Sie die folgenden Befehle aus, um die Verbindungszeichenfolge des Azure Storage-Kontos abzurufen:

* Listen Sie Ihre Speicherkonten auf.

```bash
az storage account list -g iot-gateway --query [].name
```

* Rufen Sie die Azure-Speicherverbindungszeichenfolge ab.

```bash
az storage account show-connection-string -g iot-gateway -n {storage name}
```

Verwenden Sie „iot-gateway“ als Wert von `{resource group name}`, sofern Sie diesen nicht in Lektion 2 geändert haben.

<a id="configure-the-device-connection" class="xliff"></a>

## Konfigurieren der Geräteverbindung

Aktualisieren Sie die Datei `config-azure.json` so, dass mit dem auf dem Hostcomputer ausgeführten Beispielcode die Nachricht in Ihrem Azure-Tabellenspeicher gelesen werden kann. Gehen Sie folgendermaßen vor, um die Geräteverbindung zu konfigurieren:

1. Öffnen Sie die Gerätekonfigurationsdatei `config-azure.json`, indem Sie den folgenden Befehl ausführen:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

   ![Konfiguration](media/iot-hub-gateway-kit-lessons/lesson4/config_azure.png)

2. Ersetzen Sie `[Azure storage connection string]` durch die Azure-Speicherverbindungszeichenfolge, die Sie abgerufen haben.

   `[IoT hub connection string]` sollte bereits in Lektion&3; im Abschnitt [Read messages from your IoT hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md) (Lesen von Nachrichten von Ihrem IoT Hub) ersetzt worden sein.

<a id="read-messages-in-your-azure-table-storage" class="xliff"></a>

## Lesen von Nachrichten in Ihrem Azure-Tabellenspeicher

Führen Sie die Gatewaybeispielanwendung aus, und lesen Sie die Azure-Tabellenspeicher-Nachrichten mit dem folgenden Befehl:

```bash
gulp run --table-storage
```

Der IoT Hub löst Ihre Azure-Funktionen-Anwendung aus, um eine Nachricht im Azure-Tabellenspeicher zu speichern, wenn eine neue Nachricht empfangen wird.
Mit dem Befehl `gulp run` wird die Gatewaybeispielanwendung ausgeführt, mit der Nachrichten an Ihren IoT Hub gesendet werden. Mit dem Parameter `table-storage` wird außerdem ein untergeordneter Prozess erzeugt, um die gespeicherte Nachricht in Ihrem Azure-Tabellenspeicher zu empfangen.

Alle gesendeten und empfangenen Nachrichten werden auf dem Hostcomputer sofort in demselben Konsolenfenster angezeigt. Die Instanz der Beispielanwendung endet automatisch nach 40 Sekunden.

   ![Gulp-Lesevorgang](media/iot-hub-gateway-kit-lessons/lesson4/gulp_run_read_table.png)


<a id="summary" class="xliff"></a>

## Zusammenfassung

Sie haben den Beispielcode zum Lesen der Nachrichten in Ihrem Azure-Tabellenspeicher nach dem Speichern mit der Azure-Funktionen-Anwendung ausgeführt.
