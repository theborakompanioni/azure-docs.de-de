---
title: "Anzeigen von Analysedaten für Azure-Web-Apps | Microsoft-Dokumentation"
description: "Mithilfe der Lösung Azure-Web-Apps-Analyse können Sie Erkenntnisse über Ihre Azure-Web-Apps gewinnen, indem Sie verschiedene Metriken über alle Azure-Web-App-Ressourcen sammeln."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 20ff337f-b1a3-4696-9b5a-d39727a94220
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: banders
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: 90d0b7e3f49daa33ab8c617d07ba9098cdda751f
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017

---

# <a name="view-analytic-data-for-metrics-across-all-your-azure-web-app-resources"></a>Anzeigen von Analysedaten für Metriken über alle Azure-Web-App-Ressourcen

![Symbol für Web-Apps](./media/log-analytics-azure-web-apps-analytics/azure-web-apps-analytics-symbol.png)  
Die Lösung Azure-Web-Apps-Analyse (Vorschau) bietet Erkenntnisse über Ihre [Azure-Web-Apps](../app-service-web/app-service-web-overview.md), indem verschiedene Metriken über alle Azure-Web-App-Ressourcen gesammelt werden. Mit der Lösung können Sie Metrikdaten für Web-App-Ressourcen analysieren und nach diesen suchen.

Mit der Lösung können Sie Folgendes anzeigen:

- Web-Apps mit der höchsten Antwortzeit
- Anzahl der Anforderungen für Ihre Web-Apps, einschließlich des Status der Anforderungen (Erfolg oder Fehler)
- Web-Apps mit dem meisten eingehenden und ausgehenden Datenverkehr
- Servicepläne mit hoher CPU- und Arbeitsspeicherauslastung
- Vorgänge im Azure-Web-Apps-Aktivitätsprotokoll

## <a name="connected-sources"></a>Verbundene Quellen

Im Gegensatz zu den meisten anderen Log Analytics-Lösungen werden die Daten für Azure-Web-Apps nicht von Agents erfasst. Alle von der Lösung verwendeten Daten stammen direkt von Azure.

| Verbundene Quelle | Unterstützt | Beschreibung |
| --- | --- | --- |
| [Windows-Agents](log-analytics-windows-agents.md) | Nein | Die Lösung erfasst keine Informationen von Windows-Agents. |
| [Linux-Agents](log-analytics-linux-agents.md) | Nein | Die Lösung erfasst keine Informationen von Linux-Agents. |
| [SCOM-Verwaltungsgruppe](log-analytics-om-agents.md) | Nein | Die Lösung erfasst keine Informationen von Agents in einer verbundenen SCOM-Verwaltungsgruppe. |
| [Azure-Speicherkonto](log-analytics-azure-storage.md) | Nein | Die Lösung sammelt keine Informationen von Azure Storage. |

## <a name="prerequisites"></a>Voraussetzungen

- Für den Zugriff auf die Metrikinformationen von Azure-Web-App-Ressourcen müssen Sie über ein Azure-Abonnement verfügen.

## <a name="configuration"></a>Konfiguration

Führen Sie die folgenden Schritte aus, um die Lösung Azure-Web-Apps-Analyse für Ihre Arbeitsbereiche zu konfigurieren.

1. Aktivieren Sie die Lösung Azure-Web-Apps-Analyse für Aktivitätsprotokolle in [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureWebAppsAnalyticsOMS?tab=Overview) oder entsprechend den unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Schritten.
2. [Aktivieren Sie die Protokollierung von Azure-Ressourcenmetriken in OMS über PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell).

Die Lösung Azure-Web-Apps-Analyse sammelt zwei Gruppen von Metriken aus Azure:

- Azure-Web-Apps-Metriken
  - Durchschnittlicher Arbeitssatz für Arbeitsspeicher
  - Durchschnittliche Antwortzeit
  - Bytes gesendet/empfangen
  - CPU-Zeit
  - Anforderungen
  - Arbeitssatz für Arbeitsspeicher
  - Httpxxx
- App Service-Planmetriken
  - Bytes gesendet/empfangen
  - CPU-Prozentsatz
  - Warteschlangenlänge des Datenträgers
  - Länge der HTTP-Warteschlange
  - Arbeitsspeicherprozentsatz

App Service-Planmetriken werden nur gesammelt, wenn Sie einen dedizierten App Service-Plan verwenden. Dies gilt nicht für kostenlose oder freigegebene App Service-Pläne.

Wenn Sie die Lösung über das OMS-Portal hinzufügen, wird die folgende Kachel angezeigt. Sie müssen [die Protokollierung von Azure-Ressourcenmetriken in OMS über PowerShell aktivieren](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell).

![Ausführen von Bewertungsbenachrichtigungen](./media/log-analytics-azure-web-apps-analytics/performing-assessment.png)

Nach dem Konfigurieren der Lösung sollten innerhalb von 15 Minuten Daten in Ihren Arbeitsbereich übermittelt werden.

## <a name="using-the-solution"></a>Verwenden der Lösung

Wenn Sie die Lösung Azure-Web-Apps-Analyse Ihrem Arbeitsbereich hinzufügen, wird Ihrem Übersichtsdashboard die Kachel **Azure-Web-Apps-Analyse** hinzugefügt. Auf dieser Kachel wird die Anzahl der Azure-Web-Apps angezeigt, auf die die Lösung in Ihrem Abonnement Zugriff hat.

![Kachel für Azure-Web-Apps-Analyse](./media/log-analytics-azure-web-apps-analytics/azure-web-apps-analytics-tile.png)

### <a name="view-azure-web-apps-analytics-information"></a>Anzeigen von Informationen aus Azure-Web-Apps-Analyse

Klicken Sie auf die Kachel **Azure-Web-Apps-Analyse**, um das Dashboard **Azure-Web-Apps-Analyse** zu öffnen. Das Dashboard enthält die Blätter, die in der folgenden Tabelle angegeben sind. Auf jedem Blatt sind bis zu zehn Einträge aufgeführt, die die Kriterien des Blatts für den angegebenen Bereich und Zeitraum erfüllen. Sie können eine Protokollsuche durchführen, mit der alle Einträge zurückgegeben werden, indem Sie unten auf dem Blatt auf **Alle anzeigen** oder auf die Blattüberschrift klicken.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

| Column | Beschreibung |
| --- | --- |
| Azure-Web-Apps |   |
| Anforderungstrends für Web-Apps | Zeigt ein Liniendiagramm des Anforderungstrends für Web-Apps für den ausgewählten Datumsbereich sowie eine Liste der häufigsten zehn Webanforderungen. Klicken Sie auf das Liniendiagramm, um eine Protokollsuche nach <code>Type=AzureMetrics ResourceId=*"/MICROSOFT.WEB/SITES/"* (MetricName=Requests OR MetricName=Http*) &#124; measure avg(Average) by MetricName interval 1HOUR</code> auszuführen. <br>Klicken Sie auf ein Webanforderungselement, um eine Protokollsuche nach dem Webanforderungs-Metriktrend dieser Anforderung auszuführen. |
| Web-Apps-Antwortzeit | Zeigt ein Liniendiagramm der Web-Apps-Antwortzeit für den ausgewählten Datumsbereich. Zeigt außerdem eine Liste der ersten zehn Web-Apps-Antwortzeiten an. Klicken Sie auf das Diagramm, um eine Protokollsuche nach <code>Type:AzureMetrics ResourceId=*"/MICROSOFT.WEB/SITES/"* MetricName="AverageResponseTime" &#124; measure avg(Average) by Resource interval 1HOUR</code> auszuführen.<br> Klicken Sie auf eine Web-App, um eine Protokollsuche auszuführen, bei der die Antwortzeiten für die Web-App zurückgegeben werden. |
| Web-Apps-Datenverkehr | Zeigt ein Liniendiagramm für Web Apps-Datenverkehr in MB an, und listet den Datenverkehr der obersten Web-Apps auf. Klicken Sie auf das Diagramm, um eine Protokollsuche nach <code>Type:AzureMetrics ResourceId=*"/MICROSOFT.WEB/SITES/"*  MetricName=BytesSent OR BytesReceived &#124; measure sum(Average) by Resource interval 1HOUR</code> auszuführen.<br> Dabei werden alle Web-Apps mit Datenverkehr während der letzten Minute angezeigt. Klicken Sie auf eine Web-App, um eine Protokollsuche auszuführen, die die empfangenen und gesendeten Bytes für die Web-App anzeigt. |
| Azure App Service-Pläne |   |
| App Service-Pläne mit CPU-Auslastung &gt; 80 % | Zeigt die Gesamtanzahl der App Service-Pläne an, die eine CPU-Auslastung über 80 % aufweisen, und listet die obersten zehn App Service-Pläne nach CPU-Auslastung auf. Klicken Sie auf den Gesamtbereich, um eine Protokollsuche nach <code>Type=AzureMetrics ResourceId=*"/MICROSOFT.WEB/SERVERFARMS/"* MetricName=CpuPercentage &#124; measure Avg(Average) by Resource</code> auszuführen.<br> Dabei wird eine Liste Ihrer App Service-Pläne mit ihrer durchschnittlichen CPU-Auslastung angezeigt. Klicken Sie auf einen App Service-Plan, um eine Protokollsuche auszuführen, die dessen durchschnittliche CPU-Auslastung anzeigt. |
| App Service-Pläne mit Speicherauslastung &gt; 80 % | Zeigt die Gesamtanzahl der App Service-Pläne an, die eine Speicherauslastung über 80 % aufweisen, und listet die obersten zehn App Service-Pläne nach Speicherauslastung auf. Klicken Sie auf den Gesamtbereich, um eine Protokollsuche nach <code>Type=AzureMetrics ResourceId=*"/MICROSOFT.WEB/SERVERFARMS/"* MetricName=MemoryPercentage &#124; measure Avg(Average) by Resource</code> auszuführen.<br> Dabei wird eine Liste Ihrer App Service-Pläne mit ihrer durchschnittlichen Speicherauslastung angezeigt. Klicken Sie auf einen App Service-Plan, um eine Protokollsuche auszuführen, die dessen durchschnittliche Speicherauslastung anzeigt. |
| Azure-Web-Apps – Aktivitätsprotokolle |   |
| Azure-Web-Apps – Aktivitätsüberwachung | Zeigt die Gesamtanzahl der Web-Apps mit [Aktivitätsprotokollen](log-analytics-activity.md) und listet die Vorgänge in den obersten zehn Aktivitätsprotokollen auf. Klicken Sie auf den Gesamtbereich, um eine Protokollsuche nach <code>Type=AzureActivity ResourceProvider= "Azure Web Sites" &#124; measure count() by OperationName</code> auszuführen.<br> Hierbei wird eine Liste der Vorgänge im Aktivitätsprotokoll angezeigt. Klicken Sie auf einen Vorgang im Aktivitätsprotokoll, um eine Protokollsuche auszuführen, die die Einträge für den Vorgang auflistet. |



### <a name="azure-web-apps"></a>Azure-Web-Apps 

Im Dashboard können Sie sich mithilfe eines Drilldowns einen besseren Einblick in Ihre Web-Apps-Metriken verschaffen. Diese erste Gruppe von Blättern zeigt den Trend der Web-Apps-Anforderungen, die Anzahl von Fehlern (z.B. HTTP 404), den Datenverkehr und die durchschnittliche Antwortzeit über die Zeit an. Außerdem zeigt sie eine Aufschlüsselung dieser Metriken für andere Web-Apps an.

![Azure-Web-Apps – Blätter](./media/log-analytics-azure-web-apps-analytics/web-apps-dash01.png)

Diese Daten werden Ihnen vor allem angezeigt, damit Sie eine Web-App mit hoher Antwortzeit identifizieren und untersuchen können, um die Ursache herauszufinden. Außerdem wird ein Schwellenwert angewendet, damit Sie die problematischen Web-Apps einfacher identifizieren können.

- Rot dargestellte Web-Apps weisen eine Antwortzeit über 1 Sekunde auf.
- Orange dargestellte Web-Apps weisen eine Antwortzeit über 0,7 Sekunden und unter 1 Sekunde auf.
- Grün dargestellte Web-Apps weisen eine Antwortzeit unter 0,7 Sekunden auf.

Im folgenden Beispielbild einer Protokollsuche ist zu erkennen, dass die Web-App *anugup3* eine erheblich höhere Antwortzeit als die anderen Web-Apps aufweist.

![Beispiel: Protokollsuche](./media/log-analytics-azure-web-apps-analytics/web-app-search-example.png)

### <a name="app-service-plans"></a>App Service-Pläne

Wenn Sie dedizierte Servicepläne verwenden, können Sie auch Metriken für Ihre App Service-Pläne sammeln. In dieser Ansicht finden Sie Ihre App Service-Pläne mit hoher CPU- oder Speicherauslastung (&gt; 80 %). Außerdem werden die obersten App-Dienste mit hoher Speicher- oder CPU-Auslastung angezeigt. Wiederum wird ein Schwellenwert angewendet, damit Sie die problematischen Web-Apps einfacher identifizieren können.

- Rot dargestellte App Service-Pläne weisen eine CPU-/Speicherauslastung über 80 % auf.
- Orange dargestellte App Service-Pläne weisen eine CPU-/Speicherauslastung über 60 % und unter 80 % auf.
- Grün dargestellte App Service-Pläne weisen eine CPU-/Speicherauslastung unter 60 % auf.

![Blätter für Azure App Service-Pläne](./media/log-analytics-azure-web-apps-analytics/web-apps-dash02.png)

## <a name="azure-web-apps-log-searches"></a>Azure-Web-Apps – Protokollsuchen

Die **Liste häufiger Azure-Web-Apps-Suchabfragen** enthält alle verwandten Aktivitätsprotokolle für Web-Apps. Dadurch erhalten Sie einen Einblick in die Vorgänge, die auf Ihre Web-Apps-Ressourcen angewendet wurden. Außerdem werden die zugehörigen Vorgänge und die Häufigkeit, mit die sie aufgetreten sind, aufgeführt.

Mit einer der Protokollsuchabfragen als Ausgangspunkt können Sie problemlos eine Warnung erstellen. Beispielsweise können Sie eine Warnung erstellen, wenn die durchschnittliche Antwortzeit einer Metrik einmal pro Sekunde überschreitet.

## <a name="next-steps"></a>Nächste Schritte

- Erstellen Sie eine [Warnung](log-analytics-alerts-creating.md) für eine bestimmte Metrik.
- Verwenden der [Protokollsuche](log-analytics-log-searches.md) zum Anzeigen ausführlicher Informationen aus den Aktivitätsprotokollen

