---
title: "Hinzufügen von Azure Log Analytics-Verwaltungslösungen | Microsoft-Dokumentation"
description: "Bei den Verwaltungslösungen von Operations Management Suite (OMS) bzw. von Log Analytics handelt sind um eine Sammlung von Logik-, Visualisierungs- und Datenerfassungsregeln, die Metriken rund um einen bestimmten Problembereich bereitstellen."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 540180e7d6cd02dfa1f3cac8ccd343e965ded91b
ms.openlocfilehash: 1ba1bb02c27fa040cc2daef4baf5c9ecc827d323
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---
# <a name="add-azure-log-analytics-management-solutions-to-your-workspace"></a>Hinzufügen von Azure Log Analytics-Verwaltungslösungen zu Ihrem Arbeitsbereich

Log Analytics-Verwaltungslösungen sind eine Sammlung von **Logik**-, **Visualisierungs**- und **Datenerfassungsregeln**, die Metriken rund um einen bestimmten Problembereich bereitstellen. In diesem Artikel sind von Log Analytics unterstützte Verwaltungslösungen aufgeführt. Darüber hinaus erfahren Sie, wie Sie mithilfe des Azure-Portals einen Arbeitsbereich hinzufügen und entfernen. Sie können auch Lösungen im OMS-Portal mithilfe des Lösungskatalogs hinzufügen.

Die detaillierten Einblicke der Verwaltungslösungen ermöglichen Folgendes:

* Schnelleres Untersuchen und Lösen von Betriebsproblemen
* Sammeln und Korrelieren von verschiedenen Arten von Computerdaten
* Proaktive Behandlung von Aktivitäten, die von der Lösung verfügbar gemacht werden

> [!NOTE]
> Log Analytics verfügt über eine integrierte Protokollsuche, sodass nicht extra eine Verwaltungslösung installiert werden muss. Sie können jedoch Datenvisualisierungen, Vorschläge für Suchen und Einblicke nutzen, indem Sie Verwaltungslösungen zu Ihrem Arbeitsbereich hinzufügen.

In diesem Artikel fügen Sie mit dem Marketplace im Azure-Portal einem Arbeitsbereich Verwaltungslösungen hinzu. Nachdem Sie eine Lösung hinzugefügt haben, werden Daten von den Servern in Ihrer Infrastruktur erfasst und an den OMS-Dienst gesendet. Die Verarbeitung durch den OMS-Dienst dauert normalerweise von einigen Minuten bis zu einer Stunde. Nach der Verarbeitung der Daten durch den Dienst können Sie diese in OMS anzeigen.

Wenn eine Verwaltungslösung nicht mehr benötigt wird, können Sie sie problemlos entfernen. Wenn Sie eine Verwaltungslösung entfernen, werden deren Daten nicht an OMS gesendet. Bei Verwendung des Free-Tarifs lässt sich durch Entfernen einer Lösung die genutzte Datenmenge verringern, um eine Überschreitung des Tageskontingents zu vermeiden.

## <a name="view-available-management-solutions"></a>Anzeigen verfügbarer Verwaltungslösungen

Azure Marketplace enthält die Liste der [Verwaltungslösungen für Log Analytics](https://azuremarketplace.microsoft.com/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).

Sie können Verwaltungslösungen aus Azure Marketplace installieren, indem Sie unten in jeder Lösung auf den Link **Jetzt herunterladen** klicken.

## <a name="add-a-management-solution"></a>Hinzufügen einer Verwaltungslösung
1. Melden Sie sich mit Ihrem Azure-Abonnement beim [Azure-Portal](https://portal.azure.com) an, sofern Sie noch nicht angemeldet sind.
2. Wählen Sie unter **Marketplace** auf dem Blatt **Neu** die Option **Überwachung und Verwaltung**.
3. Klicken Sie auf dem Blatt **Überwachung und Verwaltung** auf **See all** (Alle anzeigen).  
    ![Blatt „Überwachung und Verwaltung“](./media/log-analytics-add-solutions/monitoring-management-blade.png)  
4. Klicken Sie rechts neben **Verwaltungslösungen** auf **More** (Mehr).
5. Wählen Sie auf dem Blatt **Verwaltungslösungen** eine Verwaltungslösung aus, die Sie einem Arbeitsbereich hinzufügen möchten.  
    ![Blatt „Überwachung und Verwaltung“](./media/log-analytics-add-solutions/management-solutions.png)  
6. Überprüfen Sie auf dem Blatt mit der Verwaltungslösung die Informationen zur Verwaltungslösung, und klicken Sie dann auf **Erstellen**.
7. Wählen Sie auf dem Blatt *Name der Verwaltungslösung* einen Arbeitsbereich aus, den Sie der Verwaltungslösung zuordnen möchten.
8. Ändern Sie optional die Arbeitsbereichseinstellungen für das Azure-Abonnement, die Ressourcengruppe und den Standort. Sie können auch **Automatisierungsoptionen** auswählen. Klicken Sie auf **Erstellen**.  
    ![Lösungsarbeitsbereich](./media/log-analytics-add-solutions/solution-workspace.png)  
9. Um die dem Arbeitsbereich hinzugefügte Verwaltungslösung zu verwenden, navigieren Sie zu **Log Analytics** > **Abonnements** > ***Name des Arbeitsbereichs*** > **Übersicht**. Eine neue Kachel für Ihre Verwaltungslösung wird angezeigt. Klicken Sie auf die Kachel, um sie zu öffnen, und verwenden Sie die Lösung, nachdem Daten für sie gesammelt wurden.

## <a name="remove-a-management-solution"></a>Entfernen einer Verwaltungslösung

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu **Log Analytics** > **Abonnements** > ***Name des Arbeitsbereichs***, und klicken Sie auf dem Blatt ***Name des Arbeitsbereichs*** auf **Lösungen**.
2. Wählen Sie in der Liste der Verwaltungslösungen die Lösung aus, die Sie entfernen möchten.
3. Klicken Sie auf dem Lösungsblatt für den Arbeitsbereich auf **Löschen**.  
    ![Lösung löschen](./media/log-analytics-add-solutions/solution-delete.png)  
4. Klicken Sie im Bestätigungsdialogfeld auf **Ja**.

## <a name="offers-and-pricing-tiers"></a>Angebote und Tarife

In der folgenden Tabelle erfahren Sie, welche Verwaltungslösungen zu den einzelnen Operations Management Suite-Angeboten gehören.
In der Tabelle sind außerdem die verfügbaren Tarife für die einzelnen Verwaltungslösungen angegeben.
Alle Lösungen in der folgenden Tabelle sind im Azure-Portal und im Lösungskatalog des Log Analytics-Portals verfügbar.

| Verwaltungslösung                                                                       | Angebot                                                                     | Tarife<sup>1</sup>                                                 | Hinweise |
| ---                                                                                       | ---                                                                       | ---                                                                                                       | ---   |
| [Aktivitätsprotokollanalyse](log-analytics-activity.md)                                                                   | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | Kostenloses Datenvolumen (90 Tage)<br>Daten unterliegen nicht der Obergrenze für den Free-Tarif |
| [AD-Bewertung](log-analytics-ad-assessment.md)                                           | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | |
| [AD-Replikationsstatus](log-analytics-ad-replication-status.md)                           | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | Steht über das Azure-Portal oder den Marketplace nicht zum Hinzufügen zur Verfügung. |
| [Agent-Integrität](../operations-management-suite/oms-solution-agenthealth.md)                                                                                | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | Daten unterliegen nicht der Obergrenze für den Free-Tarif<br> Steht über das Azure-Portal oder den Marketplace nicht zum Hinzufügen zur Verfügung. |
| [Benachrichtigungsverwaltung](log-analytics-solution-alert-management.md)                            | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | Steht über das Azure-Portal oder den Marketplace nicht zum Hinzufügen zur Verfügung. |
| [Application Insights-Connector (Vorschau)](log-analytics-app-insights-connector.md)                                               | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | |
| [Automation Hybrid Worker](../automation/automation-hybrid-runbook-worker.md)                                                                     | <ul><li>Automation & Control</li></ul>                                  | Kostenlos<br> Pro&nbsp;Knoten&nbsp;(OMS)                                                                         | Erfordert die Verknüpfung Ihres Log Analytics-Arbeitsbereichs mit einem Automation-Konto |
| [Azure Application Gateway-Analyse](log-analytics-azure-networking-analytics.md)    | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | |
| [Azure-Netzwerksicherheitsgruppen-Analyse](log-analytics-azure-networking-analytics.md)     | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | |
| [Azure SQL Analytics (Vorschau)](log-analytics-azure-sql.md)                                                       | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br>Pro&nbsp;Knoten&nbsp;(OMS)                                                                          | Erfordert die Verknüpfung Ihres Log Analytics-Arbeitsbereichs mit einem Automation-Konto|
| [Azure-Web-Apps-Analyse](log-analytics-azure-web-apps-analytics.md)     | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | |
|[Sicherung](../backup/backup-introduction-to-azure-backup.md)                                                                                 | <ul><li>Insight & Analytics</li></ul>                                   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)                                                                       | Erfordert einen klassischen Sicherungstresor.<br> Steht über das Azure-Portal oder den Marketplace nicht zum Hinzufügen zur Verfügung. |
| [Kapazität und Leistung (Vorschau)](log-analytics-capacity.md)                                                   | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | |
| [Änderungsnachverfolgung](log-analytics-change-tracking.md)                                       | <ul><li>Automation & Control</li></ul>                                  | Kostenlos<br> Pro&nbsp;Knoten&nbsp;(OMS)                                                                         | Erfordert die Verknüpfung Ihres Log Analytics-Arbeitsbereichs mit einem Automation-Konto |
| [Container](log-analytics-containers.md)                                                 | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | |
| [Symbol für den IT Service Management Connector](log-analytics-itsmc-overview.md)                                              | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Pro&nbsp;Knoten&nbsp;(OMS)     | |
| HDInsight HBase-Überwachung <br>(Vorschau)                                                  | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | |
| [Key Vault-Analysen](log-analytics-azure-key-vault.md)                   | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | |
| [B2B-Logik-App-Verwaltung](../logic-apps/logic-apps-track-b2b-messages-omsportal.md)                    | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | Steht über das Azure-Portal oder den Marketplace nicht zum Hinzufügen zur Verfügung. |
| [Bewertung von Schadsoftware](log-analytics-malware.md)                                            | <ul><li>Sicherheit und Compliance</li></ul>                                 | Kostenlos<br> Eigenständig<br>Pro&nbsp;Knoten&nbsp;(OMS)                                                                           | Wenn Sie die Sicherheits- und Compliancelösungen nach dem 19. Juni 2017 hinzufügen, erfolgt die [Abrechnung pro Knoten](https://azure.microsoft.com/pricing/details/security-compliance/), unabhängig vom Arbeitsbereichstarif. Die ersten 60 Tage sind kostenlos.  |
| [Netzwerkleistungsmonitor](log-analytics-network-performance-monitor.md) <br>  | <ul><li>Insight & Analytics</li></ul>                                   | Kostenlos<br> Pro&nbsp;Knoten&nbsp;(OMS)                                                                         | |
| [Office 365-Analyse (Vorschau)](../operations-management-suite/oms-solution-office-365.md)                                                       | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | |
| [Sicherheit und Überwachung](../operations-management-suite/oms-security-getting-started.md)      | <ul><li>Sicherheit&nbsp;und&nbsp;Compliance</li></ul>                       | Kostenlos<br> Eigenständig<br>Pro&nbsp;Knoten&nbsp;(OMS)                                                                           | Erforderliche Lösung für die Erfassung von Sicherheitsereignisprotokollen<br>Wenn Sie die Sicherheits- und Compliancelösungen nach dem 19. Juni 2017 hinzufügen, erfolgt die [Abrechnung pro Knoten](https://azure.microsoft.com/pricing/details/security-compliance/), unabhängig vom Arbeitsbereichstarif. Die ersten 60 Tage sind kostenlos. |
| [Service Fabric-Analysen (Vorschau)](log-analytics-service-fabric.md)                     | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | |
| [Dienstzuordnung (Vorschau)](../operations-management-suite/operations-management-suite-service-map.md) | <ul><li>Insight & Analytics</li></ul>                      | Kostenlos<br> Pro&nbsp;Knoten&nbsp;(OMS)                                                                         | Verfügbar in den Regionen „USA, Osten“, „Europa, Westen“ und „USA, Westen-Mitte“    |
| [Site Recovery](../site-recovery/site-recovery-overview.md)                                                                               | <ul><li>Insight & Analytics</li></ul>                                   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)                                                                       | Erfordert einen klassischen Site Recovery-Tresor.<br> Steht über das Azure-Portal oder den Marketplace nicht zum Hinzufügen zur Verfügung. |
| [SQL-Bewertung](log-analytics-sql-assessment.md)                                         | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | |
| Starten/Beenden von VMs außerhalb der Kernzeit<br>(Vorschau)                                              | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Pro&nbsp;Knoten&nbsp;(OMS)                                                                         | Erfordert die Verknüpfung Ihres Log Analytics-Arbeitsbereichs mit einem Automation-Konto |
| [SurfaceHub](log-analytics-surface-hubs.md)                                               | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | Steht über das Azure-Portal oder den Marketplace nicht zum Hinzufügen zur Verfügung. |
| [System Center Operations Manager-Bewertung (Vorschau)](log-analytics-scom-assessment.md)  | <ul><li>Insight & Analytics</li><li>Log Analytics</li></ul>        | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | |
| [Updateverwaltung](../operations-management-suite/oms-solution-update-management.md)                                                                         | <ul><li>Automation & Control</li></ul>                                  | Kostenlos<br> Pro&nbsp;Knoten&nbsp;(OMS)                                                                         | Erfordert die Verknüpfung Ihres Log Analytics-Arbeitsbereichs mit einem Automation-Konto |
| [Updatekonformität (Vorschau)](https://docs.microsoft.com/windows/deployment/update/update-compliance-get-started)                                                             | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | Keine Gebühr für Daten oder Knoten<br>Daten unterliegen nicht der Obergrenze für den Free-Tarif.<br> Steht über das Azure-Portal oder den Marketplace nicht zum Hinzufügen zur Verfügung. |
| [Upgradebereitschaft](https://docs.microsoft.com/windows/deployment/upgrade/upgrade-readiness-get-started)                                                          | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | Keine Gebühr für Daten oder Knoten<br>Daten unterliegen nicht der Obergrenze für den Free-Tarif.<br> Steht über das Azure-Portal oder den Marketplace nicht zum Hinzufügen zur Verfügung. |
| [VMware-Überwachung (Vorschau)](log-analytics-vmware.md)                                | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Standard<br> Premium&nbsp;(OMS)<br> Pro&nbsp;GB&nbsp;(eigenständig)<br> Pro&nbsp;Knoten&nbsp;(OMS)   | |
| [Wire Data 2.0 (Vorschau)](log-analytics-wire-data.md)                                                                 | <ul><li>Insight & Analytics</li></ul>                                   | Kostenlos<br> Pro&nbsp;Knoten&nbsp;(OMS)                                                                         | Verfügbar in den Regionen „USA, Osten“, „Europa, Westen“ und „USA, Westen-Mitte“ |

<sup>1</sup> Die Tarife *Standard* und *Premium (OMS)* sind nur für Kunden verfügbar, die ihren Log Analytics-Arbeitsbereich vor dem 21. September 2016 erstellt haben.

### <a name="community-provided-management-solutions"></a>Verwaltungslösungen aus der Community

Von der Community bereitgestellte Lösungen sind über den [Azure-Vorlagenkatalog](https://azure.microsoft.com/resources/templates/?term=Per&nbsp;Node&nbsp;(OMS)) sowie direkt bei den Autoren erhältlich.

| Verwaltungslösung               | Angebot                                                                     | Tarife                         | Hinweise |
| ---                               | ---                                                                       | ---                                   | ---   |
| Alle von der Community bereitgestellten Lösungen  | <ul><li>Insight&nbsp;&&nbsp;Analytics</li><li>Log Analytics</li></ul>   | Kostenlos<br> Pro&nbsp;Knoten&nbsp;(OMS)     |   Erfordert die Verknüpfung Ihres Log Analytics-Arbeitsbereichs mit einem Automation-Konto |




## <a name="data-collection-details"></a>Details zur Datensammlung
Die folgenden Tabellen enthalten Datensammlungsmethoden und andere Details zur Erfassung von Daten für Log Analytics-Verwaltungslösungen und Datenquellen. Die Tabellen sind nach Lösungsangeboten kategorisiert, die den [Abonnementtarifen](https://go.microsoft.com/fwlink/?linkid=827926) entsprechen. Die Lösung für die Aktivitätsprotokollanalyse ist in allen Tarifen kostenlos.

Der Log Analytics-Windows-Agent und der System Center Operations Manager-Agent sind im Grunde identisch. Der Windows-Agent bietet jedoch zusätzliche Funktionen, die eine Verbindung mit dem OMS-Arbeitsbereich und die Weiterleitung über einen Proxy ermöglichen. Bei Verwendung eines Operations Manager-Agents muss er wie ein OMS-Agent verwendet werden, damit eine Kommunikation mit OMS möglich ist. Bei den Operations Manager-Agents in dieser Tabelle handelt es sich um mit Operations Manager verbundene OMS-Agents. Weitere Informationen zum Herstellen einer Verbindung zwischen Ihrer vorhandenen Operations Manager-Umgebung mit OMS finden Sie unter [Verbinden von Operations Manager mit Log Analytics](log-analytics-om-agents.md).

> [!NOTE]
> Der Typ des verwendeten Agents bestimmt, wie Daten an OMS gesendet werden. Dabei gelten die folgenden Bedingungen:
> - Sie verwenden entweder den Windows-Agent oder einen mit Operations Manager verbundenen OMS-Agent.
> - Wenn Operations Manager erforderlich ist, werden Operations Manager-Agent-Daten für die Lösung immer unter Verwendung der Operations Manager-Verwaltungsgruppe an OMS gesendet. Außerdem wird von der Lösung in diesem Fall nur der Operations Manager-Agent verwendet.
> - Wenn Operations Manager nicht erforderlich ist und Operations Manager-Agent-Daten laut Tabelle unter Verwendung der Verwaltungsgruppe an OMS gesendet werden, werden Operations Manager-Agent-Daten immer unter Verwendung von Verwaltungsgruppen an OMS gesendet. Windows-Agents umgehen die Verwaltungsgruppe und senden ihre Daten direkt an OMS.
> - Wenn die Operations Manager-Agent-Daten nicht unter Verwendung einer Verwaltungsgruppe gesendet werden, werden die Daten direkt (also unter Umgehung der Verwaltungsgruppe) an OMS gesendet.

### <a name="insight--analytics--log-analytics"></a>Insight & Analytics/Log Analytics

| Verwaltungslösung | Plattform | Microsoft Monitoring Agent | Operations Manager-Agent | Azure-Speicher | Operations Manager erforderlich? | Daten vom Operations Manager-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Aktivitätsprotokollanalyse | Azure |   |   |   |   |   | Bei Benachrichtigung |
| AD Assessment |Windows |&#8226; |&#8226; |  |  |&#8226; |7 Tage |
| AD Replication Status |Windows |&#8226; |&#8226; |  |  |&#8226; |5 Tage |
| Agent-Integrität | Windows und Linux | &#8226; | &#8226; |   |   | &#8226; | 1 Minute |
| Warnungsverwaltung (Nagios) |Linux |&#8226; |  |  |  |  |Bei Ankunft |
| Warnungsverwaltung (Zabbix) |Linux |&#8226; |  |  |  |  |1 Minute |
| Warnungsverwaltung (Operations Manager) |Windows |  |&#8226; |  |&#8226; |&#8226; |3 Minuten |
| Application Insights-Connector (Vorschau) | Azure |   |   |   |   |   | Bei Benachrichtigung |
| Azure Application Gateway-Analyse | Azure |   |   |   |   |   | Bei Benachrichtigung |
| Azure-Netzwerksicherheitsgruppen-Analyse | Azure |   |   |   |   |   | Bei Benachrichtigung |
| Azure SQL Analytics (Vorschau) |Windows |  |  |  |  |  | 10 Minuten |
| Capacity Management |Windows |&#8226; |&#8226; |  |  |&#8226; |Bei Ankunft |
| Container | Windows und Linux | &#8226; | &#8226; |   |   |   | 3 Minuten |
| Key Vault-Analysen |Windows |  |  |  |  |  |Bei Benachrichtigung |
| Netzwerkleistungsmonitor | Windows | &#8226; | &#8226; |   |   |   | TCP-Handshakes werden alle fünf Sekunden gesendet, Daten alle drei Minuten |
| Office 365-Analysen (Vorschau) |Windows |  |  |  |  |  |Bei Benachrichtigung |
| Service Fabric-Analysen |Windows |  |  |&#8226; |  |  |5 Minuten |
| Dienstzuordnung | Windows und Linux | &#8226; | &#8226; |   |   |   | 15 Sekunden |
| SQL Assessment |Windows |&#8226; |&#8226; |  |  |&#8226; |7 Tage |
| SurfaceHub |Windows |&#8226; |  |  |  |  |Bei Ankunft |
| System Center Operations Manager-Bewertung (Vorschau) | Windows | &#8226; | &#8226; |   |   | &#8226; | sieben Tage |
| Upgradeanalyse (Vorschau) | Windows | &#8226; |   |   |   |   | 2 Tage |
| VMware-Überwachung (Vorschau) | Linux | &#8226; |   |   |   |   | 3 Minuten |
| Wire Data |Windows (2012 R2 / 8.1 oder höher) |&#8226; |&#8226; |  |  |  | 1 Minute |


### <a name="automation--control"></a>Automation & Control

| Verwaltungslösung | Plattform | Microsoft Monitoring Agent | Operations Manager-Agent | Azure-Speicher | Operations Manager erforderlich? | Daten vom Operations Manager-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Automation Hybrid Worker | Windows | &#8226; | &#8226; |   |   |   | – |
| Change Tracking |Windows |&#8226; |&#8226; |  |  |&#8226; |Stündlich |
| Change Tracking |Linux |&#8226; |  |  |  |  |Stündlich |
| Updateverwaltung | Windows |&#8226; |&#8226; |  |  |&#8226; |Mindestens zweimal pro Tag und 15 Minuten nach Installation eines Updates |

### <a name="security--compliance"></a>Sicherheit und Compliance

| Verwaltungslösung | Plattform | Microsoft Monitoring Agent | Operations Manager-Agent | Azure-Speicher | Operations Manager erforderlich? | Daten vom Operations Manager-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Antischadsoftwarebewertung |Windows |&#8226; |&#8226; |  |  |&#8226; |Stündlich |
| Sicherheit und Überwachung<sup>1</sup> | Windows und Linux | Teilweise | Teilweise | Teilweise |   | Teilweise | Verschiedene |

<sup>1</sup> Die Lösung für Sicherheit und Überwachung kann Protokolle von Windows-, Operations Manager- und Linux-Agents erfassen. Unter [Datenquellen](#data-sources) finden Sie Datenerfassungsinformationen zu Folgendem:

- syslog
- Windows-Sicherheitsereignisprotokolle
- Windows-Firewallprotokolle
- Windows-Ereignisprotokolle



### <a name="protection--recovery"></a>Schutz und Wiederherstellung

| Verwaltungslösung | Plattform | Microsoft Monitoring Agent | Operations Manager-Agent | Azure-Speicher | Operations Manager erforderlich? | Daten vom Operations Manager-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Sicherung | Azure |   |   |   |   |   | – |
| Azure Site Recovery | Azure |   |   |   |   |   | – |


### <a name="data-sources"></a>Datenquellen


| Datenquelle | Plattform | Microsoft Monitoring Agent | Operations Manager-Agent | Azure-Speicher | Operations Manager erforderlich? | Daten vom Operations Manager-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Azure-Aktivitätsprotokolle |Windows |  |  |  |  |  |Bei Benachrichtigung |
| Azure-Diagnoseprotokolle |Windows |  |  |  |  |  |Bei Benachrichtigung |
| Azure-Diagnosemetriken |Windows |  |  |  |  |  |Bei Benachrichtigung |
| ETW |Windows |  |  |&#8226; |  |  |5 Minuten |
| IIS-Protokolle |Windows |&#8226; |&#8226; |&#8226; |  |  |5 Minuten |
| Leistungsindikatoren |Windows |&#8226; |&#8226; |  |  |  |Gemäß Zeitplan, mindestens 10 Sekunden |
| Leistungsindikatoren |Linux |&#8226; |  |  |  |  |Gemäß Zeitplan, mindestens 10 Sekunden |
| syslog |Linux |&#8226; |  |  |  |  |Von Azure-Speicher: 10 Minuten; von Agent: bei Ankunft |
| Windows-Sicherheitsereignisprotokolle |Windows |&#8226; |&#8226; |&#8226; |  |  |Für Azure-Speicher: 10 Minuten; für Agent: bei Ankunft |
| Windows-Firewallprotokolle |Windows |&#8226; |&#8226; |  |  |  |Bei Ankunft |
| Windows-Ereignisprotokolle |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; |Für Azure-Speicher: 10 Minuten; für Agent: bei Ankunft |



## <a name="preview-management-solutions-and-features"></a>Verwaltungslösungen und Features in der Vorschauversion
Durch Ausführen eines Diensts und Anwenden von DevOps-Methoden können wir als Partner mit Kunden zusammenarbeiten, um Features und Lösungen zu entwickeln.

Während der Phase der privaten Vorschau erhält eine kleine Gruppe von Kunden Zugriff auf eine frühe Implementierung des Features bzw. der Lösung, damit wir Feedback erhalten und Verbesserungen vornehmen können. Diese frühe Implementierung verfügt nur über minimale Features und betriebsbezogene Funktionen.

Das Ziel besteht darin, Dinge schnell auszuprobieren, um zu ermitteln, was funktioniert und was nicht funktioniert. Dieser Prozess wird durchlaufen, bis wir von den Kunden mit der privaten Vorschauversion das Feedback erhalten, dass alles für eine öffentliche Vorschauversion bereit ist.

Während der öffentlichen Vorschauphase stellen wir das Feature oder die Lösung für alle Benutzer zur Verfügung, um weiteres Feedback zu erhalten und die Skalierung und Effizienz zu überprüfen. Während dieser Phase gilt Folgendes:

* Vorschaufeatures werden auf der Registerkarte „Einstellungen“ angezeigt und können von jedem Benutzer aktiviert werden.
* Vorschaulösungen werden über den Katalog oder mithilfe eines Skripts hinzugefügt.

### <a name="what-should-i-know-about-preview-features-and-solutions"></a>Was sollte ich über Features und Lösungen der Vorschauversion wissen?
Wir freuen uns, die neuen Features und Verwaltungslösungen anbieten zu können, und wir arbeiten bei der Entwicklung gern mit Ihnen zusammen.

Vorschaufeatures und -lösungen sind allerdings nicht für jeden Benutzer geeignet. Stellen Sie also sicher, dass die Verwendung einer in der Entwicklung befindlichen Anwendung für Sie in Ordnung ist, bevor Sie die Nutzung einer privaten Vorschauversion in Erwägung ziehen oder eine öffentliche Vorschauversion aktivieren.

Wenn Sie ein Vorschaufeature über das Portal aktivieren, wird eine Warnung mit dem Hinweis angezeigt, dass sich das Feature in der Vorschauphase befindet.

#### <a name="for-both-private-and-public-preview"></a>Für *private* und *öffentliche* Vorschauversion
Die folgenden Informationen gelten sowohl für öffentliche als auch für private Vorschauversionen:

* Es kann sein, dass nicht immer alles einwandfrei funktioniert.
  * Probleme können von kleineren Unzulänglichkeiten bis zu Fehlern reichen, bei denen ein Feature gar nicht funktioniert.
* Es besteht die Möglichkeit, dass die Vorschauversion eine negative Auswirkung auf Ihr System bzw. Ihre Umgebung hat.
  * Wir versuchen zu vermeiden, dass sich für die Systeme, die Sie mit OMS verwenden, negative Auswirkungen ergeben, aber es kann sein, dass unerwartete Dinge passieren.
* Es kann zu Datenverlust oder zur Beschädigung von Daten kommen.
* Unter Umständen bitten wir Sie, Diagnoseprotokolle oder andere Daten als Beitrag zur Problembehandlung zu erfassen.
* Es kann sein, dass das Feature oder die Lösung entfernt wird (entweder vorübergehend oder dauerhaft).
  * Aufgrund der Erkenntnisse während der Vorschauphase kann es sein, dass das Feature bzw. die Lösung nicht veröffentlicht wird.
* Es kann sein, dass Vorschauversionen nicht mit allen Konfigurationen funktionieren bzw. nicht umfassend getestet wurden und dass Folgendes von uns eingeschränkt wird:
  * Die verwendbaren Betriebssysteme. (Beispielsweise sind manche Features in der Vorschauphase möglicherweise nur für Linux verfügbar.)
  * Die Art des verwendbaren Agents (MMA, Operations Manager). (Beispielsweise kann es in der Vorschauphase vorkommen, dass ein Feature nicht mit Operations Manager verwendet werden kann.)  
* Vorschaulösungen und -features sind nicht durch die Vereinbarung zum Servicelevel abgedeckt.
* Für die Nutzung von Vorschaufeatures fallen Nutzungsgebühren an.
* Features oder Funktionen, die Sie benötigen, damit das Feature bzw. die Lösung nützlich ist, können fehlen oder unvollständig sein.
* Features/Lösungen sind unter Umständen nicht in allen Regionen verfügbar.
* Features/Lösungen werden unter Umständen nicht lokalisiert.
* Für Features/Lösungen gilt ggf. eine Beschränkung bei der Anzahl von Kunden oder Geräten für die Nutzung.
* Unter Umständen müssen Sie Skripts verwenden, um die Konfiguration durchzuführen und die Lösung bzw. das Feature zu aktivieren.
* Die Benutzeroberfläche ist unvollständig und kann sich von Tag zu Tag ändern.
* Öffentliche Vorschauversionen sind für Ihre Produktionssysteme bzw. kritischen Systeme ggf. nicht geeignet.

#### <a name="for-private-preview"></a>Für *private* Vorschauversion
Zusätzlich zu den obigen Punkten gelten für private Vorschauversionen folgende Informationen:

* Wir erwarten, dass Sie uns Feedback zu Ihren Erfahrungen senden, damit wir das Feature bzw. die Lösung verbessern können.
* Unter Umständen wenden wir uns per Umfrage, Telefon oder E-Mail an Sie, um Feedback zu erhalten.
* Es funktioniert nicht immer alles einwandfrei.
* Unter Umständen ist ein Geheimhaltungsvertrag für die Teilnahme erforderlich, oder Inhalte werden als vertraulich gekennzeichnet.
  * Informieren Sie sich beim Programmmanager, der für die Vorschauversion verantwortlich ist, über geltende Beschränkungen zur Offenlegung, bevor Sie Bloggen, Tweeten oder auf andere Art mit Dritten kommunizieren.
* Keine Verwendung für Produktionssysteme oder kritische Systeme

### <a name="how-do-i-get-access-to-private-preview-features-and-solutions"></a>Wie erhalte ich Zugriff auf Features und Lösungen der privaten Vorschauversion?
Wir laden Kunden je nach Art der Version über verschiedene Wege zur Nutzung von privaten Vorschauversionen ein.

* Indem Sie an der monatlichen Kundenumfrage teilnehmen und uns die Erlaubnis erteilen, mit Ihnen Kontakt aufzunehmen, erhöhen Sie Ihre Chancen für eine Einladung zur privaten Vorschauversion.
* Sie können von Ihrem Microsoft-Kundenteam nominiert werden.
* Sie können sich über die bei Twitter geposteten Details anmelden ([msopsmgmt](https://twitter.com/msopsmgmt)).
* Sie können sich basierend auf den Details registrieren, die bei Communityveranstaltungen bereitgestellt werden. Halten Sie bei Treffen, Konferenzen und in Onlinecommunitys nach uns Ausschau.

## <a name="next-steps"></a>Nächste Schritte
* [Durchsuchen von Protokollen](log-analytics-log-searches.md), um detaillierte Informationen anzuzeigen, die von Verwaltungslösungen gesammelt wurden

