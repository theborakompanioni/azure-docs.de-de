---
title: "Verwalten von Azure IoT Hub-Nachrichten zwischen Cloud und Geräten mit iothub-explorer | Microsoft-Dokumentation"
description: "Erfahren Sie, wie in Azure IoT Hub mit dem CLI-Tool „iothub-explorer“ Gerät-zu-Cloud-Nachrichten (Device to Cloud, D2C) überwachen und Cloud-zu-Gerät-Nachrichten (Cloud to Device, C2D) senden."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "iothub-explorer, Cloud-zu-Gerät-Messaging, IoT Hub Cloud-zu-Gerät, Cloud-zu-Gerät-Nachrichten"
ms.assetid: 04521658-35d3-4503-ae48-51d6ad3c62cc
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: xshi
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: a7abb8fb279e134b7f23df779f1c4548a9feb82d
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---
# <a name="use-iothub-explorer-to-send-and-receive-messages-between-your-device-and-iot-hub"></a>Verwenden von iothub-explorer zum Senden und Empfangen von Nachrichten zwischen Ihrem Gerät und IoT Hub

![Lückenloses Diagramm](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[iothub-explorer](https://github.com/azure/iothub-explorer) bietet eine übersichtliche Anzahl von Befehlen zum Vereinfachen der IoT Hub-Verwaltung. Der Schwerpunkt dieses Tutorials liegt auf dem Verwenden von iothub-explorer zum Senden und Empfangen von Nachrichten zwischen Ihrem Gerät und IoT Hub.

## <a name="what-you-will-learn"></a>Lernziele

Sie erfahren, wie Sie mit iothub-explorer Gerät-zu-Cloud-Nachrichten überwachen und Cloud-zu-Gerät-Nachrichten senden. Gerät-zu-Cloud-Nachrichten können Sensordaten sein, die Ihr Gerät erfasst und dann an Ihren IoT Hub sendet. Cloud-zu-Gerät-Nachrichten können Befehle sein, die Ihr IoT Hub an Ihr Gerät sendet, um eine LED blinken zu lassen, die mit Ihrem Gerät verbunden ist.

## <a name="what-you-will-do"></a>Aufgaben

- Überwachen von Gerät-zu-Cloud-Nachrichten mit iothub-explorer
- Senden von Cloud-zu-Gerät-Nachrichten mit iothub-explorer

## <a name="what-you-need"></a>Erforderliches Element

- Abschluss des Tutorials [Einrichten Ihres Geräts](iot-hub-raspberry-pi-kit-node-get-started.md), in dem die folgenden Anforderungen behandelt werden:
  - Ein aktives Azure-Abonnement.
  - Ein Azure IoT Hub in Ihrem Abonnement.
  - Eine Clientanwendung, die Nachrichten an Ihren Azure IoT Hub sendet.
- iothub-explorer. ([Installieren von iothub-explorer](https://github.com/azure/iothub-explorer))

## <a name="monitor-device-to-cloud-messages"></a>Überwachen von Gerät-zu-Cloud-Nachrichten

Um Nachrichten zu überwachen, die von Ihrem Gerät an Ihren IoT Hub gesendet werden, gehen Sie folgendermaßen vor:

1. Öffnen Sie ein Konsolenfenster.
1. Führen Sie den folgenden Befehl aus:

   ```bash
   iothub-explorer monitor-events <device-id> --login "<IoTHubConnectionString>"
   ```

   > [!Note]
   > Rufen Sie `<device-id>` und `<IoTHubConnectionString>` von Ihrem IoT Hub ab. Stellen Sie sicher, dass Sie das vorherige Tutorial abgeschlossen haben. Sie können auch versuchen, `iothub-explorer monitor-events <device-id> --login "HostName=<my-hub>.azure-devices.net;SharedAccessKeyName=<my-policy>;SharedAccessKey=<my-policy-key>"` zu verwenden, wenn Sie `HostName`, `SharedAccessKeyName` und `SharedAccessKey` haben.

## <a name="send-cloud-to-device-messages"></a>Senden von C2D-Nachrichten.

Um eine Nachricht von Ihrem IoT Hub zu Ihrem Gerät zu senden, gehen Sie folgendermaßen vor:

1. Öffnen Sie ein Konsolenfenster.
1. Starten Sie eine Sitzung auf Ihren IoT Hub, indem Sie den folgenden Befehl ausführen:

   ```bash
   iothub-explorer login `<IoTHubConnectionString>`
   ```

1. Senden Sie eine Nachricht an Ihr Gerät, indem Sie den folgenden Befehl ausführen:

   ```bash
   iothub-explorer send <device-id> <message>
   ```

Der Befehl lässt die LED blinken, die mit dem Gerät verbunden ist, und sendet die Nachricht an Ihr Gerät.

> [!Note]
> Bei Empfang der Nachricht muss das Gerät keine gesonderte Bestätigungsnachricht zurück zu Ihrem IoT Hub senden.

## <a name="next-steps"></a>Nächste Schritte

Sie haben gelernt, Gerät-zu-Cloud-Nachrichten zu überwachen und Cloud-zu-Gerät-Nachrichten zwischen dem IoT-Gerät und Azure IoT Hub zu senden.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

