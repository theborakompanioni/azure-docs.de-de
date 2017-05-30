---
title: "Überwachen der Nutzung und Statistiken in einem Azure Search-Dienst | Microsoft Docs"
description: "Verfolgen Sie die Ressourcennutzung und Indexgröße für Azure Search nach, einem in Microsoft Azure gehosteten Cloudsuchdienst."
services: search
documentationcenter: 
author: bernitorres
manager: jlembicz
editor: 
tags: azure-portal
ms.assetid: 122948de-d29a-426e-88b4-58cbcee4bc23
ms.service: search
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 05/01/2017
ms.author: betorres
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 16cb5a1e16a59200f0e731622398efcf24c3f777
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="monitoring-an-azure-search-service"></a>Überwachen eines Diensts von Azure Search

Azure Search bietet verschiedene Ressourcen zum Nachverfolgen der Verwendung und Leistung von Suchdiensten. Sie haben Zugriff auf Metriken, Protokolle, Indexstatistiken und erweiterte Überwachungsfunktionen von Power BI. Dieser Artikel beschreibt das Aktivieren der verschiedenen Überwachungsstrategien und das Interpretieren der resultierenden Daten.

## <a name="azure-search-metrics"></a>Azure Search-Metriken
Mit Metriken haben Sie nahezu einen Echtzeiteinblick in Ihren Suchdienst, und sie sind ohne zusätzliche Installation für jeden Dienst verfügbar. Damit können Sie die Leistung des Diensts bis zu 30 Tage nachverfolgen.

Azure Search sammelt Daten für drei verschiedene Metriken:

* Wartezeit bei Suchvorgängen: Die Zeit, die der Suchdienst für die Verarbeitung von Suchabfragen benötigt hat, auf Minutenbasis aggregiert.
* Suchabfragen pro Sekunde (Queries Per Second, QPS): Anzahl der pro Sekunde empfangenen Suchabfragen, auf Minutenbasis aggregiert.
* Prozentsatz gedrosselter Suchabfragen: Prozentsatz der gedrosselten Suchabfragen, auf Minutenbasis aggregiert.

![Screenshot der QPS-Aktivität][1]

### <a name="set-up-alerts"></a>Einrichten von Warnungen
Auf der Metrikdetailseite können Sie Warnungen konfigurieren, um eine E-Mail-Benachrichtigung oder automatisierte Aktion auszulösen, wenn eine Metrik einen Schwellenwert passiert, den Sie definiert haben.

Weitere Informationen zu Metriken finden Sie in der vollständigen Dokumentation zu Azure-Monitor.  

## <a name="how-to-track-resource-usage"></a>Nachverfolgen der Ressourcenverwendung
Durch Nachverfolgen des Wachstums von Indizes und Dokumentgröße können Sie die Kapazität proaktiv anpassen, bevor die Obergrenze, die Sie für den Dienst festgelegt haben, erreicht wird. Dies ist im Portal oder programmgesteuert mithilfe der REST-API möglich.

### <a name="using-the-portal"></a>Verwenden des Portals

Zum Überwachen der Ressourcennutzung zeigen Sie die Anzahl und die Statistik für den Dienst im [Portal](https://portal.azure.com) an.

1. Melden Sie sich beim [Portal](https://portal.azure.com) an.
2. Öffnen Sie das Service-Dashboard des Azure-Suchdiensts. Die Startseite enthält Kacheln für den Dienst, oder Sie können auch über „Durchsuchen“ auf der Indexleiste zum Dienst navigieren.

Der Abschnitt „Verwendung“ umfasst einen Verbrauchszähler, aus dem hervorgeht, welcher Teil des verfügbaren Ressourcen zurzeit verwendet wird. Informationen zu Einschränkungen für Indizes, Dokumente und Speicher pro Dienst finden Sie unter [Diensteinschränkungen](search-limits-quotas-capacity.md).

  ![Kachel „Nutzung“][2]

> [!NOTE]
> Der oben dargestellte Screenshot bezieht sich auf den kostenlosen Dienst, der maximal je ein Replikat und eine Partition aufweist und nur drei Indizes, 10.000 Dokumente oder 50 MB an Daten (je nachdem, was zuerst eintritt) hosten kann. Dienste, die mit einem Basic- oder Standard-Tarif erstellt werden, weisen höhere Werte für die Diensteinschränkungen auf. Weitere Informationen zum Auswählen eines Tarifs finden Sie unter [Auswählen einer SKU oder eines Tarifs für Azure Search](search-sku-tier.md).
>
>

### <a name="using-the-rest-api"></a>Verwenden der REST-API
Sowohl die REST-API von Azure Search als auch das .NET SDK bieten programmgesteuerten Zugriff auf Dienstmetriken.  Bei Verwendung von [Indexern](https://msdn.microsoft.com/library/azure/dn946891.aspx) zum Laden eines Index aus Azure SQL-Datenbank oder Azure Cosmos DB steht eine zusätzliche API zum Abrufen der benötigen Zahlen zur Verfügung.

* [Abrufen von Indexstatistiken](/rest/api/searchservice/get-index-statistics)
* [Dokumentenanzahl](/rest/api/searchservice/count-documents)
* [Abrufen des Indexerstatus](/rest/api/searchservice/get-indexer-status)

## <a name="how-to-export-logs-and-metrics"></a>Exportieren von Protokollen und Metriken

Sie können die Vorgangsprotokolle für den Dienst und die unformatierten Daten für die im vorherigen Abschnitt beschriebenen Metriken exportieren. Durch Vorgangsprotokolle erfahren Sie, wie der Dienst verwendet wird und von Power BI genutzt werden kann, wenn Daten in ein Speicherkonto kopiert werden. Azure Search bietet zu diesem Zweck ein Power BI-Inhaltspaket zur Überwachung.


### <a name="enabling-monitoring"></a>Aktivieren der Überwachung
Öffnen Sie Ihren Dienst von Azure Search im [Azure-Portal](http://portal.azure.com) unter der Option „Überwachung aktivieren“.

Wählen Sie die Daten, die Sie exportieren möchten: Protokolle, Metriken oder beides. Sie können sie in ein Speicherkonto kopieren, an einen Event Hub senden oder nach Log Analytics exportieren.

![Aktivieren der Überwachung im Portal][3]

Informationen zur Aktivierung mit PowerShell oder Azure CLI finden Sie in [dieser](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs#how-to-enable-collection-of-diagnostic-logs) Dokumentation.

### <a name="logs-and-metrics-schemas"></a>Protokolle und Metrikenschemata
Beim Kopieren in ein Speicherkonto werden die Daten im JSON-Format formatiert und in zwei Containern platziert:

* „insights-logs-operationlogs“: für Suchdatenverkehrsprotokolle
* „insights-metrics-pt1m“: für Metriken

Es gibt einen Blob pro Stunde pro Container.

Beispiel-Pfad: `resourceId=/subscriptions/<subscriptionID>/resourcegroups/<resourceGroupName>/providers/microsoft.search/searchservices/<searchServiceName>/y=2015/m=12/d=25/h=01/m=00/name=PT1H.json`

#### <a name="log-schema"></a>Protokollschema
Die Protokollblobs enthalten die Datenverkehrprotokolle des Suchdiensts.
Jedes Blob hat ein Stammobjekt namens **records** , das ein Array mit Protokollobjekten enthält.
Jedes Blob enthält Einträge zu allen Vorgängen, die während derselben Stunde erfolgt sind.

| Name | Typ | Beispiel | Hinweise |
| --- | --- | --- | --- |
| in |datetime |„2015-12-07T00:00:43.6872559Z“ |Zeitstempel des Vorgangs |
| resourceId |string |"/SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111/<br/>RESOURCEGROUPS/DEFAULT/PROVIDERS/<br/> MICROSOFT.SEARCH/SEARCHSERVICES/SEARCHSERVICE" |Ihre Ressourcen-ID |
| operationName |string |„Query.Search“ |Der Name des Vorgangs |
| operationVersion |string |„2015-02-28“ |Die verwendete API-Version |
| category |string |„OperationLogs“ |Konstante |
| resultType |string |„Success“ |Mögliche Werte: „Success“ oder „Failure“ |
| resultSignature |int |200 |HTTP-Ergebniscode |
| durationMS |int |50 |Dauer des Vorgangs in Millisekunden |
| Eigenschaften |objekt |Siehe hierzu die folgende Tabelle. |Objekt, das vorgangsspezifische Daten enthält |

**Eigenschaftsschema**
| Name | Typ | Beispiel | Hinweise |
| --- | --- | --- | --- |
| Beschreibung |string |„GET-/indexes('content')/docs“ |Endpunkt des Vorgangs |
| Abfrage |string |„?search=AzureSearch&$count=true&api-version=2015-02-28“ |Die Abfrageparameter |
| Dokumente |int |42 |Anzahl von verarbeiteten Dokumenten |
| IndexName |string |„testindex“ |Name des Indexes, der dem Vorgang zugeordnet ist |

#### <a name="metrics-schema"></a>Metrikenschema
| Name | Typ | Beispiel | Hinweise |
| --- | --- | --- | --- |
| resourceId |string |"/SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111/<br/>RESOURCEGROUPS/DEFAULT/PROVIDERS/<br/>MICROSOFT.SEARCH/SEARCHSERVICES/SEARCHSERVICE" |Ihre Ressourcen-ID |
| metricName |string |„Latency“ |Der Name der Metrik |
| in |datetime |„2015-12-07T00:00:43.6872559Z“ |Der Zeitstempel des Vorgangs |
| average |int |64 |Der Durchschnittswert der unformatierten Beispiele im Metrikzeitintervall |
| minimum |int |37 |Der Mindestwert der unformatierten Beispiele im Metrikzeitintervall |
| maximum |int |78 |Der Höchstwert der unformatierten Beispiele im Metrikzeitintervall |
| total |int |258 |Der Gesamtwert der unformatierten Beispiele im Metrikzeitintervall |
| count |int |4 |Die Anzahl der unformatierten Beispiele, die zum Generieren der Metrik verwendet werden |
| timegrain |string |„PT1M“ |Das Aggregationsintervall der Metrik in ISO 8601 |

Alle Metriken werden in Intervallen von einer Minute gemeldet. Jede Metrik macht Mindest-, Höchst- und Durchschnittswerte pro Minute verfügbar.

Bei der Metrik „SearchQueriesPerSecond“ ist der Mindestwert der niedrigste Wert für Suchabfragen pro Sekunde, der während dieser Minute registriert wurde. Dasselbe gilt für den Höchstwert. Der Durchschnittswert ist das Aggregat der gesamten Minute.
Beispiel: Innerhalb einer Minute kann es für eine Sekunde eine sehr hohe Last geben (dies ist der Höchstwert für „SearchQueriesPerSecond“), gefolgt von 58 Sekunden mit mittlerer Last sowie einer Sekunde mit nur einer Abfrage, was der Mindestwert ist.

Für „ThrottledSearchQueriesPercentage“ entsprechen der Mindest-, Höchst-, Durchschnitts- und Gesamtwert demselben Wert, nämlich dem Prozentsatz von Suchabfragen, die gedrosselt wurden, basierend auf der Gesamtanzahl von Suchabfragen während einer Minute.

## <a name="analyzing-your-data-with-power-bi"></a>Analysieren Ihrer Daten mit Power BI

Zum Untersuchen und Visualisieren Ihrer Daten sollten Sie [Power BI](https://powerbi.microsoft.com) verwenden. Sie können ganz einfach eine Verbindung mit Ihrem Azure Storage-Konto herstellen und schnell mit der Analyse Ihrer Daten beginnen.

Azure Search bietet ein [Power BI-Inhaltspaket](https://app.powerbi.com/getdata/services/azure-search), dass Ihnen ermöglicht, mit vordefinierten Diagrammen und Tabellen Ihren Suchdatenverkehr zu überwachen und zu verstehen. Es enthält einen Satz von Power BI-Berichten, die automatisch eine Verbindung mit Ihren Daten herstellen und Ihnen visuell Erkenntnisse über Ihren Suchdienst vermitteln. Weitere Informationen finden Sie auf der [Hilfeseite zum Inhaltspaket](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-search/).

![Power BI-Dashboard für Azure Search][4]

## <a name="next-steps"></a>Nächste Schritte
Unter [Grenzwerte für den Azure Search-Dienst](search-limits-quotas-capacity.md) finden Sie Anleitungen dazu, wie Sie die Zuweisung von Partitionen und Replikaten für einen vorhandenen Dienst ausgleichen können.

Besuchen Sie [Verwalten Ihres Suchdiensts in Microsoft Azure](search-manage.md), um weitere Informationen zur Dienstverwaltung zu erhalten, oder [Überlegungen zur Leistung und Optimierung von Azure Search](search-performance-optimization.md) für eine Optimierungsanleitung.

Erfahren Sie hier mehr über das Erstellen erstaunlicher Berichte. Weitere Informationen finden Sie unter [Erste Schritte mit Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/).

<!--Image references-->
[1]: ./media/search-monitor-usage/AzSearch-Monitor-BarChart.PNG
[2]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG
[3]: ./media/search-monitor-usage/AzureSearch-Enable-Monitoring.PNG
[4]: ./media/search-monitor-usage/AzureSearch-PowerBI-Dashboard.png

