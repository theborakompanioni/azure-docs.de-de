---
title: "Azure SQL Analytics-Lösung in Log Analytics | Microsoft-Dokumentation"
description: "Die Azure SQL Analytics-Lösung hilft Ihnen bei der Verwaltung Ihrer Azure SQL-Datenbanken."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: b2712749-1ded-40c4-b211-abc51cc65171
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: banders
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: cab45cc6dd621eb4a95ef5f1842ec38c25e980b6
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---


# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview-in-log-analytics"></a>Überwachen von Azure SQL-Datenbank mithilfe von Azure SQL Analytics (Vorschau) in Log Analytics

![Symbol Azure SQL Analytics](./media/log-analytics-azure-sql/azure-sql-symbol.png)

Die Azure SQL Analytics-Lösung in Azure Log Analytics erfasst und visualisiert wichtige SQL Azure-Leistungsmetriken. Anhand der mit der Lösung erfassten Kennzahlen können Sie benutzerdefinierte Überwachungsregeln und -warnungen erstellen. Zudem können Sie Azure SQL-Datenbank und Kennzahlen für Pools für elastische Datenbanken über mehrere Azure-Abonnements und Pools für elastische Datenbanken hinweg überwachen und visuell darstellen. Die Lösung hilft Ihnen außerdem beim Erkennen von Problemen auf jeder Stufe Ihres Anwendungsstapels.  Sie verwendet [Azure-Diagnosemetriken](log-analytics-azure-storage.md) zusammen mit Log Analytics-Ansichten, um Daten zu allen Azure SQL-Datenbanken und Pools für elastische Datenbanken in einem einzelnen Log Analytics-Arbeitsbereich darzustellen.

Zurzeit unterstützt diese Vorschaulösung bis zu 150.000 Azure SQL-Datenbanken und 5.000 Pools für elastische SQL-Datenbanken pro Arbeitsbereich.

Die Azure SQL Analytics-Lösung unterstützt Sie wie andere für Log Analytics verfügbare Lösungen bei der Überwachung und dem Empfang von Benachrichtigungen über den Zustand der Azure-Ressourcen, in diesem Fall von Azure SQL-Datenbank. Microsoft Azure SQL-Datenbank ist ein skalierbarer relationaler Datenbankdienst, der vertraute SQL Server-ähnliche Funktionen für Anwendungen bereitstellt, die in der Azure-Cloud ausgeführt werden. Log Analytics unterstützt Sie beim Erfassen, Korrelieren und Visualisieren strukturierter und nicht strukturierter Daten.

## <a name="connected-sources"></a>Verbundene Quellen

Die Azure SQL Analytics-Lösung verwendet keine Agents für die Verbindung mit dem Log Analytics-Dienst.

In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von der Lösung unterstützt werden.

| Verbundene Quelle | Support | Beschreibung |
| --- | --- | --- |
| [Windows-Agents](log-analytics-windows-agents.md) | Nein | Direkte Windows-Agents werden von der Lösung nicht verwendet. |
| [Linux-Agents](log-analytics-linux-agents.md) | Nein | Direkte Linux-Agents werden von der Lösung nicht verwendet. |
| [SCOM-Verwaltungsgruppe](log-analytics-om-agents.md) | Nein | Von der Lösung wird keine direkte Verbindung zwischen SCOM-Agent und Log Analytics verwendet. |
| [Azure-Speicherkonto](log-analytics-azure-storage.md) | Nein | Log Analytics liest keine Daten aus einem Speicherkonto. |
| [Azure-Diagnose](log-analytics-azure-storage.md) | Ja | Metrikdaten von Azure werden direkt von Azure an Log Analytics gesendet. |

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Falls Sie keines haben, können Sie [kostenlos](https://azure.microsoft.com/free/) eines erstellen.
- Einen Log Analytics-Arbeitsbereich Sie können ein vorhandenes verwenden oder [ein neues erstellen](log-analytics-get-started.md), bevor Sie diese Lösung verwenden.
- Aktivieren Sie die Azure-Diagnose für Ihre Azure SQL-Datenbanken und Pools für elastische Datenbanken, und [konfigurieren Sie diese so, dass ihre Daten an Log Analytics gesendet werden](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).

## <a name="configuration"></a>Konfiguration

Führen Sie die folgenden Schritte aus, um die Azure SQL Analytics-Lösung Ihrem Arbeitsbereich hinzuzufügen.

1. Fügen Sie mithilfe des [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) oder des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses Ihrem Arbeitsbereich die Azure SQL Analytics-Lösung hinzu.
2. Klicken Sie im Azure-Portal auf **Neu** (das Symbol +), und wählen Sie dann in der Liste der Ressourcen **Überwachung und Verwaltung** aus.  
    ![Überwachung und Verwaltung](./media/log-analytics-azure-sql/monitoring-management.png)
3. Klicken Sie in der Liste **Überwachung und Verwaltung** auf **See all** (Alle anzeigen).
4. Klicken Sie in der Liste **Empfohlen** auf **Weitere Informationen**, suchen Sie in der neuen Liste **Azure SQL Analytics (Vorschau)**, und wählen Sie dann diese Option aus.  
    ![Azure SQL Analytics-Lösung](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)
5. Klicken Sie auf dem Blatt **Azure SQL Analytics (Vorschau)** auf **Erstellen**.  
    ![Erstellen](./media/log-analytics-azure-sql/portal-create.png)
6. Wählen Sie auf dem Blatt **Neue Lösung erstellen** den Arbeitsbereich aus, dem Sie die Lösung hinzufügen möchten, und klicken Sie dann auf **Erstellen**.  
    ![Zu Arbeitsbereich hinzufügen](./media/log-analytics-azure-sql/add-to-workspace.png)


### <a name="to-configure-multiple-azure-subscriptions"></a>So konfigurieren Sie mehrere Azure-Abonnements

Wenn mehrere Abonnements unterstützt werden, verwenden Sie das PowerShell-Skript aus [Enable Azure resource metrics logging using PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/) (Aktivieren der Protokollierung der Azure-Ressourcenmetriken mit PowerShell). Geben Sie die Ressourcen-ID des Arbeitsbereichs als Parameter beim Ausführen des Skripts zum Senden von Diagnosedaten aus Ressourcen in einem Azure-Abonnement an einen Arbeitsbereich in einem anderen Azure-Abonnement an.

**Beispiel**

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-the-solution"></a>Verwenden der Lösung

Wenn Sie die Lösung zu Ihrem Arbeitsbereich hinzufügen, wird die Kachel „Azure SQL Analytics“ zu Ihrem Arbeitsbereich hinzugefügt und erscheint in der Übersicht. Die Kachel zeigt die Anzahl der Azure SQL-Datenbanken und der Pools für elastische SQL Azure-Datenbanken, mit denen die Lösung verbunden ist.

![Kachel „Azure SQL Analytics“](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a>Anzeigen von Azure SQL Analytics-Daten

Klicken Sie auf die Kachel **Azure SQL Analytics**, um das Dashboard von Azure SQL Analytics zu öffnen. Das Dashboard enthält die unten festgelegten Blätter. Auf jedem Blatt werden bis zu 15 Ressourcen aufgelistet (Abonnement, Server, Pool für elastische Datenbanken und Datenbank). Klicken Sie auf eine beliebige Ressource, um deren Dashboard zu öffnen. „Pool für elastische Datenbanken“ und „Datenbank“ enthalten die Diagramme mit den Metriken der ausgewählten Ressource. Klicken Sie auf ein Diagramm, um das Dialogfeld „Protokollsuche“ zu öffnen.

| Blatt | Beschreibung |
|---|---|
| Abonnements | Liste der Abonnements mit der Anzahl der verbundenen Server, Pools und Datenbanken. |
| Server | Liste der Server mit der Anzahl der verbundenen Pools und Datenbanken. |
| Elastische Pools | Liste der verbundenen elastische Pools mit maximaler Größe in GB und eDTU im Beobachtungszeitraum. |
|Datenbanken | Liste der verbundenen Datenbanken mit maximaler Größe in GB und DTU im Beobachtungszeitraum.|


### <a name="analyze-data-and-create-alerts"></a>Analysieren von Daten und Erstellen von Warnungen

Sie können problemlos Warnungen mit den Daten erstellen, die aus Azure SQL-Datenbank-Ressourcen stammen. Hier sind einige nützliche Abfragen für die [Protokollsuche](log-analytics-log-searches.md), die Sie für Warnungen verwenden können:

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]


*Hohe DTU in Azure SQL-Datenbank*

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/DATABASES/"* MetricName=dtu_consumption_percent | measure Avg(Average) by Resource interval 5minutes
```

*Hohe DTU in Pool für elastische Datenbanken in Azure SQL-Datenbank*

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource interval 5minutes
```

Sie können diese warnungsbasierten Abfragen verwenden, um bei bestimmten Schwellenwerten für Azure SQL-Datenbanken und Pools für elastische Datenbanken eine Warnung auszugeben. So konfigurieren Sie eine Warnung für Ihren OMS-Arbeitsbereich:

#### <a name="to-configure-an-alert-for-your-workspace"></a>Konfigurieren einer Warnung für Ihren Arbeitsbereich

1. Wechseln Sie zum [OMS-Portal](http://mms.microsoft.com/), und melden Sie sich an.
2. Öffnen Sie den Arbeitsbereich, den Sie für die Lösung konfiguriert haben.
3. Klicken Sie auf der Seite „Übersicht“ auf die Kachel **Azure SQL Analytics (Vorschau)**.
4. Führen Sie eine der Beispielabfragen aus.
5. Klicken Sie in der Protokollsuche auf **Warnung**.  
![Erstellen einer Warnung in der Suche](./media/log-analytics-azure-sql/create-alert01.png)
6. Konfigurieren Sie auf der Seite **Warnungsregel hinzufügen** die entsprechenden Eigenschaften und die spezifischen Schwellenwerte nach Bedarf, und klicken Sie dann auf **Speichern**.  
![Warnungsregel hinzufügen](./media/log-analytics-azure-sql/create-alert02.png)

### <a name="act-on-azure-sql-analytics-data"></a>Bearbeiten von Azure SQL Analytics-Daten

Beispiel: Eine der hilfreichsten Abfragen, die Sie durchführen können, ist der Vergleich der DTU-Auslastung für alle Pools für elastische Azure SQL-Datenbanken in allen Ihren Azure-Abonnements. DTU (Database Throughput Unit; Datenbankdurchsatzeinheit) bietet eine Möglichkeit der Beschreibung der relativen Kapazität einer Leistungsstufe von Basic-, Standard- und Premium-Datenbanken und -Pools. DTUs basieren auf einer kombinierten Messung von CPU, Arbeitsspeicher, Lese- und Schreibvorgängen. Mit steigender Anzahl der DTUs steigt auch die gemäß der Leistungsstufe bereitgestellte Leistungsfähigkeit. Beispiel: Eine Leistungsstufe mit fünf DTUs bietet fünfmal mehr Leistung als eine Leistungsstufe mit einer DTU. Ein maximales DTU-Kontingent gilt für alle Server und Pools für elastische Datenbanken.

Durch Ausführen der folgenden Protokollsuchabfrage können Sie leicht ermitteln, ob Sie die Pools für elastische SQL Azure-Datenbanken zu wenig oder zu stark auslasten.

```
Type=AzureMetrics ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource | display LineChart
```

>[!NOTE]
> Wenn für Ihren Arbeitsbereich ein Upgrade auf die [neue Log Analytics-Abfragesprache](log-analytics-log-search-upgrade.md) durchgeführt wurde, muss die obige Abfrage wie folgt geändert werden.
>
>`search in (AzureMetrics) isnotempty(ResourceId) and "/ELASTICPOOLS/" and MetricName == "dtu_consumption_percent" | summarize AggregatedValue = avg(Average) by bin(TimeGenerated, 1h), Resource | render timechart`

Im folgenden Beispiel sehen Sie, dass ein Pool für elastische Datenbanken eine hohe Nutzung von nahezu 100 Prozent DTU aufweist, während andere eine sehr geringe Nutzung aufweisen. Sie können weitere Untersuchungen durchführen, um durch kürzlich vorgenommene Änderungen verursachte potenzielle Probleme in Ihrer Umgebung mithilfe von Azure-Aktivitätsprotokollen zu beheben.

![Protokollsuchergebnisse – hohe Nutzung](./media/log-analytics-azure-sql/log-search-high-util.png)

## <a name="see-also"></a>Weitere Informationen

- Verwenden Sie die [Protokollsuche](log-analytics-log-searches.md) in Log Analytics, um ausführliche Azure SQL-Daten anzuzeigen.
- [Erstellen eigener Dashboards](log-analytics-dashboards.md), die Azure SQL-Daten anzeigen
- [Erstellen von Warnungen](log-analytics-alerts.md), wenn bestimmte Azure SQL-Ereignisse auftreten

