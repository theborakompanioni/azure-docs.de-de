---
title: "Verwalten von Preisen und Datenvolumen für Azure Application Insights | Microsoft Docs"
description: "Verwalten Sie Telemetriedatenvolumen, und überwachen Sie Kosten in Application Insights."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: ebd0d843-4780-4ff3-bc68-932aa44185f6
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: bwren
ms.translationtype: Human Translation
ms.sourcegitcommit: 245ce9261332a3d36a36968f7c9dbc4611a019b2
ms.openlocfilehash: 311cee724fc77256748153b5167d2a38ccba9775
ms.contentlocale: de-de
ms.lasthandoff: 06/09/2017

---
# <a name="manage-pricing-and-data-volume-in-application-insights"></a>Verwalten von Preisen und Datenvolumen in Application Insights


Die Preise für [Azure Application Insights][start] basieren auf dem Datenvolumen pro Anwendung. Eine geringe Nutzung während der Entwicklung oder für eine kleine App ist voraussichtlich kostenlos, da es für Telemetriedaten ein monatliches Kontingent von 1 GB gibt.

Jede Application Insights-Ressource wird als separater Dienst abgerechnet und auf der Rechnung für Ihr Azure-Abonnement aufgeführt.

Es gibt zwei Tarife. Der Standardtarif heißt „Basic“. Sie können sich für den Tarif „Enterprise“ entscheiden, für den eine tägliche Gebühr anfällt. Dieser bietet aber zusätzliche Features wie z.B. den [fortlaufenden Export](app-insights-export-telemetry.md).

Wenn Sie Fragen zu den Preisen für Application Insights haben, können Sie gerne eine Frage in unserem [Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights) posten. 

## <a name="the-price-plans"></a>Die Tarife

Aktuelle Preise in Ihrer Währung finden Sie auf der [Application Insights-Seite mit Preisen][pricing].

### <a name="basic-plan"></a>Basic-Tarif

Der Basic-Tarif ist die Standardeinstellung, wenn eine neue Application Insights-Ressource erstellt wird. Er ist für die meisten Kunden ausreichend.

* Im Basic-Tarif erfolgt die Abrechnung nach Datenvolumen: Anzahl von Bytes an Telemetriedaten, die von Application Insights empfangen werden. Das Datenvolumen wird anhand der Größe des nicht komprimierten JSON-Datenpakets gemessen, das Application Insights aus Ihrer Anwendung empfängt.
Für [in Analytics importierte Tabellendaten](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-import) wird das Datenvolumen als die unkomprimierte Größe von Dateien gemessen, die an Application Insights gesendet werden.  
* Für jede App ist 1 GB kostenlos. Wenn Sie also nur experimentieren oder entwickeln, ist es unwahrscheinlich, dass Sie dafür zahlen müssen.
* [Live Metrics Stream](app-insights-live-stream.md)-Daten werden bei der Preisgestaltung nicht berechnet.
* [Fortlaufender Export](app-insights-export-telemetry.md) ist im Basic-Tarif gegen eine Zusatzgebühr pro GB verfügbar.

### <a name="enterprise-plan"></a>Enterprise-Tarif

* Im Enterprise-Tarif kann Ihre App alle Funktionen von Application Insights nutzen. [Fortlaufender Export](app-insights-export-telemetry.md) und 

[Log Analytics-Connector](https://go.microsoft.com/fwlink/?LinkId=833039&amp;clcid=0x409) sind im Enterprise-Tarif ohne zusätzliche Kosten verfügbar.
* Sie bezahlen pro Knoten, der Telemetriedaten für Apps im Enterprise-Tarif sendet. 
 * Ein *Knoten* ist entweder ein physischer oder ein virtueller Servercomputer oder eine PaaS-Rolleninstanz (Platform-as-a-Service), der bzw. die Ihre App hostet.
 * Entwicklungscomputer, Clientbrowser und mobile Geräte werden nicht als Knoten gezählt.
 * Wenn die App mehrere Komponenten aufweist, die Telemetriedaten senden, z.B. ein Webdienst und ein Back-End-Worker, werden sie separat gezählt.
 * [Live Metrics Stream](app-insights-live-stream.md)-Daten werden nicht zu Preiszwecken gezählt.* In einem Abonnement fallen Gebühren knoten- und nicht App-bezogen an. Wenn Sie fünf Knoten haben, die Telemetriedaten für 12 Apps senden, wird die Gebühr für fünf Knoten berechnet.
* Obwohl Gebühren pro Monat angegeben sind, müssen Sie nur für jede Stunde bezahlen, in der ein Knoten Telemetriedaten von einer App sendet. Die Stundengebühr wird als Monatsgebühr angegeben: 744 (die Anzahl der Stunden pro Monat mit 31 Tagen).
* Eine Datenvolumenzuteilung von 200 MB pro Tag ist für jeden erkannten Knoten vorgesehen (mit stundenbezogener Granularität). Eine nicht genutzte Datenzuteilung wird nicht von einem Tag auf den nächsten übertragen.
 * Wenn Sie sich für die Preisoption Enterprise entscheiden, wird jedem Abonnement basierend auf der Anzahl von Knoten, die in diesem Abonnement Telemetriedaten an Application Insights-Ressourcen senden, ein tägliches Datenkontingent zugeordnet. Wenn Sie also fünf Knoten haben, die den ganzen Tag Daten senden, wird Ihnen insgesamt 1 GB für alle Application Insights-Ressourcen in diesem Abonnement zugeteilt. Es ist dabei unerheblich, ob bestimmte Knoten mehr Daten senden als andere, da die enthaltene Datenmenge für alle Knoten zusammen berechnet wird. Wenn die Application Insights-Ressourcen an einem bestimmten Tag mehr Daten empfangen, als in der täglichen Datenzuteilung für dieses Abonnement vorgesehen ist, gelten Gebühren pro GB Überschreitungsdaten. 
 * Das tägliche Datenkontingent wird berechnet aus der Anzahl der Stunden pro Tag (nach UTC), die jeder Knoten Telemetriedaten sendet, dividiert durch 24 mal 200 MB. Wenn Sie also über vier Knoten verfügen, die an 15 Stunden pro Tag Telemetriedaten senden, sind pro Tag ((4 x 15) / 24) x 200 MB = 500 MB an Daten enthalten. Bei einem Preis von 2,30 USD pro GB für Datenüberschreitung wäre die Gebühr 1,15 USD, wenn die Knoten an diesem Tag 1 GB an Daten senden.
 * Beachten Sie, dass das tägliche Kontingent gemäß Enterprise-Tarif nicht für Anwendungen gilt, für die Sie die Basic-Option ausgewählt haben, und ein nicht genutztes Kontingent wird nicht auf den nächsten Tag übertragen. 
* Im Folgenden finden Sie einige Beispiele zum Bestimmen der eindeutigen Knotenanzahl:
| Szenario                               | Gesamtanzahl der Knoten pro Tag |
|:---------------------------------------|:----------------:|
| 1 Anwendung verwendet 3 Azure App Service-Instanzen und 1 virtuellen Server | 4 |
| 3 Anwendungen auf 2 virtuellen Computern, und die Application Insights-Ressourcen für diese Anwendungen befinden sich im selben Abonnement und gehören zum Enterprise-Tarif | 2 | 
| 4 Anwendungen, deren Application Insights-Ressourcen Teil des gleichen Abonnements sind. Jede Anwendung führt in 16 Stunden außerhalb der Spitzenzeiten 2 Instanzen und in 8 Stunden während der Spitzenzeiten 4 Instanzen aus. | 13,33 | 
| Clouddienste mit 1 Workerrolle und 1 Webrolle, die je 2 Instanzen ausführen | 4 | 
| Service Fabric-Cluster mit 5 Knoten und 50 Microservices, wobei jeder Microservice 3 Instanzen ausführt | 5|

* Die genaue Knotenanzahl hängt vom Application Insights SDK ab, das von der Anwendung verwendet wird. 
  * In den SDK-Versionen 2.2 und höher melden das [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) und das [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) von Application Insights jeden Anwendungshost als Knoten, z.B. den Computernamen für physische Server und VM-Hosts oder den Instanznamen bei Clouddiensten.  Die einzige Ausnahme bilden Anwendungen, die nur [.NET Core](https://dotnet.github.io/) und das Application Insights Core SDK verwenden. In diesem Fall wird nur ein Knoten für alle Hosts gemeldet, da der Hostname nicht verfügbar ist. 
  * Für frühere Versionen des SDK verhält sich das [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) wie die höheren SDK-Versionen, das [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) meldet jedoch nur einen Knoten, unabhängig von der Anzahl der tatsächlichen Anwendungshosts. 
  * Beachten Sie, dass Ihre Anwendung das SDK verwendet, um „roleInstance“ auf einen benutzerdefinierten Wert festzulegen. Standardmäßig wird dieser Wert verwendet, um die Anzahl der Knoten zu bestimmen. 
  * Wenn Sie eine neue SDK-Version mit einer App verwenden, die über Clientcomputer oder mobile Geräte ausgeführt wird, ist es möglich, dass für die Anzahl der Knoten eine sehr hohe Zahl zurückgegeben wird (durch die große Anzahl von Clientcomputern oder mobilen Geräten). 

### <a name="multi-step-web-tests"></a>Webtests mit mehreren Schritten

Für [Webtests mit mehreren Schritten](app-insights-monitor-web-app-availability.md#multi-step-web-tests) wird eine zusätzliche Gebühr erhoben. Dies sind Webtests, die eine Sequenz von Aktionen ausführen. 

Es gibt keine gesonderte Gebühr für „Pingtests“ einer einzelnen Seite. Telemetriedaten von Pingtests und Tests mit mehreren Schritten werden zusammen mit anderen Telemetriedaten aus Ihrer App in Rechnung gestellt.
 
## <a name="operations-management-suite-subscription-entitlement"></a>Berechtigung für Operations Management Suite-Abonnement

Wie [vor Kurzem angekündigt](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/) können Kunden, die Microsoft Operations Management Suite E1 und E2 erwerben, Application Insights Enterprise ohne weitere Kosten als zusätzliche Komponente erhalten. Jede Einheit der Operations Management Suite-Pläne E1 und E2 umfasst eine Berechtigung für einen Knoten des Enterprise-Tarifs von Application Insights. Wie bereits zuvor erwähnt, ermöglicht jeder Application Insights-Knoten das Erfassen von bis zu 200 MB Daten pro Tag (getrennt von der Log Analytics-Datenerfassung) bei einer Datenbeibehaltungsdauer von 90 Tagen ohne zusätzliche Kosten. 

> [!NOTE]
> Um sicherzustellen, dass Sie diese Berechtigung erhalten, muss für Ihre Application Insights-Ressourcen der Enterprise-Tarif gelten. Diese Berechtigung gilt nur für Knoten, sodass es für Application Insights-Ressourcen im Tarif Basic keine Vorteile gibt. Beachten Sie, dass diese Berechtigung nicht in den geschätzten Kosten ausgewiesen ist, die auf dem Blatt „Features + Preise“ gezeigt werden. 
>
 
## <a name="review-pricing-plans-and-estimate-costs"></a>Überprüfen von Tarifen und Schätzen von Kosten

Application Insights erleichtert das Verständnis der verfügbaren Tarife und der Kosten, die basierend auf den aktuellen Nutzungsmustern voraussichtlich anfallen. Öffnen Sie im Azure-Portal in der Application Insights-Ressource für Ihre Anwendung das Blatt **Features + Preise**:

![Wählen Sie „Preise“ aus.](./media/app-insights-pricing/01-pricing.png)

**a.** Überprüfen Sie Ihr Datenvolumen für den Monat. Dies schließt alle nach einer beliebigen [Stichprobennahme](app-insights-sampling.md) in Ihren Server- und Client-Apps sowie Verfügbarkeitstests empfangenen und beibehaltenen Daten ein.

**b.** Für [Webtests mit mehreren Schritten](app-insights-monitor-web-app-availability.md#multi-step-web-tests) wird eine gesonderte Gebühr erhoben. (Darin nicht enthalten sind einfache Verfügbarkeitstests, die von der Gebühr für das Datenvolumen abgedeckt sind.)

**c.** Aktivieren Sie den Enterprise-Tarif.

**d.** Navigieren Sie zu den Datenverwaltungsoptionen, um das Datenvolumen für den Vormonat anzuzeigen, eine tägliche Obergrenze festzulegen oder erfasste Stichproben festzulegen.

Die Gebühren für Application Insights werden Ihrer Azure-Rechnung hinzugefügt. Sie sehen die Details zu Ihrer Azure-Rechnung im Bereich "Abrechnung" des Azure-Portals oder im [Azure-Abrechnungsportal](https://account.windowsazure.com/Subscriptions). 

![Wählen Sie im seitlichen Menü die Option „Abrechnung“.](./media/app-insights-pricing/02-billing.png)

## <a name="data-rate"></a>Datenrate
Es gibt drei Möglichkeiten zum Begrenzen des gesendeten Datenvolumens:

* **Stichprobenerstellung**: Dieser Mechanismus kann die Menge der von Ihrem Server und Ihren Client-Apps gesendeten Telemetriedaten bei minimaler Verzerrung von Metriken verringern. Dies ist Ihr wichtigstes Tool zum Optimieren der Menge der Daten. Erfahren Sie mehr über die [Merkmale von Stichproben](app-insights-sampling.md). 
* **Tägliche Obergrenze**: Beim Erstellen einer Application Insights-Ressource über das Azure-Portal wird dieser Wert auf 500 GB pro Tag festgelegt. Die Standardeinstellung beim Erstellen einer Application Insights-Ressource in Visual Studio ist niedrig (nur 32,3 MB pro Tag) und nur zum Vereinfachen von Tests gedacht. In diesem Fall ist es vorgesehen, dass der Benutzer vor dem Bereitstellen der App in der Produktion die tägliche Obergrenze erhöht. Die Obergrenze ist 500 GB pro Tag, es sei denn, Sie haben einen höheren Maximalwert für eine Anwendung mit hohem Datenverkehr angefordert. Seien Sie beim Festlegen der täglichen Obergrenze aufmerksam, da es Ihre Absicht sein muss, **die tägliche Obergrenze niemals zu erreichen**, weil dann für den Rest des Tages Daten verloren gehen und Sie nicht in der Lage sein werden, Ihre Anwendung zu überwachen. Diese Einstellung können Sie auf dem Blatt „Volumenbegrenzung pro Tag“ ändern, das über das Blatt „Datenverwaltung“ geöffnet wird (siehe unten). Beachten Sie, dass einige Abonnementtypen Guthaben aufweisen, das nicht für Application Insights verwendet werden kann. Wenn für das Abonnement ein Ausgabenlimit gilt, enthält das Blatt mit der täglichen Obergrenze Anweisungen, wie es aufzuheben ist und wie die tägliche Obergrenze über 32,3 MB pro Tag erhöht werden kann.  
* **Drosselung**: Hiermit wird die Datenrate auf 32.000 Ereignisse pro Sekunde ein (gemittelt über 1 Minute) eingeschränkt. 


*Was geschieht, wenn meine App die Drosselungsrate überschreitet?*

* Die Datenmenge, die Ihre App sendet, wird minütlich gemessen. Wenn sie die pro Sekunde, über eine Minute gemittelt Datenmenge überschreitet, lehnt der Server einige Anforderungen ab. Das SDK puffert die Daten und versucht dann, erneut zu senden, wobei ein rascher Anstieg über mehrere Minuten hinweg verteilt wird. Wenn Ihre App die Drosselungsrate beim Senden von Daten laufend überschreitet, werden einige Daten gelöscht. (Die ASP.NET-, Java- und JavaScript-SDKs versuchen auf diese Weise erneut zu senden, andere SDKs löschen gedrosselte Daten möglicherweise einfach.) Tritt eine Drosselung auf, erhalten Sie zur Warnung eine Benachrichtigung über diesen Vorgang.

*Woher weiß ich, wie viele Daten meine App sendet?*

* Öffnen Sie das Blatt **Datenvolumenverwaltung**, um das Diagramm des täglichen Datenvolumens anzuzeigen. 
* Oder fügen Sie im Metrik-Explorer ein neues Diagramm hinzu, und wählen Sie **Datenpunktvolumen** als Metrik aus. Aktivieren Sie "Gruppierung", und gruppieren Sie nach **Datentyp**.

## <a name="to-reduce-your-data-rate"></a>So verringern Sie die Datenrate
Hier sind einige Schritte, die Sie ausführen können, um Ihr Datenvolumen zu reduzieren:

* Verwenden Sie [Stichproben](app-insights-sampling.md). Diese Technologie verringert die Datenrate, ohne die Metriken zu verzerren und ohne die Navigation zwischen verwandten Elementen bei der Suche zu stören. In Server-Apps arbeitet sie automatisch.
* [Begrenzen Sie die Anzahl der gemeldeten AJAX-Aufrufe](app-insights-javascript.md#detailed-configuration) für jeden Seitenaufruf, oder deaktivieren Sie AJAX-Berichte.
* Deaktivieren Sie nicht benötigte Erfassungsmodule durch [Bearbeiten von „ApplicationInsights.config“](app-insights-configuration-with-applicationinsights-config.md). Das kann z. B. für Leistungsindikator- oder Abhängigkeitsdaten gelten.
* Teilen Sie Ihre Telemetrie auf getrennte Instrumentierungsschlüssel auf. 
* Aggregieren Sie Metriken vorab. Wenn Sie Ihrer App Aufrufe an TrackMetric eingefügt haben, können Sie Datenverkehr reduzieren, indem Sie die Überladung verwenden, die Ihre Berechnung des Durchschnitts und die Standardabweichung eines Batches von Messungen akzeptiert. Oder Sie können ein [vorab aggregierendes Paket](https://www.myget.org/gallery/applicationinsights-sdk-labs)verwenden.

## <a name="managing-the-maximum-daily-data-volume"></a>Verwalten des maximalen täglichen Datenvolumens

Sie können die gesammelten Daten mithilfe der täglichen Volumenobergrenze begrenzen. Jedoch gehen alle von der Anwendung gesendeten Telemetriedaten für den Rest des Tags verloren, wenn die Obergrenze überschritten wurde. Es ist **nicht ratsam**, die Anwendung die tägliche Obergrenze erreichen zu lassen, da Sie nach dem Überschreiten der Obergrenze die Integrität und Leistung der Anwendung nicht mehr nachverfolgen können. 

Verwenden Sie stattdessen [Stichproben](app-insights-sampling.md), um das gewünschte Datenvolumen anzupassen, und verwenden Sie die tägliche Obergrenze nur als letzten Ausweg für den Fall, dass die Anwendung unerwartet ein weitaus höheres Volumen an Telemetriedaten sendet. 

Um die tägliche Obergrenze zu ändern, klicken Sie im Abschnitt „Konfigurieren“ Ihrer Application Insights-Ressource erst auf **Datenvolumenverwaltung** und dann auf **Tägliche Obergrenze**.

![Anpassen der Volumenobergrenze für Telemetriedaten pro Tag](./media/app-insights-pricing/daily-cap.png) 

## <a name="sampling"></a>Stichproben
Die [Stichprobenerstellung](app-insights-sampling.md) ist eine Methode, die Rate, mit der Telemetriedaten an Ihre App gesendet werden, zu verringern. Gleichzeitig soll die Möglichkeit erhalten bleiben, bei Diagnosesuchläufen relevante Ereignisse zu ermitteln und korrekte Ereigniszahlen zu erhalten. 

Die Stichprobenerstellung ist eine effektive Möglichkeit, die Gebühren zu senken und innerhalb Ihres monatlichen Kontingents zu bleiben. Der Stichprobenalgorithmus behält Elemente in Bezug auf die Telemetrie bei, sodass Sie beispielsweise in Search die Anforderung ermitteln können, die in Beziehung zu einer bestimmten Ausnahme steht. Der Algorithmus behält außerdem korrekte Zahlenwerte bei, sodass Sie im Metrik-Explorer die richtigen Werte für Anforderungsraten, Ausnahmeraten und weitere Messwerte sehen.

Es gibt verschiedene Formen der Stichprobenerstellung.

* [Adaptive sampling](app-insights-sampling.md) verwendet, bei der die Menge der an Ihre App gesendeten Telemetriedaten automatisch angepasst wird. Die Stichprobenerstellung erfolgt im SDK Ihrer Web-App automatisch, sodass der Telemetriedatenverkehr im Netzwerk verringert wird. 
* *Erfassungs-Stichprobenerstellung* stellt eine Alternative dar, die an dem Punkt arbeitet, an dem Telemetriedaten von Ihrer App den Application Insights-Dienst erreichen. Diese Art der Stichprobenerstellung wirkt sich nicht auf die Menge der Telemetriedaten aus, die von Ihrer App gesendet werden, verringert jedoch die Menge der Daten, die vom Dienst beibehalten werden. Damit können Sie das Kontingent verringern, das von Telemetriedaten von Browsern und anderen SDKs beansprucht wird.

Konfigurieren Sie zum Festlegen der Erfassungs-Stichprobenerstellung die Einstellung auf dem Blatt „Preise“:

![Klicken Sie im Blatt „Kontingent und Preise“ auf die Kachel „Stichproben“, und wählen Sie eine Einheit für die Stichprobenerstellung.](./media/app-insights-pricing/04.png)

> [!WARNING]
> Über das Blatt „Datensampling“ wird nur der Wert für die Erfassung von Stichproben gesteuert. Die Samplingrate, die vom Application Insights SDK in Ihrer App angewendet wird, wird nicht widergespiegelt. Falls für die eingehenden Telemetriedaten bereits im SDK Stichproben erstellt wurden, wird die Erfassungs-Stichprobenerstellung nicht angewendet.
> 

Verwenden Sie etwa folgende [Analytics-Abfrage](app-insights-analytics.md) , um den tatsächlichen Stichproben-Prozentsatz unabhängig davon zu ermitteln, wo er angewendet wird:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h) 
    | render areachart 

In jedem beibehaltenen Datensatz gibt `itemCount` die Anzahl ursprünglicher Datensätze an, die der Datensatz darstellt (Anzahl zuvor verworfener Datensätze + 1). 


## <a name="automation"></a>Automation

Sie können mit der Azure-Ressourcenverwaltung ein Skript schreiben, um den Tarif festzulegen. [Weitere Informationen](app-insights-powershell.md#price).

## <a name="limits-summary"></a>Zusammenfassung der Grenzwerte
[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Stichproben](app-insights-sampling.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: app-insights-overview.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/


