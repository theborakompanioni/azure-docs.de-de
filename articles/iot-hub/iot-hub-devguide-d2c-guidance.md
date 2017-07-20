---
title: "Azure IoT Hub – D2C-Optionen (Gerät-zu-Cloud) | Microsoft Docs"
description: "Entwicklerhandbuch – Leitfaden, der angibt, wann D2C-Nachrichten, gemeldete Eigenschaften oder Dateiupload für die C2D-Kommunikation verwendet werden sollen."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 979136db-c92d-4288-870c-f305e8777bdd
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: elioda
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: a36283053939ccd53856a394cd9efb66285271ae
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="device-to-cloud-communications-guidance"></a>Leitfaden zur D2C-Kommunikation
Beim Senden von Informationen von der Geräte-App an das Lösungs-Back-End stehen in IoT Hub drei Optionen zur Verfügung:

* [Gerät-zu-Cloud-Nachrichten][lnk-d2c] für Time Series-Telemetrie und Warnungen
* [Gemeldete Eigenschaften][lnk-twins] für die Meldung von Gerätestatusinformationen, z.B. verfügbare Funktionen, Bedingungen und Status von Workflows mit langer Laufzeit, z.B. Konfigurations- und Softwareupdates.
* [Dateiuploads][lnk-fileupload] für Mediendateien und große Telemetriebatches, die von zeitweise verbundenen Geräten hochgeladen oder komprimiert werden, um Bandbreite zu sparen.

Hier finden Sie einen detaillierten Vergleich verschiedener Optionen für die D2C-Kommunikation.

|  | D2C-Nachrichten | Gemeldete Eigenschaften | Dateiuploads |
| ---- | ------- | ---------- | ---- |
| Szenario | Telemetrie-Zeitreihen und -Warnungen, Beispiel: Sendung von 256-KB-Sensordatenbatches alle 5 Minuten | Verfügbare Funktionen und Bedingungen, z.B. der aktuelle Gerätekonnektivitätsmodus wie Mobilfunk oder WLAN. Synchronisierung von Workflows mit langer Laufzeit, z.B. Konfiguration und Softwareupdates. | Mediendateien. Große (normalerweise komprimierte) Telemetriebatches. |
| Speichern und Abrufen | Temporäre Speicherung durch IoT Hub, bis zu 7 Tage. Nur sequenzielles Lesen. | Von IoT Hub im Gerätezwilling gespeichert. Abrufbar mithilfe der [IoT Hub-Abfragesprache][lnk-query]. | Speicherung im vom Benutzer bereitgestellten Azure Storage-Konto. |
| Größe | Nachrichten bis zu 256 KB | Die Maximalgröße gemeldeter Eigenschaften beträgt 8 KB. | Maximale von Azure Blob Storage unterstützte Dateigröße. |
| Frequency | Hoch. Weitere Informationen finden Sie unter [Referenz: IoT Hub-Kontingente und -Drosselung][lnk-quotas]. | Mittel. Weitere Informationen finden Sie unter [Referenz: IoT Hub-Kontingente und -Drosselung][lnk-quotas]. | Niedrig. Weitere Informationen finden Sie unter [Referenz: IoT Hub-Kontingente und -Drosselung][lnk-quotas]. |
| Protocol | Mit allen Protokollen verfügbar. | Derzeit nur mit MQTT verfügbar. | Mit jedem Protokoll verfügbar, doch auf dem Gerät ist HTTP erforderlich. |

Möglicherweise erfordert eine Anwendung, dass Informationen sowohl als Telemetrie-Time Series als auch Warnung gesendet werden und außerdem im Gerätezwilling verfügbar sind. In diesem Szenario können Sie eine der folgenden Optionen wählen:

* Die Geräte-App sendet eine D2C-Nachricht und meldet eine Eigenschaftsänderung.
* Das Lösungs-Back-End speichert die Informationen beim Empfang der Nachricht in den Tags des Gerätezwillings.

Da D2C-Nachrichten einen viel höheren Durchsatz zulassen als Gerätezwillingsupdates, ist es in einigen Fällen ratsam, das Gerätezwillingsupdate nicht für jede D2C-Nachricht durchzuführen.


[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-fileupload]: iot-hub-devguide-file-upload.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md

