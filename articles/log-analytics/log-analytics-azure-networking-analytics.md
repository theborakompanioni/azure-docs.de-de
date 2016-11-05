---
title: Azure Networking Analytics-Lösung in Log Analytics | Microsoft Docs
description: Mit der Azure Networking Analytics-Lösung in Log Analytics können Sie Protokolle für Azure-Netzwerksicherheitsgruppen und Azure Application Gateway-Protokolle überprüfen.
services: log-analytics
documentationcenter: ''
author: richrundmsft
manager: jochan
editor: ''

ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2016
ms.author: richrund

---
# <a name="azure-networking-analytics-(preview)-solution-in-log-analytics"></a>Azure Networking Analytics-Lösung (Vorschau) in Log Analytics
> [!NOTE]
> Dies ist eine [Vorschaulösung](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).
> 
> 

Mit der Azure Networking Analytics-Lösung in Log Analytics können Sie Azure Application Gateway-Protokolle und Protokolle für Azure-Netzwerksicherheitsgruppen überprüfen.

Sie können die Protokollierung für die Protokolle von Azure Application Gateway und Azure-Netzwerksicherheitsgruppen aktivieren. Diese Protokolle werden in Blob Storage geschrieben, wo sie dann durch Log Analytics für die Suche und Analyse indiziert werden können.

Die folgenden Protokolle werden für Anwendungsgateways unterstützt:

* ApplicationGatewayAccessLog
* ApplicationGatewayPerformanceLog

Die folgenden Protokolle werden für Netzwerksicherheitsgruppen unterstützt:

* NetworkSecurityGroupEvent
* NetworkSecurityGroupRuleCounter

## <a name="install-and-configure-the-solution"></a>Installieren und Konfigurieren der Lösung
Gehen Sie folgendermaßen vor, um die Azure Networking Analytics-Lösung zu installieren und zu konfigurieren:

1. Aktivieren Sie die Diagnoseprotokollierung für Ressourcen, die Sie überwachen möchten.
   * [Application Gateway](../application-gateway/application-gateway-diagnostics.md)
   * [Netzwerksicherheitsgruppe](../virtual-network/virtual-network-nsg-manage-log.md)
2. Konfigurieren Sie Log Analytics für das Lesen der Protokolle aus Blob Storage. Gehen Sie dabei vor wie unter [JSON-Dateien in Blob Storage](log-analytics-azure-storage-json.md) beschrieben.
3. Aktivieren Sie die Azure Networking Analytics-Lösung mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses.  

Wenn Sie die Diagnoseprotokollierung für einen bestimmten Ressourcentyp nicht aktivieren, sind die Dashboardblätter für diese Ressource leer.

## <a name="review-azure-networking-analytics-data-collection-details"></a>Überprüfen der Details zur Datensammlung für Azure Networking Analytics
Die Azure Networking Analytics-Lösung erfasst Diagnoseprotokolle von Azure Blob Storage für Azure-Anwendungsgateways und Netzwerksicherheitsgruppen.
Es ist kein Agent für die Datensammlung erforderlich.

Die folgende Tabelle enthält die Datensammlungsmethoden und andere Details dazu, wie Daten für Azure Networking Analytics erfasst werden.

| Plattform | Direkt-Agent | System Center Operations Manager-Agent (SCOM) | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |![Nein](./media/log-analytics-azure-networking/oms-bullet-red.png) |![Nein](./media/log-analytics-azure-networking/oms-bullet-red.png) |![Ja](./media/log-analytics-azure-networking/oms-bullet-green.png) |![Nein](./media/log-analytics-azure-networking/oms-bullet-red.png) |![Nein](./media/log-analytics-azure-networking/oms-bullet-red.png) |10 Minuten |

## <a name="use-azure-networking-analytics"></a>Verwenden von Azure Networking Analytics
Nach der Installation der Lösung können Sie die Zusammenfassung von Client- und Serverfehlern für die überwachte Anwendungsgateways anzeigen. Verwenden Sie dazu in Log Analytics die Kachel **Azure Networking Analytics** auf der Seite **Übersicht**.

![Abbildung der Kachel „Azure Networking Analytics“](./media/log-analytics-azure-networking/log-analytics-azurenetworking-tile.png)

Nachdem Sie auf die Kachel **Übersicht** geklickt haben, können Sie Zusammenfassungen der Protokolle anzeigen und einen Drilldown zu Details für die folgenden Kategorien durchführen:

* Application Gateway-Zugriffsprotokolle
  * Client- und Serverfehler für Application Gateway-Zugriffsprotokolle
  * Anforderungen pro Stunde für jedes Anwendungsgateway
  * Anforderungen mit Fehlern pro Stunde für jedes Anwendungsgateway
  * Fehler nach Benutzer-Agent für Anwendungsgateways
* Application Gateway-Leistung
  * Integrität von Hosts für Application Gateway
  * Maximum und 95. Perzentil für Application Gateway-Anforderungen mit einem Fehler
* Blockierte Flows der Netzwerksicherheitsgruppe
  * Regeln der Netzwerksicherheitsgruppe mit blockierten Flows
  * MAC-Adressen mit blockierten Flows
* Zugelassene Flows der Netzwerksicherheitsgruppe
  * Regeln der Netzwerksicherheitsgruppe mit zugelassenen Flows
  * MAC-Adressen mit zugelassenen Flows

![Abbildung des Azure Networking Analytics-Dashboards](./media/log-analytics-azure-networking/log-analytics-azurenetworking01.png)

![Abbildung des Azure Networking Analytics-Dashboards](./media/log-analytics-azure-networking/log-analytics-azurenetworking02.png)

![Abbildung des Azure Networking Analytics-Dashboards](./media/log-analytics-azure-networking/log-analytics-azurenetworking03.png)

![Abbildung des Azure Networking Analytics-Dashboards](./media/log-analytics-azure-networking/log-analytics-azurenetworking04.png)

### <a name="to-view-details-for-any-log-summary"></a>So zeigen Sie Details zu einzelnen Protokollzusammenfassungen an
1. Klicken Sie auf der Seite **Übersicht** auf die Kachel **Azure Networking Analytics**.
2. Prüfen Sie im **Azure Networking Analytics**-Dashboard die Zusammenfassungsinformationen in einem der Blätter, und klicken Sie dann auf einen Eintrag, um ausführliche Informationen auf der Seite „Protokollsuche“ anzuzeigen.
   
    Sie können auf jeder Seite für die Protokollsuche die Ergebnisse nach Zeit, detaillierte Ergebnisse und Ihren Protokollsuchverlauf anzeigen. Außerdem können Sie nach Facets filtern, um die Ergebnisse einzugrenzen.

## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie [Protokollsuchen in Log Analytics](log-analytics-log-searches.md), um ausführliche Azure Networking Analytics-Daten anzuzeigen.

<!--HONumber=Oct16_HO2-->


