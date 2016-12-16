---
title: "Überwachen der Nutzung und Statistiken in einem Azure Search-Dienst | Microsoft Docs"
description: "Verfolgen Sie die Ressourcennutzung und Indexgröße für Azure Search nach, einem in Microsoft Azure gehosteten Cloudsuchdienst."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 122948de-d29a-426e-88b4-58cbcee4bc23
ms.service: search
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 10/29/2016
ms.author: heidist
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 6e40acb01d6297aa66090859533fd48c108a9a88


---
# <a name="monitor-usage-and-query-metrics-in-an-azure-search-service"></a>Überwachen der Nutzung und Abfragemetriken in einem Azure Search-Dienst
Azure Search erfasst Statistiken zur Ausführung von Abfragen auf Dienstebene, einschließlich der Abfragen pro Sekunde (Queries per Second, QPS), der Latenz und des Prozentsatzes von gelöschten Abfragen, wenn das Volumen die Kapazität überschreitet. Metriken werden im Portal auf dem Blatt „Überwachung“ angezeigt.

   ![Screenshot der QPS-Aktivität][5]

Dieser Artikel beschreibt die Metriken für den Dienst. Einblicke in die Aktivität auf Indexebene erhalten Sie, indem Sie die Analyse des Suchdatenverkehrs aktivieren und mit Power BI die Analyse anzeigen. Die ersten Schritte finden Sie unter [„Datenverkehrsanalyse durchsuchen“ für Azure Search](search-traffic-analytics.md) .

## <a name="view-query-throughput-statistics"></a>Anzeigen der Statistiken zum Abfragedurchsatz
Klicken Sie auf dem Dienstdashboard auf die Kachel „Überwachung“, um das Blatt „Überwachung“ zu öffnen.

   ![Kachel „Überwachung“][2]

Abfrageaktivitäten auf Dienstebene werden für die Ausführung von Abfragen, die Latenz und die Drosselung aufgezeichnet. Metriken werden kontinuierlich gesammelt, jedoch kann es einige Minuten dauern, bis das Portal die letzte Aktivität anzeigt. 

Klicken Sie auf die Kachel einer Metrik, um Details anzuzeigen, Warnungen hinzufügen, die Diagnose zu aktivieren oder das Diagramm zu bearbeiten.

  ![Befehle auf dem Blatt „Metrik“][4]

### <a name="set-up-alerts"></a>Einrichten von Warnungen
Auf der Detailseite der Metrik können Sie Warnungen konfigurieren, damit eine E-Mail-Benachrichtigung ausgelöst wird, wenn die Aktivität im Hinblick auf die Ausführung von Abfragen, die Latenz oder die Drosselung definierte Kriterien überschreitet.

### <a name="enable-diagnostics"></a>Aktivieren der Diagnosefunktion
Wenn Sie die Diagnose aktivieren, können Sie konfigurieren, wo Diagnosedaten gespeichert werden, ob Vorgangsprotokolle und Metriken eingeschlossen werden und wie lange die Daten beibehalten werden sollen.

### <a name="change-chart-type-and-data-collection-interval"></a>Ändern des Diagrammtyps und des Datenerfassungsintervalls
Sie können für jede Metrik auf **Bearbeiten** klicken, um die Visualisierung von einem Liniendiagramm in ein Balkendiagramm zu ändern oder um die X-Achse auf einen anderen Zeitbereich festzulegen.

  ![Konfiguration des Zeitbereichs][3]

## <a name="view-counts-and-resource-usage-in-the-portal"></a>Anzeigen von Anzahl und Ressourcennutzung im Portal
Durch Nachverfolgen des Wachstums von Indizes und Dokumentgröße können Sie die Kapazität proaktiv anpassen, bevor die Obergrenze, die Sie für den Dienst festgelegt haben, erreicht wird. 

Zum Überwachen der Ressourcennutzung zeigen Sie die Anzahl und die Statistik für den Dienst im [Portal](https://portal.azure.com) an. Sie können die Informationen auch programmgesteuert abrufen, wenn Sie ein benutzerdefiniertes Dienstverwaltungstool erstellen.

1. Melden Sie sich beim [Portal](https://portal.azure.com) an. 
2. Öffnen Sie das Service-Dashboard des Azure-Suchdiensts. Die Startseite enthält Kacheln für den Dienst, oder Sie können auch über „Durchsuchen“ auf der Indexleiste zum Dienst navigieren. 

Der Abschnitt „Verwendung“ umfasst einen Verbrauchszähler, aus dem hervorgeht, welcher Teil des verfügbaren Ressourcen zurzeit verwendet wird. Informationen zu Einschränkungen für Indizes, Dokumente und Speicher pro Dienst finden Sie unter [Diensteinschränkungen](search-limits-quotas-capacity.md).

  ![Kachel „Nutzung“][1]

> [!NOTE]
> Der oben dargestellte Screenshot bezieht sich auf den kostenlosen Dienst, der maximal je ein Replikat und eine Partition aufweist und nur drei Indizes, 10.000 Dokumente oder 50 MB an Daten (je nachdem, was zuerst eintritt) hosten kann. Dienste, die mit einem Basic- oder Standard-Tarif erstellt werden, weisen höhere Werte für die Diensteinschränkungen auf. Weitere Informationen zum Auswählen eines Tarifs finden Sie unter [Auswählen einer SKU oder eines Tarifs für Azure Search](search-sku-tier.md).
> 
> 

### <a name="get-index-statistics-using-the-rest-api"></a>Abrufen von Indexstatistiken mit der REST-API
Sowohl die REST-API von Azure Search als auch das .NET SDK bieten programmgesteuerten Zugriff auf Dienstmetriken.  Bei Verwendung von [Indexern](https://msdn.microsoft.com/library/azure/dn946891.aspx) zum Laden eines Index aus Azure SQL-Datenbank oder DocumentDB steht eine zusätzliche API zum Abrufen der benötigen Zahlen zur Verfügung. 

* [Abrufen von Indexstatistiken](https://msdn.microsoft.com/library/azure/dn798942.aspx)
* [Dokumentenanzahl](https://msdn.microsoft.com/library/azure/dn798924.aspx)
* [Abrufen des Indexerstatus](https://msdn.microsoft.com/library/azure/dn946884.aspx)

## <a name="next-steps"></a>Nächste Schritte
Unter [Grenzwerte für den Azure Search-Dienst](search-limits-quotas-capacity.md) finden Sie Anleitungen dazu, wie Sie die Zuweisung von Partitionen und Replikaten für einen vorhandenen Dienst ausgleichen können. 

Besuchen Sie [Verwalten Ihres Suchdiensts in Microsoft Azure](search-manage.md), um weitere Informationen zur Dienstverwaltung zu erhalten, oder [Überlegungen zur Leistung und Optimierung von Azure Search](search-performance-optimization.md) für eine Optimierungsanleitung.

<!--Image references-->
[1]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG
[2]: ./media/search-monitor-usage/AzSearch-Monitor-Tile.PNG
[3]: ./media/search-monitor-usage/AzSearch-Monitor-Intervals.PNG
[4]: ./media/search-monitor-usage/AzSearch-Monitor-AlertCmd.PNG
[5]: ./media/search-monitor-usage/AzSearch-Monitor-BarChart.PNG








<!--HONumber=Nov16_HO3-->


