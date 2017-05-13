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
ms.sourcegitcommit: 7f8b63c22a3f5a6916264acd22a80649ac7cd12f
ms.openlocfilehash: 46d58e94f3fede3f7f19de8d97e23ac6910bc12a
ms.contentlocale: de-de
ms.lasthandoff: 05/01/2017

---
# <a name="diagnose-and-solve-problems"></a>Diagnose und Problembehandlung

## <a name="i-do-not-see-my-data"></a>Ich sehe meine Daten nicht.
Die folgenden Gründe könnten möglicherweise die Ursache sein, warum Sie Ihre Daten nicht in Ihrer Umgebung im [Time Series-Insights-Portal](https://insights.timeseries.azure.com) sehen.

### <a name="does-your-event-source-have-data-in-json-format"></a>Enthält Ihre Ereignisquelle Daten im JSON-Format?
Azure Time Series Insights unterstützt derzeit nur JSON-Daten. JSON-Beispiele finden Sie im Abschnitt *Unterstützte JSON-Formen* [hier](time-series-insights-send-events.md#supported-json-shapes).

### <a name="when-registering-your-event-source-did-you-provide-the-key-with-required-permissions"></a>Haben sie bei der Registrierung Ihrer Ereignisquelle den Schlüssel mit den erforderlichen Berechtigungen bereitgestellt?
1. Für IoTHub müssen Sie den Schlüssel mit *Dienstverbindungsberechtigung* bereitstellen.

   ![Berechtigung zur Verbindung mit dem IotHub-Dienst](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)

   Wie in der vorherigen Abbildung dargestellt, würde die Richtlinie „iothubowner“ oder „service“ funktionieren, da beide mit der Berechtigung „Dienstverbindung“ ausgestattet sind.
2. Bei EventHub müssen Sie den Schlüssel mit der Berechtigung *Lauschen* bereitstellen.

   ![Event Hub-Lauschberechtigung](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

   Wie in der vorherigen Abbildung dargestellt, würde die Richtlinie „lesen“ oder „verwalten“ funktionieren, da beide mit der Berechtigung „Lesen“ ausgestattet sind.

### <a name="are-you-sure-that-the-consumer-group-provided-is-exclusive-to-time-series-insights"></a>Sind Sie sicher, dass die bereitgestellte Consumergruppe exklusiv für Time Series Insights gilt?
Bei IoTHub oder EventHub müssen Sie während der Registrierung die Consumergruppe angeben, die beim Lesen Ihrer Daten verwendet werden soll. Diese Consumergruppe darf nicht freigegeben werden. Wenn diese freigegeben wird, trennt der zugrunde liegende Event Hub willkürlich automatisch einen der Leser.

## <a name="i-see-my-data-but-there-is-a-lag"></a>Ich sehe meine Daten zwar, jedoch nur mit Verzögerung.
Die folgenden Gründe könnten möglicherweise die Ursache sein, warum Sie einen Teil der Daten nicht in Ihrer Umgebung im [Time Series-Insights-Portal](https://insights.timeseries.azure.com) sehen.

### <a name="your-environment-might-be-getting-throttled"></a>Ihre Umgebung wird möglicherweise gedrosselt.
Der Einschränkungsgrenzwert wird basierend auf der Art und Kapazität der Umgebungs-SKU erzwungen. Alle Ereignisquellen in der Umgebung gemeinsam nutzen diese Kapazität. Wenn Ihre Event Hub-/IoT Hub-Ereignisquelle Daten per Push über die erzwungenen Grenzwerte hinaus überträgt, sehen Sie die Drosselung und Verzögerung.

Die folgende Abbildung zeigt eine Time Series Insights-Umgebung mit SKU S1 und Kapazität 3. Es können 3 Millionen Ereignisse pro Tag eingehen.

![Aktuelle Kapazität der Umgebungs-SKU](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

Nehmen wir an, dass diese Umgebung Nachrichten von einem Event Hub mit der in folgender Abbildung gezeigten Eingangsrate erfasst:

![Aktuelle Kapazität der Umgebungs-SKU](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

Wie in der Abbildung dargestellt wird, beträgt die tägliche Eingangsrate ca. 67.000 Nachrichten. Diese Rate übersetzt ungefähr 46 Nachrichten pro Minute. Wenn jede Event Hub-Nachricht auf ein einzelnes Time Series Insights-Ereignis reduziert wird, wird keine Drosselung auf diese Umgebung angewendet. Wenn jede Event Hub-Nachricht auf 100 Time Series Insights-Ereignisse reduziert wird, sollten 4.600 Ereignisse pro Minute erfasst werden. Eine S1-SKU-Umgebung mit einer Kapazität von drei kann lediglich 2.100 Ereignisse pro Minute erfassen. (1 Mio. Ereignisse pro Tag => 700 Ereignisse pro Minute, 3 Einheiten => 2.100 Ereignisse pro Minute) Die Verzögerung tritt daher aufgrund der Drosselung auf. Einen Überblick über die Funktionsweise der Vereinfachungslogik finden Sie im Abschnitt *Unterstützte JSON-Formen* [hier](time-series-insights-send-events.md#supported-json-shapes).

#### <a name="recommended-steps"></a>Empfohlene Schritte
Um die Verzögerung zu beheben, erhöhen Sie die SKU-Kapazität Ihrer Umgebung. [Vorgehensweise zur Skalierung Ihrer Time Series Insights-Umgebung](time-series-insights-how-to-scale-your-environment.md)

### <a name="you-might-be-pushing-historical-data-and-hence-the-slow-ingress"></a>Die langsame Erfassung ist möglicherweise auf eine Pushübertragung von Verlaufsdaten zurückzuführen.
Wenn Sie eine Verbindung zu einer vorhandenen Ereignisquelle herstellen, besteht die Wahrscheinlichkeit, dass Ihr Event Hub/IoT Hub bereits Daten darin enthält. Die Umgebung beginnt daher mit dem Abruf von Daten am Anfang der Beibehaltungsdauer für Nachrichten aus der Ereignisquelle. Dieses Verhalten ist das Standardverhalten und kann nicht überschrieben werden. Eine Drosselung kann möglicherweise belegt sein und dauert möglicherweise eine Weile, bis die Erfassung von Verlaufsdaten aufgefangen ist.

#### <a name="recommended-steps"></a>Empfohlene Schritte
Um die Verzögerung zu beheben, führen Sie die folgenden Schritte aus:
1. Erhöhen Sie die SKU-Kapazität auf den maximal zulässigen Wert (in diesem Fall 10). Sobald die Kapazität erhöht wird, startet der Erfassungsvorgang wesentlich schneller mit dem Abfangen. Sie können visualisieren, wie schnell der Auffangvorgang über das Verfügbarkeitsdiagramm im [Time Series-Insights-Portal](https://insights.timeseries.azure.com) erfolgen soll. Die erhöhte Kapazität wird Ihnen in Rechnung gestellt.
2. Sobald die Verzögerung abgefangen wird, verringern Sie die SKU-Kapazität wieder auf die normale Eingangsrate.

## <a name="my-event-source-timestamp-property-name-setting-does-not-work"></a>Meine Einstellung für den *Namen für die Zeitstempeleigenschaft* der Ereignisquelle funktioniert nicht.
Stellen Sie sicher, dass Name und Wert den folgenden Regeln entsprechen:
1. Beachten Sie beim Namen der Zeitstempeleigenschaft die __Groß-/Kleinschreibung__.
2. Der Wert für die Zeitstempeleigenschaft, der als JSON-Zeichenfolge von Ihrer Ereignisquelle eingeht, sollte das Format __JJJJ-MM-TTTHH:mm:ss.FFFFFFFK__ aufweisen. Ein Beispiel für eine solche Zeichenfolge ist „2008-04-12T12:53Z“.
