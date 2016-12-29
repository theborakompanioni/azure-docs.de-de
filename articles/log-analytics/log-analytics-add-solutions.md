---
title: "Hinzufügen von Log Analytics-Verwaltungslösungen | Microsoft-Dokumentation"
description: "Log Analytics-Verwaltungslösungen sind eine Sammlung von Logik-, Visualisierungs- und Datenerfassungsregeln, die Metriken rund um einen bestimmten Problembereich bereitstellen."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: jwhit
editor: 
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: 6cdc0730d7632e41b393c4abb17badc255e21a8d
ms.openlocfilehash: d036717661c252336ec0d747d6176d7b32913afe


---
# <a name="add-log-analytics-management-solutions"></a>Hinzufügen von Log Analytics-Verwaltungslösungen

Log Analytics-Verwaltungslösungen sind eine Sammlung von **Logik**-, **Visualisierungs**- und **Datenerfassungsregeln**, die Metriken rund um einen bestimmten Problembereich bereitstellen. In diesem Artikel sind von Log Analytics unterstützte Verwaltungslösungen aufgeführt. Darüber hinaus erfahren Sie, wie Sie mithilfe des Azure-Portals einen Arbeitsbereich hinzufügen und entfernen. Sie können auch Lösungen im OMS-Portal mithilfe des Lösungskatalogs hinzufügen.

Die detaillierten Einblicke der Verwaltungslösungen ermöglichen Folgendes:

* Schnelleres Untersuchen und Lösen von Betriebsproblemen
* Sammeln und Korrelieren von verschiedenen Arten von Computerdaten
* Proaktive Behandlung von Aktivitäten, die von der Lösung verfügbar gemacht werden

> [!NOTE]
> Log Analytics verfügt über eine integrierte Protokollsuche, sodass nicht extra eine Verwaltungslösung installiert werden muss. Sie können jedoch Datenvisualisierungen, Vorschläge für Suchen und Einblicke nutzen, indem Sie Verwaltungslösungen zu Ihrem Arbeitsbereich hinzufügen.

In diesem Artikel fügen Sie mit dem Marketplace im Azure-Portal einem Arbeitsbereich Verwaltungslösungen hinzu. Nachdem Sie eine Lösung hinzugefügt haben, werden Daten von den Servern in Ihrer Infrastruktur erfasst und an den OMS-Dienst gesendet. Die Verarbeitung durch den OMS-Dienst dauert normalerweise von einigen Minuten bis zu einer Stunde. Nach der Verarbeitung der Daten durch den Dienst können Sie diese in OMS anzeigen.

Wenn eine Verwaltungslösung nicht mehr benötigt wird, können Sie sie problemlos entfernen. Wenn Sie eine Verwaltungslösung entfernen, werden ihre Daten nicht mehr an OMS gesendet, und die Datenmenge, die Ihr Tageskontingent belastet (sofern vorhanden), verringert sich entsprechend.

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


## <a name="data-collection-details"></a>Details zur Datensammlung
Die folgende Tabelle zeigt die Datensammlungsmethoden und andere Details dazu, wie Daten für Log Analytics-Verwaltungslösungen und Datenquellen erfasst werden. Die Tabellen sind nach Lösungsangeboten kategorisiert, die den [Abonnementtarifen](https://go.microsoft.com/fwlink/?linkid=827926) entsprechen. Die Lösung für die Aktivitätsprotokollanalyse ist in allen Tarifen kostenlos.

Windows-Agents und SCOM-Agents sind nahezu identisch. Windows-Agents stellen jedoch zusätzliche Funktionen bereit, die eine Verbindung mit dem OMS-Arbeitsbereich und die Weiterleitung über einen Proxy erlauben. Wenn Sie einen SCOM-Agent verwenden, muss er als Ziel wie ein OMS-Agent konfiguriert werden, damit eine Kommunikation mit OMS möglich ist. Die SCOM-Agents in dieser Tabelle sind mit SCOM verbunden. Weitere Informationen zum Verbinden Ihrer vorhandenen SCOM-Umgebung mit OMS finden Sie unter [Verbinden von Operations Manager mit Log Analytics](log-analytics-om-agents.md).

> [!NOTE]
> Der Typ des verwendeten Agents bestimmt, wie Daten an OMS gesendet werden. Dabei gelten die folgenden Bedingungen:
> - Sie verwenden den Windows-Agent oder einen mit SCOM verbundenen OMS-Agent.
> - Wenn SCOM erforderlich ist, werden SCOM-Agent-Daten für die Lösung immer mithilfe der SCOM-Verwaltungsgruppe an OMS gesendet. Wenn SCOM erforderlich ist, verwendet die Lösung darüber hinaus den SCOM-Agent.
> - Wenn SCOM nicht erforderlich ist und die Tabelle zeigt, dass Daten vom SCOM-Agent mithilfe der Verwaltungsgruppe an OMS gesendet werden, werden SCOM-Agent-Daten immer mithilfe von Verwaltungsgruppen an OMS gesendet. Windows-Agents umgehen die Verwaltungsgruppe und senden ihre Daten direkt an OMS.
> - Wenn die SCOM-Agent-Daten nicht mit einer Verwaltungsgruppe gesendet werden, erfolgt ein direkter Versand an OMS – unter Umgehung der Verwaltungsgruppe.

### <a name="insight--analytics-or-log-analytics-standalone-per-gigabyte"></a>Insight & Analytics oder Log Analytics – Standalone (pro Gigabyte)

| Lösung | Plattform | Windows-Agent | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Aktivitätsprotokollanalyse | Azure | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | Bei Benachrichtigung |
| AD Assessment |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |7 Tage |
| AD Replication Status |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |5 Tage |
| Agent-Integrität | Windows und Linux | ![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) | ![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) | 1 Minute |
| Warnungsverwaltung (Nagios) | Linux |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |Bei der Ankunft |
| Warnungsverwaltung (Zabbix) | Linux |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |1 Minute |
| Warnungsverwaltung (Operations Manager) |Windows |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |3 Minuten |
| Application Insights-Connector (Vorschau) | Azure | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | Bei Benachrichtigung |
| Azure Network Analytics (Vorschau) | Azure | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | 10 Minuten |
| Kapazitätsverwaltung<sup>1</sup> |Windows |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |Stündlich |
| Container |  Linux | ![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | 3 Minuten |
| Key Vault-Analysen (Vorschau) |Windows |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |10 Minuten |
| Netzwerkleistungsmonitor | Windows | ![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) | ![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | TCP-Handshakes werden alle fünf Sekunden gesendet, Daten alle drei Minuten |
| Office 365-Analysen (Vorschau) |Windows |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |Bei Benachrichtigung |
| Service Fabric-Analysen |Windows |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |5 Minuten |
| Dienstzuordnung | Windows und Linux | ![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) | ![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) | 15 Sekunden |
| SQL Assessment |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |7 Tage |
| SurfaceHub |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |Bei der Ankunft |
| System Center Operations Manager-Bewertung (Vorschau) | Windows | ![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) | ![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) | sieben Tage |
| Upgradeanalyse (Vorschau) | Windows | ![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | 2 Tage |
| VMware-Überwachung (Vorschau) |  Linux | ![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | 3 Minuten |
| Wire Data<sup>2</sup> |Windows (2012 R2 / 8.1 oder höher) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | 1 Minute |


<sup>1</sup>Die Kapazitätsverwaltungslösung kann keinen Arbeitsbereichen hinzugefügt werden. Kunden, die die Kapazitätsverwaltungslösung installiert haben, können die Lösung weiterhin verwenden.

<sup>2</sup>Die Wire Data-Lösung kann derzeit keinen Arbeitsbereichen hinzugefügt werden. Kunden, für die die Wire Data-Lösung bereits aktiviert wurde, können diese Lösung weiterhin verwenden.


### <a name="automation--control"></a>Automation & Control

| Lösung | Plattform | Windows-Agent | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Aktivitätsprotokollanalyse | Azure | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | Bei Benachrichtigung |
| Automation Hybrid Worker | Windows | ![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) | ![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | – |
| Change Tracking |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |Stündlich |
| Change Tracking |Linux |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |Stündlich |
| Updateverwaltung | Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |Mindestens zweimal pro Tag und 15 Minuten nach Installation eines Updates |

### <a name="security--compliance"></a>Sicherheit und Compliance

| Lösungstyp | Plattform | Windows-Agent | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Aktivitätsprotokollanalyse | Azure | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | Bei Benachrichtigung |
| Antischadsoftwarebewertung |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |Stündlich |
| Sicherheit und Überwachung<sup>1</sup> | Windows und Linux | ![Einige](./media/log-analytics-add-solutions/oms-bullet-yellow.png) | ![Einige](./media/log-analytics-add-solutions/oms-bullet-yellow.png) | ![Einige](./media/log-analytics-add-solutions/oms-bullet-yellow.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Einige](./media/log-analytics-add-solutions/oms-bullet-yellow.png) | Verschiedene |

<sup>1</sup>Die Lösung für Sicherheit und Überwachung kann Protokolle von Windows-, SCOM- und Linux-Agents erfassen. Unter [Datenquellen](#data-sources) weiter unten finden Sie Datenerfassungsinformationen zu folgenden Elementen:

- syslog
- Windows-Sicherheitsereignisprotokolle
- Windows-Firewallprotokolle
- Windows-Ereignisprotokolle



### <a name="protection--recovery"></a>Schutz und Wiederherstellung

| Lösung | Plattform | Windows-Agent | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Aktivitätsprotokollanalyse | Azure | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | Bei Benachrichtigung |
| Sicherung | Azure | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | – |
| Azure Site Recovery | Azure | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | ![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) | – |


### <a name="data-sources"></a>Datenquellen


| Datenquelle | Plattform | Windows-Agent | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ETW |Windows |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |5 Minuten |
| IIS-Protokolle |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |5 Minuten |
| Netzwerk-Anwendungsgateways |Windows |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |10 Minuten |
| Netzwerksicherheitsgruppen |Windows |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |10 Minuten |
| Leistungsindikatoren |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |Gemäß Zeitplan, mindestens 10 Sekunden |
| Leistungsindikatoren |Linux |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |Gemäß Zeitplan, mindestens 10 Sekunden |
| syslog |Linux |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |Von Azure-Speicher: 10 Minuten; von Agent: bei Ankunft |
| Windows-Sicherheitsereignisprotokolle |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |Für Azure-Speicher: 10 Minuten; für Agent: bei Ankunft |
| Windows-Firewallprotokolle |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |Bei Ankunft |
| Windows-Ereignisprotokolle |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |Für Azure-Speicher: 1 Minute; für Agent: bei Ankunft |



## <a name="preview-management-solutions-and-features"></a>Verwaltungslösungen und Features in der Vorschauversion
Indem wir einen Dienst ausführen und DevOps-Methoden anwenden, können wir als Partner mit Kunden zusammenarbeiten, um Features und Lösungen zu entwickeln.

Während der Phase der privaten Vorschau erhält eine kleine Gruppe von Kunden Zugriff auf eine frühe Implementierung des Features bzw. der Lösung, damit wir Feedback erhalten und Verbesserungen einbauen können. Diese frühe Implementierung verfügt nur über minimale Features und betriebsbezogene Funktionen.

Das Ziel besteht darin, Dinge schnell auszuprobieren, um zu ermitteln, was funktioniert und was nicht funktioniert. Dieser Prozess wird durchlaufen, bis wir von den Kunden mit der privaten Vorschauversion das Feedback erhalten, dass alles für eine öffentliche Vorschauversion bereit ist.

Während der öffentlichen Vorschauphase stellen wir das Feature oder die Lösung für alle Benutzer zur Verfügung, um weiteres Feedback zu erhalten und die Skalierung und Effizienz zu überprüfen. Während dieser Phase gilt Folgendes:

* Vorschaufeatures werden auf der Registerkarte „Einstellungen“ angezeigt und können von jedem Benutzer aktiviert werden.
* Vorschaulösungen können über den Katalog oder mit einem veröffentlichten Skript hinzugefügt werden.

### <a name="what-should-i-know-about-preview-features-and-solutions"></a>Was sollte ich über Features und Lösungen der Vorschauversion wissen?
Wir freuen uns, die neuen Features und Verwaltungslösungen anbieten zu können, und wir arbeiten bei der Entwicklung gern mit Ihnen zusammen.

Vorschaufeatures und -lösungen sind aber nicht für jeden Benutzer geeignet. Stellen Sie also sicher, dass die Verwendung einer in der Entwicklung befindlichen Anwendung für Sie in Ordnung ist, bevor Sie die Nutzung einer privaten Vorschauversion beantragen oder eine öffentliche Vorschau aktivieren.

Wenn Sie ein Vorschaufeature über das Portal aktivieren, wird eine Warnung mit dem Hinweis angezeigt, dass sich das Feature in der Vorschauphase befindet.

#### <a name="for-both-private-and-public-preview"></a>Für *private* und *öffentliche* Vorschauversion
Folgendes gilt sowohl für öffentliche als auch für private Vorschauversionen:

* Es kann sein, dass nicht immer alles einwandfrei funktioniert.
  * Probleme können von kleineren Unzulänglichkeiten bis zu Fehlern reichen, bei denen ein Feature gar nicht funktioniert.
* Es besteht die Möglichkeit, dass die Vorschauversion eine negative Auswirkung auf Ihr System bzw. Ihre Umgebung hat.
  * Wir versuchen zu vermeiden, dass sich für die Systeme, die Sie mit OMS verwenden, negative Auswirkungen ergeben, aber es kann sein, dass unerwartete Dinge passieren.
* Es kann zu Datenverlust oder zur Beschädigung von Daten kommen.
* Unter Umständen bitten wir Sie, Diagnoseprotokolle oder andere Daten als Beitrag zur Problembehandlung zu erfassen.
* Es kann sein, dass das Feature oder die Lösung entfernt wird (entweder vorübergehend oder dauerhaft).
  * Aufgrund der Erkenntnisse während der Vorschauphase kann es sein, dass das Feature bzw. die Lösung nicht veröffentlicht wird.
* Es kann sein, dass Vorschauversionen nicht mit allen Konfigurationen funktionieren bzw. nicht umfassend getestet wurden und dass Folgendes von uns eingeschränkt wird:
  * Zulässige Betriebssysteme (z.B. kann ein Feature in der Vorschauphase nur für Linux gelten)
  * Zulässiger Agent-Typ (MMA, SCOM) (z.B. funktioniert ein Feature in der Vorschauphase nicht mit SCOM)  
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
Zusätzlich zu den obigen Punkten gilt für private Vorschauversionen Folgendes:

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



<!--HONumber=Nov16_HO5-->


