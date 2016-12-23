---
title: Leitfaden zur D2C-Kommunikation mit Azure IoT Hub | Microsoft Docs
description: "Azure IoT Hub-Entwicklerhandbuch – Leitfaden, der angibt, wann D2C-Nachrichten, gemeldete Eigenschaften oder Dateiupload verwendet werden sollen."
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
ms.date: 09/30/2016
ms.author: elioda
translationtype: Human Translation
ms.sourcegitcommit: 53f14e6fe115ed5f96d25b9ec5ab04abe23712d5
ms.openlocfilehash: 6d5608cd86e3f042a7d63bd4e43e75a06141dfab


---
# <a name="device-to-cloud-communications-guidance"></a>Leitfaden zur D2C-Kommunikation
Beim Senden von Informationen von der Geräte-App an das Back-End macht IoT Hub drei Optionen verfügbar:

* [Gerät-zu-Cloud-Nachrichten (D2C)][lnk-d2c] für Time Series-Telemetrie und Warnungen;
* [Gemeldete Eigenschaften][lnk-twins] für die Meldung von Gerätestatusinformationen, z.B. verfügbare Funktionen, Bedingungen und Status von Workflows mit langer Laufzeit (z.B. Konfiguration und Softwareupdates);
* [Dateiuploads][lnk-fileupload] für Mediendateien und große Telemetriebatches, die von zeitweise verbundenen Geräten hochgeladen oder komprimiert werden, um Bandbreite zu sparen.

Hier finden Sie einen detaillierten Vergleich verschiedener Optionen für die D2C-Kommunikation.

|  | D2C-Meldungen | Gemeldete Eigenschaften | Dateiuploads |
| ---- | ------- | ---------- | ---- |
| Szenario | Telemetrie-Time Series und Warnungen, z.B. Sendung von 256-KB-Sensordatenbatches alle 5 Minuten. | Verfügbare Funktionen und Bedingungen, z.B. aktueller Verbindungsmodus des Geräts (Mobilfunk oder WiFi). Synchronisierung von Workflows mit langer Laufzeit, z.B. Konfiguration und Softwareupdates. | Mediendateien. Große (normalerweise komprimierte) Telemetriebatches. |
| Speichern und Abrufen | Temporäre Speicherung durch IoT Hub, bis zu 7 Tage. Nur sequenzielles Lesen. | Von IoT Hub im Gerätezwilling gespeichert. Abrufbar mithilfe der [IoT Hub-Abfragesprache][lnk-query]. | Speicherung im vom Benutzer bereitgestellten Azure Storage-Konto. |
| Größe | Nachrichten bis zu 256KB. | Die Maximalgröße gemeldeter Eigenschaften beträgt 8KB. | Maximale von Azure Blob Storage unterstützte Dateigröße. |
| Frequency | Hoch. Weitere Informationen finden Sie unter [IoT Hub-Grenzen][lnk-quotas]. | Mittel. Weitere Informationen finden Sie unter [IoT Hub-Grenzen][lnk-quotas]. | Niedrig. Weitere Informationen finden Sie unter [IoT Hub-Grenzen][lnk-quotas]. |
| Protocol | Mit allen Protokollen verfügbar. | Derzeit nur mit MQTT verfügbar. | Mit jedem Protokoll verfügbar, doch auf dem Gerät ist HTTP erforderlich. |

> [!NOTE]
> Möglicherweise erfordert eine Anwendung, dass Informationen sowohl als Telemetrie-Time Series als auch Warnung gesendet werden und außerdem im Gerätezwilling verfügbar sind. In diesen Fällen kann die Geräte-App sowohl eine D2C-Nachricht senden als auch eine Eigenschaftenänderung melden, oder das Back-End kann die Informationen beim Empfang der Nachricht in den Gerätezwillingstags speichern. Da D2C-Nachrichten einen viel höheren Durchsatz zulassen als Gerätezwillingsupdates, ist es in einigen Fällen ratsam, das Gerätezwillingsupdate nicht für jede D2C-Nachricht durchzuführen.
> 
> 

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-fileupload]: iot-hub-devguide-file-upload.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages



<!--HONumber=Nov16_HO5-->


