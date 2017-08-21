---
title: "Überblick über Metriken in Microsoft Azure | Microsoft Docs"
description: "Erfahren Sie, wie Sie Überwachungsdiagramme in Azure anpassen."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c36031eb-4df5-4cd5-9479-311d493a40d2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: robb
ms.translationtype: HT
ms.sourcegitcommit: cddb80997d29267db6873373e0a8609d54dd1576
ms.openlocfilehash: 3f9ebb0f5737714dd685f0dcc1ff4b1c0c89528f
ms.contentlocale: de-de
ms.lasthandoff: 07/18/2017

---
# <a name="overview-of-metrics-in-microsoft-azure"></a>Überblick über Metriken in Microsoft Azure
Alle Azure-Dienste verfolgen wichtige Metriken, die Ihnen das Überwachen der Integrität, Leistung, Verfügbarkeit und Nutzung Ihrer Dienste ermöglichen. Sie können diese Metriken im Azure-Portal anzeigen, und Sie können darüber hinaus mithilfe von [REST-API](https://msdn.microsoft.com/library/azure/dn931930.aspx) oder [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) programmgesteuert auf sämtliche Metriken zugreifen.

Für einige Dienste müssen Sie möglicherweise die Diagnose aktivieren, um Metriken anzeigen zu können. Für andere Dienste, z. B. virtuelle Computer, wird ein grundlegender Satz an Metriken bereitgestellt, die Hochfrequenzmetriken müssen jedoch aktiviert werden. Weitere Informationen finden Sie unter [Aktivieren von Überwachung und Diagnose](insights-how-to-use-diagnostics.md).

## <a name="using-monitoring-charts"></a>Verwenden von Überwachungsdiagrammen
Sie können für beliebige Metriken und einen gewählten Zeitraum ein Diagramm erstellen.

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/)auf **Durchsuchen**und anschließend auf eine Ressource, die Sie überwachen möchten.
2. Der Bereich **Überwachen** enthält die wichtigsten Metriken zu jeder Azure-Ressource. Für eine Web-App ist dies beispielsweise **Anforderungen und Fehler**, während für einen virtuellen Computer **CPU-Prozentsatz** und **Lese- und Schreibvorgänge auf dem Datenträger** angezeigt wird: ![Überwachungsfokus](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)
3. Wenn Sie auf eines dieser Diagramme klicken, wird das Blatt **Metrik** geöffnet. Auf diesem Blatt wird neben dem Diagramm eine Tabelle angezeigt, die Aggregationen der Metriken enthält (z. B. Durchschnitt, Mindest- und Maximalwert für den gewählten Zeitraum). Darunter werden die Warnregeln für die Ressource angezeigt.
    ![Blatt „Metrik“](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)
4. Zum Anpassen der angezeigten Zeilen klicken Sie auf die Schaltfläche **Bearbeiten** im Diagramm, oder klicken Sie auf den Befehl **Bearbeiten** auf dem Blatt „Metrik“.
5. Auf dem Blatt "Abfrage bearbeiten" haben Sie drei Möglichkeiten:
   
   * Ändern des Zeitraums
   * Wechseln zwischen der Darstellung "Balken" und "Linie"
   * Auswählen anderer Metriken, ![Abfrage bearbeiten](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)
6. Die Änderung des Zeitbereichs ist denkbar einfach: Sie wählen einen anderen Bereich aus (beispielsweise **Letzte Stunde**) und klicken unten auf dem Blatt auf **Speichern**. Mit der Einstellung **Benutzerdefiniert**können Sie einen beliebigen Zeitraum in den letzten zwei Wochen auswählen. Beispielsweise können Sie die gesamten letzten zwei Wochen oder lediglich eine Stunde gestern anzeigen. Geben Sie in das Textfeld eine andere Uhrzeit ein.
    ![Benutzerdefinierter Zeitbereich](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)
7. Unterhalb des Zeitbereichs können Sie beliebig viele Metriken auswählen, die in dem Diagramm angezeigt werden sollen.
8. Wenn Sie auf "Speichern" klicken, werden Ihre Änderungen für die jeweilige Ressource gespeichert. Wenn Sie beispielsweise über zwei virtuelle Computer verfügen und ein Diagramm für einen der virtuellen Computer ändern, hat dies keine Auswirkungen auf den zweiten virtuellen Computer.

## <a name="creating-side-by-side-charts"></a>Erstellen nebeneinander angezeigter Diagramme
Dank der umfangreichen Anpassungsmöglichkeiten im Portal können Sie so viele Diagramme hinzufügen, wie Sie möchten.

1. Klicken Sie im oberen Bereich des Blatts im Menü **...** auf **Kacheln hinzufügen**:  
    ![Menü „Hinzufügen“](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. Anschließend können Sie ein Diagramm aus dem **Katalog** auf der rechten Bildschirmseite auswählen: ![Katalog](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. Wenn die gewünschte Metrik nicht angezeigt wird, können Sie einfach eine der voreingestellten Metriken hinzufügen und durch **Bearbeiten** des Diagramms die Metrik anzeigen, die Sie benötigen.

## <a name="monitoring-usage-quotas"></a>Überwachen mithilfe von Kontingenten
Die meisten Metriken zeigen Trends im Zeitverlauf an, für bestimmte Daten – beispielsweise für Nutzungskontingente – werden jedoch Informationen zu einem bestimmten Zeitpunkt mit einem Schwellenwert angezeigt.

Für Ressourcen mit Kontingenten können Sie Nutzungskontingente auch auf dem Blatt für Ressourcen anzeigen:

![Verwendung](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

Wie bei den Metriken können Sie mithilfe von [REST-API](https://msdn.microsoft.com/library/azure/dn931963.aspx) oder [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) programmgesteuert auf den vollständigen Satz an Nutzungskontingenten zugreifen.

## <a name="next-steps"></a>Nächste Schritte
* [Empfangen von Warnbenachrichtigungen](insights-receive-alert-notifications.md) , wenn eine Metrik einen Schwellenwert überschreitet.
* [Aktivieren von Überwachung und Diagnose](insights-how-to-use-diagnostics.md) , um detaillierte Hochfrequenzmetriken zu Ihrem Dienst zu sammeln.
* [Automatisches Skalieren der Instanzenanzahl](insights-how-to-scale.md) , um sicherzustellen, dass Ihr Dienst verfügbar und reaktionsfähig ist.
* [Überwachen der Anwendungsleistung](../application-insights/app-insights-azure-web-apps.md) , um präzise Informationen zur Leistung Ihres Codes in der Cloud zu ermitteln.
* Verwenden von [Application Insights für JavaScript-Apps und Webseiten](../application-insights/app-insights-web-track-usage.md) , um eine Clientanalyse über die Browser zu erhalten, mit denen auf eine Webseite zugegriffen wird.
* [Überwachen der Verfügbarkeit und Reaktionsfähigkeit einer beliebigen Webseite](../application-insights/app-insights-monitor-web-app-availability.md) mit Application Insights, um zu ermitteln, ob eine Seite offline ist.


