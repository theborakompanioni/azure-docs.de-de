---
title: "Anzeigen von Azure-Aktivitätsprotokollen mit Log Analytics | Microsoft-Dokumentation"
description: "Mithilfe der Azure-Lösung für Aktivitätsprotokolle können Sie das Azure-Aktivitätsprotokoll in all Ihren Azure-Abonnements analysieren und suchen."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: dbac4c73-0058-4191-a906-e59aca8e2ee0
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: cfdc507d5e45fc716940bd4607631ba500bf4c59
ms.lasthandoff: 04/15/2017


---

# <a name="view-azure-activity-logs"></a>Anzeigen von Azure-Aktivitätsprotokollen

 ![Symbol für Azure-Aktivitätsprotokolle](./media/log-analytics-activity/activity-log-analytics.png) Mithilfe der Log Analytics-Lösung für Aktivitätsprotokolle können Sie das [Azure-Aktivitätsprotokoll](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) in all Ihren Azure-Abonnements analysieren und suchen. Das Azure-Aktivitätsprotokoll bietet Einblicke in Vorgänge, die für Ressourcen in Ihren Abonnements durchgeführt wurden. Das Aktivitätsprotokoll wurde bisher als *Überwachungsprotokolle* oder *Vorgangsprotokolle* bezeichnet, da es Ereignisse für Ihre Abonnements enthält.

Mit dem Aktivitätsprotokoll können Sie die Antworten auf die Fragen *Was*, *Wer* und *Wann* für alle Schreibvorgänge (PUT, POST, DELETE) ermitteln, die für die Ressourcen Ihres Abonnements durchgeführt wurden. Sie können auch den Status der Vorgänge und andere relevante Eigenschaften ermitteln. Das Aktivitätsprotokoll umfasst keine Lesevorgänge (GET) oder Vorgänge für Ressourcen, die das klassische Bereitstellungsmodell verwenden.

Wenn Sie Ihre Azure-Aktivitätsprotokolle mit Log Analytics verbinden, haben Sie folgende Möglichkeiten:

- Analysieren der Aktivitätsprotokolle mit vordefinierten Ansichten
- Analysieren und Suchen von Aktivitätsprotokollen in mehreren Azure-Abonnements
- Speichern von Aktivitätsprotokollen länger als 90 Tage<sup>1</sup>
- Korrelieren von Aktivitätsprotokollen mit anderen Plattform- und Anwendungsdaten in Azure
- Anzeigen operativer Aktivitäten nach Status
- Anzeigen von Aktivitätstrends für jeden einzelnen Ihrer Azure-Dienste
- Berichte zu Autorisierungsänderungen für all Ihre Azure-Ressourcen
- Identifizieren von Ausfällen oder Dienstintegritätsproblemen, die sich auf Ihre Ressourcen auswirken
- Verwenden der Protokollsuche zum Korrelieren von Benutzeraktivitäten, Vorgängen zur automatischen Skalierung, Autorisierungsänderungen und Dienstintegrität mit anderen Protokollen oder Metriken aus Ihrer Umgebung

<sup>1</sup>Standardmäßig werden Azure-Aktivitätsprotokolle in Log Analytics 90 Tage gespeichert, auch wenn Sie den Free-Tarif verwenden. Dies gilt auch, wenn die Aufbewahrungsdauer für Ihren Arbeitsbereich auf weniger als 90 Tage festgelegt ist. Wenn für Ihren Arbeitsbereich eine Aufbewahrungsdauer von mehr als 90 Tagen gilt, werden die Aktivitätsprotokolle über den für den Arbeitsbereich geltenden Zeitraum aufbewahrt.

Mit Log Analytics werden Aktivitätsprotokolle kostenfrei erfasst und kostenfrei 90 Tage gespeichert. Wenn Sie Protokolle länger als 90 Tage speichern, fallen für die länger als 90 Tage gespeicherten Daten Gebühren für die Datenaufbewahrung an.

Wenn Sie den Free-Tarif verwenden, zählen Aktivitätsprotokolle nicht zum täglichen Datenverbrauch.

## <a name="connected-sources"></a>Verbundene Quellen

Im Gegensatz zu den meisten anderen Log Analytics-Lösungen werden die Daten für Aktivitätsprotokolle nicht von Agents erfasst. Alle von der Lösung verwendeten Daten stammen direkt von Azure.

| Verbundene Quelle | Unterstützt | Beschreibung |
| --- | --- | --- |
| [Windows-Agents](log-analytics-windows-agents.md) | Nein | Die Lösung erfasst keine Informationen von Windows-Agents. |
| [Linux-Agents](log-analytics-linux-agents.md) | Nein | Die Lösung erfasst keine Informationen von Linux-Agents. |
| [SCOM-Verwaltungsgruppe](log-analytics-om-agents.md) | Nein | Die Lösung erfasst keine Informationen von Agents in einer verbundenen SCOM-Verwaltungsgruppe. |
| [Azure-Speicherkonto](log-analytics-azure-storage.md) | Nein | Die Lösung erfasst keine Informationen von Azure Storage. |

## <a name="prerequisites"></a>Voraussetzungen

- Für den Zugriff auf die Informationen von Azure-Aktivitätsprotokollen müssen Sie über ein Azure-Abonnement verfügen.

## <a name="configuration"></a>Konfiguration

Führen Sie die folgenden Schritte aus, um die Log Analytics-Lösung für Aktivitätsprotokolle für Ihre Arbeitsbereiche zu konfigurieren.

1. Aktivieren Sie die Log Analytics-Lösung für Aktivitätsprotokolle in [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureActivityOMS?tab=Overview) oder entsprechend den unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Schritten.
2. Konfigurieren Sie Aktivitätsprotokolle so, dass sie in Ihrem Log Analytics-Arbeitsbereich gespeichert werden.
    1. Wählen Sie im Azure-Portal Ihren Arbeitsbereich aus, und klicken Sie dann auf **Azure-Aktivitätsprotokoll**.
    2. Klicken Sie für jedes Abonnement auf den entsprechenden Namen.  
        ![Abonnement hinzufügen](./media/log-analytics-activity/add-subscription.png)
    3. Klicken Sie auf dem Blatt *SubscriptionName* auf **Verbinden**.  
        ![Abonnement verbinden](./media/log-analytics-activity/subscription-connect.png)

Wenn Sie die Lösung über das OMS-Portal hinzufügen, wird die folgende Kachel angezeigt. Melden Sie sich beim Azure-Portal an, um ein Azure-Abonnement mit Ihrem Arbeitsbereich zu verbinden.  
![Bewertung wird durchgeführt](./media/log-analytics-activity/tile-performing-assessment.png)

## <a name="using-the-solution"></a>Verwenden der Lösung

Wenn Sie die Log Analytics-Lösung für Aktivitätsprotokolle Ihrem Arbeitsbereich hinzufügen, wird Ihrem Übersichtsdashboard die Kachel **Azure-Aktivitätsprotokolle** hinzugefügt. Auf dieser Kachel wird die Anzahl der Azure-Aktivitätsdatensätze für die Azure-Abonnements angezeigt, auf die die Lösung Zugriff hat.

![Kachel „Azure-Aktivitätsprotokolle“](./media/log-analytics-activity/azure-activity-logs-tile.png)

### <a name="view-azure-activity-logs"></a>Anzeigen von Azure-Aktivitätsprotokollen

Klicken Sie auf die Kachel **Azure-Aktivitätsprotokolle**, um das Dashboard **Azure-Aktivitätsprotokolle** zu öffnen. Das Dashboard enthält die Blätter, die in der folgenden Tabelle angegeben sind. Auf jedem Blatt sind bis zu 10 Einträge aufgeführt, die die Kriterien des Blatts für den angegebenen Bereich und Zeitraum erfüllen. Sie können eine Protokollsuche durchführen, mit der alle Einträge zurückgegeben werden, indem Sie unten auf dem Blatt auf **Alle anzeigen** oder auf die Blattüberschrift klicken.

Die Aktivitätsprotokolldaten werden nur angezeigt, *nachdem* Sie die Aktivitätsprotokolle so konfiguriert haben, dass sie in der Lösung gespeichert werden. Andernfalls werden die Daten nicht angezeigt.

| Blatt | Beschreibung |
| --- | --- |
| Azure Activity Log Entries (Azure-Aktivitätsprotokolleinträge) | Zeigt ein Balkendiagramm aller Datensätze des ersten Azure-Aktivitätsprotokolleintrags für den ausgewählten Datumsbereich sowie eine Liste der ersten 10 Aktivitätsaufrufer an. Klicken Sie auf das Balkendiagramm, um eine Protokollsuche für <code>Type=AzureActivity</code> durchzuführen. Klicken Sie auf ein Aufruferelement, um eine Protokollsuche durchzuführen, mit der alle Aktivitätsprotokolleinträge für das Element zurückgegeben werden. |
| Activity Logs by Status (Aktivitätsprotokolle nach Status) | Zeigt ein Ringdiagramm für den Status der Azure-Aktivitätsprotokolle für den ausgewählten Datumsbereich an. Zeigt außerdem eine Liste der ersten zehn Statuseinträge an. Klicken Sie auf das Diagramm, um eine Protokollsuche für <code>Type=AzureActivity &#124; measure count() by ActivityStatus</code> durchzuführen. Klicken Sie auf ein Statuselement, um eine Protokollsuche durchzuführen, mit der alle Aktivitätsprotokolleinträge für das Statuselement zurückgegeben werden. |
| Activity Logs by Resource (Aktivitätsprotokolle nach Ressource) | Zeigt die Gesamtanzahl der Ressourcen mit Aktivitätsprotokollen an und listet die ersten zehn Ressourcen mit der Anzahl der Datensätze für jede Ressource auf. Klicken Sie auf den Gesamtbereich, um eine Protokollsuche für <code>Type=AzureActivity &#124; measure count() by Resource</code> durchzuführen, in der alle für die Lösung verfügbaren Azure-Ressourcen angezeigt werden. Klicken Sie auf eine Ressource, um eine Protokollsuche durchzuführen, mit der alle Aktivitätsdatensätze für die Ressource zurückgegeben werden. |
| Activity Logs by Resource Provider (Aktivitätsprotokolle nach Ressourcenanbieter) | Zeigt die Gesamtanzahl der Ressourcenanbieter an, die Aktivitätsprotokolle generieren, und listet die ersten zehn Anbieter auf. Klicken Sie auf den Gesamtbereich, um eine Protokollsuche für <code>Type=AzureActivity &#124; measure count() by ResourceProvider</code> durchzuführen, in der alle Azure-Ressourcenanbieter angezeigt werden. Klicken Sie auf einen Ressourcenanbieter, um eine Protokollsuche durchzuführen, mit der alle Aktivitätsdatensätze für den Anbieter zurückgegeben werden. |

![Dashboard „Azure-Aktivitätsprotokolle“](./media/log-analytics-activity/activity-log-dash.png)

## <a name="next-steps"></a>Nächste Schritte

- Erstellen einer [Warnung](log-analytics-alerts-creating.md) für eine bestimmte Aktivität
- Verwenden der [Protokollsuche](log-analytics-log-searches.md) zum Anzeigen ausführlicher Informationen aus den Aktivitätsprotokollen

