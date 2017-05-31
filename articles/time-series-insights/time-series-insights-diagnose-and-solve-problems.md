---
title: Diagnose und Problembehandlung | Microsoft-Dokumentation
description: "In diesem Tutorial erfahren Sie, wie Sie Probleme in Ihrer Time Series Insights-Umgebung diagnostizieren und lösen."
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/24/2017
ms.author: venkatja
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: 4b8d5fdab1744b2db658917f91d6dac05db30d2f
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017

---
# <a name="diagnose-and-solve-problems-in-your-time-series-insights-environment"></a>Diagnostizieren und Beheben von Problemen in der Time Series Insights-Umgebung

## <a name="i-dont-see-my-data"></a>Ich sehe meine Daten nicht.
Es folgen einige mögliche Ursachen dafür, warum Sie Ihre Daten nicht in Ihrer Umgebung im [Azure Time Series Insights-Portal](https://insights.timeseries.azure.com) sehen.

### <a name="your-event-source-doesnt-have-data-in-json-format"></a>Ihre Ereignisquelle enthält keine Daten im JSON-Format.
Azure Time Series Insights unterstützt derzeit nur JSON-Daten. JSON-Beispiele finden Sie unter [Unterstützte JSON-Formen](time-series-insights-send-events.md#supported-json-shapes).

### <a name="when-you-registered-your-event-source-you-didnt-provide-the-key-that-has-the-required-permission"></a>Sie haben bei der Registrierung der Ereignisquelle keinen Schlüssel mit den erforderlichen Berechtigungen angegeben.
* Für IoT Hub müssen Sie einen Schlüssel mit der Berechtigung **Dienstverbindung** bereitstellen.

   ![Berechtigung zur Verbindung mit dem IoT Hub-Dienst](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)

   Wie in der vorherigen Abbildung dargestellt, würden die Richtlinien **iothubowner** und **service** funktionieren, da beide die Berechtigung **Dienstverbindung** einschließen.
* Für einen Event Hub müssen Sie einen Schlüssel mit der Berechtigung **Lauschen** bereitstellen.

   ![Event Hub-Lauschberechtigung](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

   Wie in der vorherigen Abbildung dargestellt, würden die Richtlinien **read** oder **write** funktionieren, da beide die Berechtigung **Lauschen** einschließen.

### <a name="the-provided-consumer-group-is-not-exclusive-to-time-series-insights"></a>Die bereitgestellte Consumergruppe ist für Time Series Insights nicht exklusiv.
Bei IoT Hub oder Event Hub müssen Sie während der Registrierung die Consumergruppe angeben, die beim Lesen Ihrer Daten verwendet werden soll. Diese Consumergruppe darf nicht freigegeben werden. Wenn diese freigegeben wird, trennt der zugrunde liegende Event Hub automatisch einen zufälligen Leser.

## <a name="i-see-my-data-but-theres-a-lag"></a>Ich sehe meine Daten zwar, jedoch nur mit Verzögerung.
Es folgen einige mögliche Ursachen dafür, warum ein Teil der Daten nicht in Ihrer Umgebung im [Time Series Insights-Portal](https://insights.timeseries.azure.com) angezeigt wird.

### <a name="your-environment-is-getting-throttled"></a>Ihre Umgebung wird gedrosselt.
Der Drosselungsgrenzwert wird basierend auf dem Typ und der Kapazität der Umgebungs-SKU erzwungen. Alle Ereignisquellen in der Umgebung nutzen diese Kapazität gemeinsam. Wenn Ihre Event Hub-/IoT Hub-Ereignisquelle Daten per Push über die erzwungenen Grenzwerte hinaus überträgt, treten die Drosselung und eine Verzögerung ein.

Die folgende Abbildung zeigt eine Time Series Insights-Umgebung mit SKU S1 und Kapazität 3. Es können 3 Millionen Ereignisse pro Tag eingehen.

![Aktuelle Kapazität der Umgebungs-SKU](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

Nehmen wir an, dass diese Umgebung Nachrichten von einem Event Hub mit der in folgender Abbildung gezeigten Eingangsrate erfasst:

![Beispielerfassungsrate für einen Event Hub](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

Wie in der Abbildung dargestellt wird, beträgt die tägliche Eingangsrate ca. 67.000 Nachrichten. Diese Rate übersetzt ungefähr 46 Nachrichten pro Minute. Wenn jede Event Hub-Nachricht auf ein einzelnes Time Series Insights-Ereignis reduziert wird, wird keine Drosselung auf diese Umgebung angewendet. Wenn jede Event Hub-Nachricht auf 100 Time Series Insights-Ereignisse reduziert wird, sollten 4.600 Ereignisse pro Minute erfasst werden. Eine SKU S1-Umgebung mit einer Kapazität von 3 kann nur 2.100 Ereignisse pro Minute erfassen (1 Mio. Ereignisse pro Tag = 700 Ereignisse pro Minute von 3 Einheiten = 2.100 Ereignisse pro Minute). Daher tritt aufgrund der Drosselung eine Verzögerung auf. 

Eine Übersicht über die Funktionsweise der Vereinfachungslogik finden Sie unter [Unterstützte JSON-Formen](time-series-insights-send-events.md#supported-json-shapes).

#### <a name="recommended-steps"></a>Empfohlene Schritte
Um die Verzögerung zu beheben, erhöhen Sie die SKU-Kapazität Ihrer Umgebung. Weitere Informationen finden Sie unter [Skalieren der Time Series Insights-Umgebung](time-series-insights-how-to-scale-your-environment.md).

### <a name="youre-pushing-historical-data-and-causing-slow-ingress"></a>Sie übertragen Verlaufsdaten und verursachen damit eine langsamere Erfassung.
Wenn Sie eine Verbindung mit einer vorhandenen Ereignisquelle herstellen, besteht die Möglichkeit, dass Ihr Event Hub oder IoT Hub darin bereits Daten aufbewahrt. Die Umgebung beginnt daher mit dem Abrufen von Daten am Anfang der Vermerkdauer für Nachrichten aus der Ereignisquelle. 

Dieses Verhalten ist das Standardverhalten und kann nicht überschrieben werden. Möglicherweise lösen Sie damit eine Drosselung aus, sodass es eine Weile dauert, bis die Erfassung von Verlaufsdaten abgeschlossen ist.

#### <a name="recommended-steps"></a>Empfohlene Schritte
Führen Sie zum Beheben dieser Verzögerung die folgenden Schritte aus:
1. Erhöhen Sie die SKU-Kapazität auf den maximal zulässigen Wert (in diesem Fall 10). Sobald die Kapazität erhöht wird, schließt der Erfassungsvorgang wesentlich schneller die alten Daten ab. Sie können die Verarbeitungsgeschwindigkeit der alten Daten über das Verfügbarkeitsdiagramm im [Time Series Insights-Portal](https://insights.timeseries.azure.com) visualisieren. Die erhöhte Kapazität wird Ihnen in Rechnung gestellt.
2. Sobald die Verzögerung abgefangen wurde, können Sie die SKU-Kapazität wieder auf die normale Erfassungsrate verringern.

## <a name="my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Meine Einstellung für den *Namen für die Zeitstempeleigenschaft* der Ereignisquelle funktioniert nicht.
Stellen Sie sicher, dass Name und Wert den folgenden Regeln entsprechen:
* Beachten Sie beim Namen der Zeitstempeleigenschaft die _Groß-/Kleinschreibung_.
* Der Wert für die Zeitstempeleigenschaft, der als JSON-Zeichenfolge von Ihrer Ereignisquelle eingeht, sollte das Format _JJJJ-MM-TTTHH:mm:ss.FFFFFFFK_ aufweisen. Ein Beispiel für eine solche Zeichenfolge ist „2008-04-12T12:53Z“.

