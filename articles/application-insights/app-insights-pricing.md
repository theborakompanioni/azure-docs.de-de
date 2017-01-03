---
title: "Verwalten von Funktionen und Datenvolumen für Application Insights | Microsoft Docs"
description: "Verwalten Sie Telemetriedatenvolumen, und überwachen Sie Kosten in Application Insights."
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
ms.assetid: ebd0d843-4780-4ff3-bc68-932aa44185f6
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 3fdb050d1d6746313b2dffb089d56b71908335d2
ms.openlocfilehash: 585e493b2642b3ba798977528f7564674a44d738


---
# <a name="manage-features-and-data-volume-in-application-insights"></a>Verwalten von Funktionen und Datenvolumen in Application Insights


Preise für [Azure Application Insights][start] basieren auf dem Datenvolumen pro Anwendung. Eine geringe Nutzung während der Entwicklung oder für eine kleinen App ist voraussichtlich kostenlos, da es für Telemetriedaten ein kostenloses monatliches Kontingent gibt.

Jede Application Insights-Ressource wird als separater Dienst abgerechnet und auf der Rechnung für Ihr Azure-Abonnement aufgeführt.

Es gibt zwei Tarife. Der Standardtarif heißt „Basic“. Sie können sich für den Tarif „Enterprise“ entscheiden, für den eine tägliche Gebühr anfällt. Dieser bietet aber zusätzliche Features wie z.B. den [fortlaufenden Export](app-insights-export-telemetry.md).

[Informieren Sie sich über die Tarife][pricing].

## <a name="review-pricing-plan-for-your-application-insights-resource"></a>Überprüfen des Tarifs für Ihre Application Insights-Ressource
Öffnen Sie in der Application Insights-Ressource für Ihre Anwendung das Blatt „Features und Preise“.

![Wählen Sie „Preise“ aus.](./media/app-insights-pricing/01-pricing.png)

1. Überprüfen Sie Ihr Datenvolumen für den Monat. Dies schließt alle nach einer beliebigen [Stichprobennahme](app-insights-sampling.md) in Ihren Server- und Client-Apps sowie Verfügbarkeitstests empfangenen und beibehaltenen Daten ein.
2. Für [Webtests mit mehreren Schritten](app-insights-monitor-web-app-availability.md#multi-step-web-tests) wird eine gesonderte Gebühr erhoben. (Darin nicht enthalten sind einfache Verfügbarkeitstests, die von der Gebühr für das Datenvolumen abgedeckt sind.)
3. Aktivieren Sie die zusätzlichen Funktionen, die vom Tarif „Enterprise“ geboten werden. In diesem Tarif gibt es kein kostenloses Datenkontingent.
4. Navigieren Sie zu den Datenverwaltungsoptionen, um eine tägliche Obergrenze festzulegen, oder legen Sie „Erfassungs-Stichprobenerstellung“ fest.

Die Gebühren für Application Insights werden Ihrer Azure-Rechnung hinzugefügt. Sie sehen die Details zu Ihrer Azure-Rechnung im Bereich "Abrechnung" des Azure-Portals oder im [Azure-Abrechnungsportal](https://account.windowsazure.com/Subscriptions). 

![Wählen Sie im seitlichen Menü die Option „Abrechnung“.](./media/app-insights-pricing/02-billing.png)

## <a name="data-rate"></a>Datenrate
Es gibt drei Möglichkeiten zum Begrenzen des gesendeten Datenvolumens:

* Tägliche Obergrenze. Standardmäßig ist diese Einstellung auf 100 GB pro Tag festgelegt. Wenn Ihre App die Obergrenze erreicht, senden wir eine E-Mail und verwerfen Daten bis zum Ende des Tages. Ändern Sie diesen Wert auf dem Blatt zur Datenvolumenverwaltung.
* [Stichprobennahme](app-insights-sampling.md). Dieser Mechanismus kann die Menge der von Ihrem Server und Ihren Client-Apps gesendeten Telemetriedaten bei minimaler Verzerrung von Metriken verringern.
* Durch eine Drosselung wird die Datenrate pro Minute eingeschränkt. Beim Tarif „Basic“ liegt die Grenze bei 200 Datenpunkten pro Sekunde, gemittelt über 5 Minuten. Beim Tarif „Enterprise“ ist der Grenzwert 500/s, gemittelt über eine Minute. 

Zur Drosselung werden drei Buckets getrennt gezählt:

* [TrackTrace-Aufrufe](app-insights-api-custom-events-metrics.md#track-trace) und [Erfasste Protokolle](app-insights-asp-net-trace-logs.md)
* [Ausnahmen](app-insights-api-custom-events-metrics.md#track-exception), begrenzt auf 50 Punkte/s
* Alle anderen Telemetriedaten (Seitenaufrufe, Sitzungen, Anforderungen, Abhängigkeiten, Metriken, benutzerdefinierte Ereignisse).

*Was geschieht, wenn meine App, das Pro-Sekunde-Volumen überschreitet?*

* Die Datenmenge, die Ihre App sendet, wird minütlich gemessen. Wenn sie die pro Sekunde, über eine Minute gemittelt Datenmenge überschreitet, lehnt der Server einige Anforderungen ab. Das SDK puffert die Daten und versucht dann, erneut zu senden, wobei ein rascher Anstieg über mehrere Minuten hinweg verteilt wird. Wenn Ihre App die Drosselungsrate beim Senden von Daten laufend überschreitet, werden einige Daten gelöscht. (Die ASP.NET-, Java- und JavaScript-SDKs versuchen auf diese Weise erneut zu senden, andere SDKs löschen gedrosselte Daten möglicherweise einfach.)

Tritt eine Drosselung auf, erhalten Sie zur Warnung eine Benachrichtigung über diesen Vorgang.

*Woher weiß ich, wie viele Datenpunkte meine App sendet?*

* Öffnen Sie das Blatt „Preise“ erneut, um das Diagramm des Datenvolumens anzuzeigen.
* Oder fügen Sie im Metrik-Explorer ein neues Diagramm hinzu, und wählen Sie **Datenpunktvolumen** als Metrik aus. Aktivieren Sie "Gruppierung", und gruppieren Sie nach **Datentyp**.

## <a name="to-reduce-your-data-rate"></a>So verringern Sie die Datenrate
Hier sind einige Schritte, die Sie ausführen können, um Ihr Datenvolumen zu reduzieren:

* Senken Sie die tägliche Volumenobergrenze. Die Standardeinstellung ist 100 GB pro Tag.
* Verwenden Sie [Stichproben](app-insights-sampling.md). Diese Technologie verringert die Datenrate, ohne die Metriken zu verzerren und ohne die Navigation zwischen verwandten Elementen bei der Suche zu stören. In Server-Apps arbeitet sie automatisch.
* [Begrenzen Sie die Anzahl der gemeldeten AJAX-Aufrufe](app-insights-javascript.md#detailed-configuration) für jeden Seitenaufruf, oder deaktivieren Sie AJAX-Berichte.
* Deaktivieren Sie nicht benötigte Erfassungsmodule durch [Bearbeiten von „ApplicationInsights.config“](app-insights-configuration-with-applicationinsights-config.md). Das kann z. B. für Leistungsindikator- oder Abhängigkeitsdaten gelten.
* Teilen Sie Ihre Telemetrie auf getrennte Instrumentierungsschlüssel auf. 
* Aggregieren Sie Metriken vorab. Wenn Sie Ihrer App Aufrufe an TrackMetric eingefügt haben, können Sie Datenverkehr reduzieren, indem Sie die Überladung verwenden, die Ihre Berechnung des Durchschnitts und die Standardabweichung eines Batches von Messungen akzeptiert. Oder Sie können ein [vorab aggregierendes Paket](https://www.myget.org/gallery/applicationinsights-sdk-labs)verwenden. 

## <a name="sampling"></a>Stichproben
Die [Stichprobenerstellung](app-insights-sampling.md) ist eine Methode, die Rate, mit der Telemetriedaten an Ihre App gesendet werden, zu verringern. Gleichzeitig soll die Möglichkeit erhalten bleiben, bei Diagnosesuchläufen relevante Ereignisse zu ermitteln und korrekte Ereigniszahlen zu erhalten. 

Die Stichprobenerstellung ist eine effektive Möglichkeit, die Gebühren zu senken und innerhalb Ihres monatlichen Kontingents zu bleiben. Der Stichprobenalgorithmus behält Elemente in Bezug auf die Telemetrie bei, sodass Sie beispielsweise in Search die Anforderung ermitteln können, die in Beziehung zu einer bestimmten Ausnahme steht. Der Algorithmus behält außerdem korrekte Zahlenwerte bei, sodass Sie im Metrik-Explorer die richtigen Werte für Anforderungsraten, Ausnahmeraten und weitere Messwerte sehen.

Es gibt verschiedene Formen der Stichprobenerstellung.

* [Adaptive sampling](app-insights-sampling.md) verwendet, bei der die Menge der an Ihre App gesendeten Telemetriedaten automatisch angepasst wird. Die Stichprobenerstellung erfolgt im SDK Ihrer Web-App automatisch, sodass der Telemetriedatenverkehr im Netzwerk verringert wird. 
* *Erfassungs-Stichprobenerstellung* stellt eine Alternative dar, die an dem Punkt arbeitet, an dem Telemetriedaten von Ihrer App den Application Insights-Dienst erreichen. Diese Art der Stichprobenerstellung wirkt sich nicht auf die Menge der Telemetriedaten aus, die von Ihrer App gesendet werden, verringert jedoch die Menge der Daten, die vom Dienst beibehalten werden. Damit können Sie das Kontingent verringern, das von Telemetriedaten von Browsern und anderen SDKs beansprucht wird.

Konfigurieren Sie zum Festlegen der Erfassungs-Stichprobenerstellung die Einstellung auf dem Blatt „Preise“:

![Klicken Sie im Blatt „Kontingent und Preise“ auf die Kachel „Stichproben“, und wählen Sie eine Einheit für die Stichprobenerstellung.](./media/app-insights-pricing/04.png)

> [!WARNING]
> Der auf der Kachel „Beibehaltene Stichproben“ angegebene Wert gibt nur den Wert an, den Sie für die Erfassungs-Stichprobenerstellung festgelegt haben. Er zeigt nicht den Stichproben-Prozentsatz, der im SDK in Ihrer App angewendet wird. 
> 
> Falls für die eingehenden Telemetriedaten bereits im SDK Stichproben erstellt wurden, wird die Erfassungs-Stichprobenerstellung nicht angewendet.
> 
> 

Verwenden Sie etwa folgende [Analytics-Abfrage](app-insights-analytics.md) , um den tatsächlichen Stichproben-Prozentsatz unabhängig davon zu ermitteln, wo er angewendet wird:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h) 
    | render areachart 

In jedem beibehaltenen Datensatz gibt `itemCount` die Anzahl ursprünglicher Datensätze an, die der Datensatz darstellt (Anzahl zuvor verworfener Datensätze + 1). 


## <a name="limits-summary"></a>Zusammenfassung der Grenzwerte
[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Stichproben](app-insights-sampling.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: app-insights-overview.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/




<!--HONumber=Nov16_HO3-->


