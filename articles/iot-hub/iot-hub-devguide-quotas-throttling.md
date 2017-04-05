---
title: Informationen zu Kontingenten und Drosselung bei Azure IoT Hub | Microsoft Docs
description: "Entwicklerhandbuch: Beschreibung der für IoT Hub geltenden Kontingente und des erwarteten Drosselungsverhaltens"
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 425e1b08-8789-4377-85f7-c13131fae4ce
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: e9171844e2fbac7a4e734be9144b30d1bbd0397f
ms.lasthandoff: 03/30/2017


---
# <a name="reference---iot-hub-quotas-and-throttling"></a>Referenz: IoT Hub-Kontingente und -Drosselung

## <a name="quotas-and-throttling"></a>Kontingente und Drosselung
Jedes Azure-Abonnement kann maximal zehn IoT-Hubs und höchstens einen Hub vom Typ „Free“ enthalten.

Jeder IoT Hub wird mit einer bestimmten Anzahl von Einheiten in einer spezifischen SKU bereitgestellt (weitere Informationen finden Sie unter [Azure IoT Hub – Preise][lnk-pricing]). Die SKU und Anzahl der Einheiten bestimmen das maximale tägliche Kontingent von Nachrichten, die Sie senden können.

Die SKU legt auch die Drosselungslimits fest, die IoT Hub für alle Vorgänge erzwingt.

## <a name="operation-throttles"></a>Vorgangsdrosselung
Bei der Vorgangsdrosselung wird die Datenübertragungsrate pro Minute begrenzt, um einen Missbrauch zu verhindern. IoT Hub versucht, möglichst keine Fehler zurückzugeben, es werden jedoch Ausnahmen ausgelöst, wenn die Drosselungsgrenze zu lange überschritten wird.

Die nachfolgende Liste zeigt alle erzwungenen Werte für die Drosselung. Die Werte beziehen sich auf einen einzelnen Hub.

| Drosselung | Free- und S1-Hubs | S2-Hubs | S3-Hubs | 
| -------- | ------- | ------- | ------- |
| Identitätsregistrierungsvorgänge (Erstellen, Abrufen, Aktualisieren, Löschen) | 1,67/Sekunde/Einheit (100/Minute/Einheit) | 1,67/Sekunde/Einheit (100/Minute/Einheit) | 83,33/Sekunde/Einheit (5.000/Minute/Einheit) |
| Geräteverbindungen | Max. 100/Sekunde oder 12/Sekunde/Einheit <br/> Zwei S1-Einheiten entsprechen beispielsweise 2\*12 = 24 Sekunden. Es sind jedoch mindestens 100 Sekunden auf die Einheiten verteilt vorhanden. Mit neun S1-Einheiten erhalten Sie 108 Sekunden (9\*12) über alle Einheiten. | 120/Sekunde/Einheit | 6.000/Sekunde/Einheit |
| Senden von Nachrichten von Geräten an die Cloud | Max. 100/Sekunde oder 12/Sekunde/Einheit <br/> Zwei S1-Einheiten entsprechen beispielsweise 2\*12 = 24 Sekunden. Es sind jedoch mindestens 100 Sekunden auf die Einheiten verteilt vorhanden. Mit neun S1-Einheiten erhalten Sie 108 Sekunden (9\*12) über alle Einheiten. | 120/Sekunde/Einheit | 6.000/Sekunde/Einheit |
| C2D-Sendevorgänge | 1,67/Sekunde/Einheit (100/Minute/Einheit) | 1,67/Sekunde/Einheit (100/Minute/Einheit) | 83,33/Sekunde/Einheit (5.000/Minute/Einheit) |
| C2D-Empfangsvorgänge <br/> (nur bei Verwendung von HTTP durch das Gerät)| 16,67/Sekunde/Einheit (1.000/Minute/Einheit) | 16,67/Sekunde/Einheit (1.000/Minute/Einheit) | 833,33/Sekunde/Einheit (50.000/Minute/Einheit) |
| Dateiupload | 1,67 Dateiuploadbenachrichtigungen/Sekunde/Einheit (100/Minute/Einheit) | 1,67 Dateiuploadbenachrichtigungen/Sekunde/Einheit (100/Minute/Einheit) | 83,33 Dateiuploadbenachrichtigungen/Sekunde/Einheit (5.000/Minute/Einheit) |
| Direkte Methoden | 10/Sekunde/Einheit | 30/Sekunde/Einheit | 1.500/Sekunde/Einheit | 
| Gerätezwilling-Lesevorgänge | 10/Sekunde | Höchstens 10/Sekunde oder 1/Sekunde/Einheit | 50/Sekunde/Einheit |
| Gerätezwillingsaktualisierungen | 10/Sekunde | Höchstens 10/Sekunde oder 1/Sekunde/Einheit | 50/Sekunde/Einheit |
| Auftragsvorgänge <br/> (Erstellen, Aktualisieren, Auflisten, Löschen) | 1,67/Sekunde/Einheit (100/Minute/Einheit) | 1,67/Sekunde/Einheit (100/Minute/Einheit) | 83,33/Sekunde/Einheit (5.000/Minute/Einheit) |
| Durchsatz für Vorgänge vom Typ „Aufträge pro Gerät“ | 10/Sekunde | Höchstens 10/Sekunde oder 1/Sekunde/Einheit | 50/Sekunde/Einheit |

Hier muss gesagt werden, dass die Drosselung der *Geräteverbindungen* nicht die maximale Anzahl gleichzeitig verbundener Geräte bestimmt, sondern die Rate, mit der neue Geräteverbindungen mit einem IoT Hub eingerichtet werden können. Die Drosselung ist abhängig von der Anzahl der Einheiten, die für den IoT-Hub bereitgestellt werden.

Wenn Sie beispielsweise eine S1-Einheit erwerben, erhalten Sie eine Drosselung von 100 Verbindungen pro Sekunde. Darum dauert das Herstellen einer Verbindung mit 100.000 Geräten mindestens 1.000 Sekunden (ca. 16 Minuten). Es können jedoch so viele Geräte gleichzeitig verbunden sein, wie in der Identitätsregistrierung registriert sind.

Eine ausführliche Erläuterung der IoT Hub-Drosselung finden Sie in dem Blogbeitrag [IoT Hub throttling and you][lnk-throttle-blog] (Was habe ich mit der IoT Hub-Drosselung zu tun?).

> [!NOTE]
> Die Kontingente oder Drosselungsgrenzwerte können jederzeit angehoben werden, indem die Anzahl von bereitgestellten Einheiten in einem IoT Hub erhöht wird.
> 
> [!IMPORTANT]
> Identitätsregistrierungsvorgänge sind für die Verwendung zur Laufzeit in Szenarien für die Geräteverwaltung und -bereitstellung vorgesehen. Das Lesen oder Aktualisieren einer großen Anzahl von Geräteidentitäten wird durch [Import-/Exportaufträge][lnk-importexport] unterstützt.
> 
> 

## <a name="other-limits"></a>Andere Limits

IoT Hub erzwingt andere Grenzwerte für die verschiedenen Funktionen.

| Vorgang | Begrenzung |
| --------- | ----- |
| Dateiupload-URIs | 10.000 SAS-URIs können gleichzeitig für ein Speicherkonto geöffnet sein. <br/> 10 SAS-URIs/Gerät können gleichzeitig geöffnet sein. |
| Aufträge | Der Auftragsverlauf wird bis zu 30 Tage lang gespeichert. <br/> Maximale Anzahl gleichzeitiger Aufträge: 1 (für Free und S1), 5 (für S2), 10 (für S3) |
| Zusätzliche Endpunkte | Kostenpflichtige SKU-Hubs haben möglicherweise 10 zusätzliche Endpunkte. Kostenfreie SKU-Hubs haben möglicherweise einen zusätzlichen Endpunkt. |
| Regeln für die Nachrichtenweiterleitung | Kostenpflichtige SKU-Hubs haben möglicherweise 100 Weiterleitungsregeln. Kostenfreie SKU-Hubs haben möglicherweise fünf Weiterleitungsregeln. |

> [!NOTE]
> Derzeit können höchstens 500.000 Geräte mit einem einzelnen IoT Hub verbunden werden. Wenn Sie diesen Grenzwert erhöhen möchten, wenden Sie sich an den [Microsoft-Support](https://azure.microsoft.com/en-us/support/options/).

## <a name="next-steps"></a>Nächste Schritte
Weitere Referenzthemen in diesem IoT Hub-Entwicklungsleitfaden:

* [IoT Hub-Endpunkte][lnk-devguide-endpoints]
* [IoT Hub-Abfragesprache für Gerätezwillinge und Aufträge][lnk-devguide-query]
* [IoT Hub-MQTT-Unterstützung][lnk-devguide-mqtt]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

