---
title: "Azure SQL-Analyselösung in Log Analytics | Microsoft-Dokumentation"
description: "Die Azure SQL-Analyselösung hilft Ihnen bei der Verwaltung Ihrer Azure SQL-Datenbanken."
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
ms.date: 02/22/2017
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: 503cf4afba4575492984891a681c187a8683a553
ms.openlocfilehash: 9d8fd7ec594671e1ea7bf06459494f1c3a1adbdf
ms.lasthandoff: 02/23/2017


---


# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview-in-log-analytics"></a>Überwachen der Azure SQL-Datenbank mithilfe von Azure SQL-Analysen (Vorschau) in Log Analytics

Die Azure SQL-Überwachungslösung in Azure Log Analytics erfasst und visualisiert wichtige SQL Azure-Leistungsmetriken. Anhand der mit der Lösung erfassten Kennzahlen können Sie benutzerdefinierte Überwachungsregeln und -warnungen erstellen. Zudem können Sie Azure SQL-Datenbank und Kennzahlen für Pools für elastische Datenbanken über mehrere Azure-Abonnements und Pools für elastische Datenbanken hinweg überwachen und visuell darstellen. Die Lösung hilft Ihnen außerdem beim Erkennen von Problemen auf jeder Stufe Ihres Anwendungsstapels.  Sie verwendet [Azure-Diagnosemetriken](log-analytics-azure-storage.md) zusammen mit Log Analytics-Ansichten, um Daten zu allen Azure SQL-Datenbanken und Pools für elastische Datenbanken in einem einzelnen Log Analytics-Arbeitsbereich darzustellen.

Zurzeit unterstützt diese Vorschaulösung bis zu 150.000 Azure SQL-Datenbanken und 5.000 Pools für elastische SQL-Datenbanken pro Arbeitsbereich.

Die Azure SQL-Überwachungslösung unterstützt Sie wie andere für Log Analytics verfügbare Lösungen bei der Überwachung und dem Empfang von Benachrichtigungen über den Zustand der Azure-Ressourcen, in diesem Fall von Azure SQL-Datenbank. Microsoft Azure SQL-Datenbank ist ein skalierbarer relationaler Datenbankdienst, der vertraute SQL Server-ähnliche Funktionen für Anwendungen bereitstellt, die in der Azure-Cloud ausgeführt werden. Log Analytics unterstützt Sie beim Erfassen, Korrelieren und Visualisieren strukturierter und nicht strukturierter Daten.

## <a name="connected-sources"></a>Verbundene Quellen

Anders als die meisten anderen Log Analytics-Lösungen verwendet die Azure SQL-Überwachungslösung keine Agents für die Verbindung mit dem Log Analytics-Dienst.

In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von der Lösung unterstützt werden.

| Verbundene Quelle | Support | Beschreibung |
| --- | --- | --- |
| [Windows-Agents](log-analytics-windows-agents.md) | Nein | Direkte Windows-Agents werden von der Lösung nicht verwendet. |
| [Linux-Agents](log-analytics-linux-agents.md) | Nein | Direkte Linux-Agents werden von der Lösung nicht verwendet. |
| [SCOM-Verwaltungsgruppe](log-analytics-om-agents.md) | Nein | Von der Lösung wird keine direkte Verbindung zwischen SCOM-Agent und Log Analytics verwendet. |
| [Azure-Speicherkonto](log-analytics-azure-storage.md) | Ja | Metrikdaten von Azure werden mithilfe des Speicherkontos an Log Analytics gesendet. |

## <a name="prerequisites"></a>Voraussetzungen

1. Ein Azure-Abonnement. Falls Sie keines haben, können Sie [kostenlos](https://azure.microsoft.com/free/) eines erstellen.
2. Einen Log Analytics-Arbeitsbereich Sie können ein vorhandenes verwenden oder [ein neues erstellen](log-analytics-get-started.md), bevor Sie diese Lösung verwenden.
3. Aktivieren Sie die Azure-Diagnose für Ihre Azure SQL-Datenbanken und Pools für elastische Datenbanken, und [konfigurieren Sie diese so, dass ihre Daten an Log Analytics gesendet werden](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).

## <a name="configuration"></a>Konfiguration

Führen Sie die folgenden Schritte aus, um die Azure SQL-Überwachungslösung Ihrem Arbeitsbereich hinzuzufügen.

1. Fügen Sie mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses Ihrem Arbeitsbereich die Azure SQL-Überwachungslösung hinzu.
2. Klicken Sie im Azure-Portal auf **Neu** (das Symbol +), und wählen Sie dann in der Liste der Ressourcen **Überwachung und Verwaltung** aus.  
    ![Überwachung und Verwaltung](./media/log-analytics-azure-sql/monitoring-management.png)
3. Klicken Sie in der Liste **Überwachung und Verwaltung** auf **See all** (Alle anzeigen).
4. Klicken Sie in der Liste **Empfohlen** auf **Weitere Informationen**, suchen Sie in der neuen Liste **Azure SQL-Analysen (Vorschau)**, und wählen Sie dann diese Option aus.  
    ![Azure SQL-Analyselösung](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)
5. Klicken Sie auf dem Blatt **Azure SQL-Analysen (Vorschau)** auf **Erstellen**.  
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

Wenn Sie die Lösung zu Ihrem Arbeitsbereich hinzufügen, wird die Kachel „Azure SQL-Analysen“ zu Ihrem Arbeitsbereich hinzugefügt und erscheint in der Übersicht. Die Kachel zeigt die Anzahl der Azure SQL-Datenbanken und der Pools für elastische SQL Azure-Datenbanken, mit denen die Lösung verbunden ist.

![Kachel „Azure SQL-Analysen“](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-monitoring-data"></a>Anzeigen der Daten der Azure SQL-Überwachung

Klicken Sie auf die Kachel **Azure SQL-Überwachung**, um das Dashboard „Azure SQL-Analysen“ zu öffnen. Das Dashboard enthält die Spalten, die in der folgenden Tabelle angegeben sind. In jeder Spalte sind bis zu zehn Einträge enthalten, die die Spaltenkriterien für den angegebenen Bereich und Zeitraum erfüllen. Sie können eine Protokollsuche durchführen, mit der alle Einträge zurückgegeben werden, indem Sie unten in der Spalte auf **Alle anzeigen** oder auf die Spaltenüberschrift klicken.

Lesen Sie mehr über [SQL-Datenbankoptionen und -leistung für Dienstebenen](../sql-database/sql-database-service-tiers.md).



![Azure SQL-Analysedashboard](./media/log-analytics-azure-sql/azure-sql-dash-01.png)



![Azure SQL-Analysedashboard](./media/log-analytics-azure-sql/azure-sql-dash-02.png)

| Spalte | Beschreibung |
| --- | --- |
| **Azure SQL-Datenbankanalyse** | &nbsp; |
| Die besten n Datenbanken nach DTU-Nutzung &gt; 90% | Dieser Bereich zeigt die Anzahl der Azure SQL-Datenbanken mit einer DTU-Nutzung von mehr als 90 Prozent im ausgewählten Zeitraum. Die oberste Kachel zeigt die Anzahl der Datenbanken, die während desselben angegebenen Zeitraums mehr als 90 Prozent der insgesamt zugeordneten DTU-Verfügbarkeit aller in Log Analytics überwachten Datenbanken in Anspruch genommen haben.  Klicken Sie auf einen Datenbanknamen, um eine Protokollsuche durchzuführen, die ein Liniendiagramm zeigt, in dem die DTU-Auslastung der Datenbank mit allen anderen im Arbeitsbereich überwachten Datenbanken verglichen wird. |
| Die besten n Datenbanken nach CPU-Nutzung &gt; 90% | Dieser Bereich zeigt die Anzahl der Azure SQL-Datenbanken mit einer CPU-Nutzung von mehr als 90 Prozent im ausgewählten Zeitraum. Die oberste Kachel zeigt die Anzahl der Datenbanken, die während desselben angegebenen Zeitraums mehr als 90 Prozent der insgesamt zugeordneten CPU-Verfügbarkeit aller in Log Analytics überwachten Datenbanken in Anspruch genommen haben.  Klicken Sie auf einen Datenbanknamen, um eine Protokollsuche durchzuführen, die ein Liniendiagramm zeigt, in dem die CPU-Auslastung der Datenbank mit allen anderen im Arbeitsbereich überwachten Datenbanken verglichen wird. |
| Die besten n Datenbanken nach Speichernutzung &gt; 90% | Dieser Bereich zeigt die Anzahl der Azure SQL-Datenbanken mit einer Speicherzuteilung von mehr als 90 Prozent im ausgewählten Zeitraum. Die oberste Kachel zeigt die Anzahl der Datenbanken, die während des angegebenen Zeitraums den Schwellenwert von 90 Prozent aller in Log Analytics überwachten Datenbanken überschritten haben.  Klicken Sie auf einen Datenbanknamen, um eine Protokollsuche durchzuführen, die ein Liniendiagramm zeigt, in dem die Speichernutzung der Datenbank mit allen anderen im Arbeitsbereich überwachten Datenbanken verglichen wird. |
| **Pool für elastische Azure SQL-Datenbanken** | &nbsp; |
| Die besten n Pools für elastische Datenbanken nach DTU &gt; 90% | Dieser Bereich zeigt die Anzahl der Pools für elastische Azure SQL-Datenbanken mit einer DTU-Zuteilung von mehr als 90 Prozent im ausgewählten Zeitraum. Die oberste Kachel zeigt die Anzahl der Pools für elastische Datenbanken, die während des angegebenen Zeitraums den Schwellenwert von 90 Prozent aller in Log Analytics überwachten Pools für elastische Azure SQL-Datenbanken überschritten haben.  Klicken Sie auf einen Pool für elastische Datenbanken, um eine Protokollsuche durchzuführen, die ein Liniendiagramm zeigt, in dem die Speichernutzung des Pools für elastische Datenbanken mit allen anderen im Arbeitsbereich überwachten Datenbanken verglichen wird. |
| Die besten n Pools für elastische Datenbanken nach CPU &gt; 90% | Dieser Bereich zeigt die Anzahl der Pools für elastische Azure SQL-Datenbanken mit einer CPU-Nutzung von mehr als 90 Prozent im ausgewählten Zeitraum. Die oberste Kachel zeigt die Anzahl der Pools für elastische Datenbanken, die während des angegebenen Zeitraums den Schwellenwert von 90 Prozent aller in Log Analytics überwachten Pools für elastische Azure SQL-Datenbanken überschritten haben.  Klicken Sie auf einen Pool für elastische Datenbanken, um eine Protokollsuche durchzuführen, die ein Liniendiagramm zeigt, in dem die CPU-Nutzung des Pools für elastische Datenbanken mit allen anderen im Arbeitsbereich überwachten Datenbanken verglichen wird. |
| Die besten n Pools für elastische Datenbanken nach Speicherverbrauch &gt; 90% | Dieser Bereich zeigt die Anzahl der Pools für elastische Azure SQL-Datenbanken mit einer Speicherzuteilung von mehr als 90 Prozent im ausgewählten Zeitraum. Die oberste Kachel zeigt die Anzahl der Pools für elastische Datenbanken, die während des angegebenen Zeitraums den Schwellenwert von 90 Prozent aller in Log Analytics überwachten Pools für elastische Datenbanken überschritten haben.  Klicken Sie auf einen Pool für elastische Datenbanken, um eine Protokollsuche durchzuführen, die ein Liniendiagramm zeigt, in dem die Speichernutzung des Pools für elastische Datenbanken mit allen anderen im Arbeitsbereich überwachten Datenbanken verglichen wird. |
| **Azure SQL-Aktivitätsprotokolle** | &nbsp; |
| Audit von SQL Azure-Aktivitäten | Dieser Bereich zeigt die Anzahl der Azure-Aktivitätsdatensätze im Zusammenhang mit SQL Azure im ausgewählten Zeitraum. Klicken Sie auf ein Element, um eine Protokollsuche durchzuführen, die zusätzliche Details zum Element anzeigt. |



### <a name="analyze-data-and-create-alerts"></a>Analysieren von Daten und Erstellen von Warnungen

Die Lösung enthält hilfreiche Abfragen, die Ihnen beim Analysieren der Daten helfen sollen. Wenn Sie einen Bildlauf nach rechts durchführen, werden im Dashboard mehrere allgemeine Abfragen aufgeführt, auf die Sie klicken können, um eine [Protokollsuche](log-analytics-log-searches.md) nach Azure SQL-Daten durchzuführen.

![Abfragen](./media/log-analytics-azure-sql/azure-sql-queries.png)

Die Lösung enthält einige *warnungsbasierte Abfragen*, wie oben gezeigt, die Sie verwenden können, um bei bestimmten Schwellenwerten für Azure SQL-Datenbanken und Pools für elastische Datenbanken eine Warnung auszugeben.

#### <a name="to-configure-an-alert-for-your-workspace"></a>Konfigurieren einer Warnung für Ihren Arbeitsbereich

1. Wechseln Sie zum [OMS-Portal](http://mms.microsoft.com/), und melden Sie sich an.
2. Öffnen Sie den Arbeitsbereich, den Sie für die Lösung konfiguriert haben.
3. Klicken Sie auf der Seite „Übersicht“ auf die Kachel **Azure SQL-Analysen (Vorschau)**.
4. Führen Sie einen Bildlauf nach rechts durch, und klicken Sie auf eine Abfrage, um mit dem Erstellen einer Warnung zu beginnen.  
![Warnungsabfrage](./media/log-analytics-azure-sql/alert-query.png)
5. Klicken Sie in der Protokollsuche auf **Warnung**.  
![Erstellen einer Warnung in der Suche](./media/log-analytics-azure-sql/create-alert01.png)
6. Konfigurieren Sie auf der Seite **Warnungsregel hinzufügen** die entsprechenden Eigenschaften und die spezifischen Schwellenwerte nach Bedarf, und klicken Sie dann auf **Speichern**.  
![Warnungsregel hinzufügen](./media/log-analytics-azure-sql/create-alert02.png)

### <a name="act-on-azure-sql-monitoring-data"></a>Bearbeiten der Daten der Azure SQL-Überwachung

Beispiel: Eine der hilfreichsten Abfragen, die Sie durchführen können, ist der Vergleich der DTU-Auslastung für alle Pools für elastische Azure SQL-Datenbanken in allen Ihren Azure-Abonnements. DTU (Database Throughput Unit; Datenbankdurchsatzeinheit) bietet eine Möglichkeit der Beschreibung der relativen Kapazität einer Leistungsstufe von Basic-, Standard- und Premium-Datenbanken und -Pools. DTUs basieren auf einer kombinierten Messung von CPU, Arbeitsspeicher, Lese- und Schreibvorgängen. Mit steigender Anzahl der DTUs steigt auch die gemäß der Leistungsstufe bereitgestellte Leistungsfähigkeit. Beispiel: Eine Leistungsstufe mit fünf DTUs bietet fünfmal mehr Leistung als eine Leistungsstufe mit einer DTU. Ein maximales DTU-Kontingent gilt für alle Server und Pools für elastische Datenbanken.

Durch Ausführen der folgenden Protokollsuchabfrage können Sie leicht ermitteln, ob Sie die Pools für elastische SQL Azure-Datenbanken zu wenig oder zu stark auslasten.

```
Type=AzureMetrics ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource | display LineChart
```

Im folgenden Beispiel sehen Sie, dass ein Pool für elastische Datenbanken eine hohe Nutzung von nahezu 100 Prozent DTU aufweist, während andere eine sehr geringe Nutzung aufweisen. Sie können weitere Untersuchungen durchführen, um durch kürzlich vorgenommene Änderungen verursachte potenzielle Probleme in Ihrer Umgebung mithilfe von Azure-Aktivitätsprotokollen zu beheben.

![Protokollsuchergebnisse – hohe Nutzung](./media/log-analytics-azure-sql/log-search-high-util.png)

## <a name="see-also"></a>Weitere Informationen

- Verwenden Sie die [Protokollsuche](log-analytics-log-searches.md) in Log Analytics, um ausführliche Azure SQL-Daten anzuzeigen.
- [Erstellen eigener Dashboards](log-analytics-dashboards.md), die Azure SQL-Daten anzeigen
- [Erstellen von Warnungen](log-analytics-alerts.md), wenn bestimmte Azure SQL-Ereignisse auftreten

