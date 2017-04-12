---
title: Integration in die Operations Management Suite (OMS) | Microsoft Docs
description: "Sie können nicht nur die Standardfunktionen der OMS verwenden, sondern die Suite auch in andere Verwaltungsanwendungen und -lösungen integrieren, um eine hybride Verwaltungsumgebung, benutzerdefinierte und speziell auf Ihre Umgebung zugeschnittene Verwaltungsszenarien oder eine benutzerdefinierte Verwaltungslösung für Ihre Kunden bereitzustellen.  Dieser Artikel bietet eine Übersicht über die verschiedenen Optionen für die Integration in die OMS sowie Links zu anderen Artikeln mit detaillierten technischen Informationen."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: fc5f3a8a-77f7-4103-bd7e-744c15ffcca7
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 31291a9b0f452adeae42f744a976c14e61a4b303
ms.lasthandoff: 11/17/2016


---
# <a name="integrating-with-operations-management-suite-oms"></a>Integration in die Operations Management Suite (OMS)
Die Operations Management Suite ist die cloudbasierte IT-Verwaltungslösung von Microsoft, die Ihnen das Verwalten und Schützen Ihrer lokalen und cloudbasierten Infrastruktur erleichtert.  Sie können nicht nur die Standardfunktionen der OMS verwenden, sondern die Suite auch in andere Verwaltungsanwendungen und -lösungen integrieren, um eine hybride Verwaltungsumgebung, benutzerdefinierte und speziell auf Ihre Umgebung zugeschnittene Verwaltungsszenarien oder eine benutzerdefinierte Verwaltungslösung für Ihre Kunden bereitzustellen.  Dieser Artikel bietet eine Übersicht über die verschiedenen Optionen für die Integration in die OMS-Dienste sowie Links zu anderen Artikeln mit detaillierten technischen Informationen. 

## <a name="log-analytics"></a>Log Analytics
Die von Log Analytics gesammelten Verwaltungsdatendaten werden in einem Repository gespeichert, das in Azure gehostet wird.  Alle im Repository gespeicherten Daten sind in Protokollsuchvorgängen verfügbar, sodass extrem große Datenmengen schnell analysiert werden können.  Je nach Integrationsanforderungen müssen Sie möglicherweise das Repository mit neuen Daten auffüllen, um diese für eine Analyse zur Verfügung zu stellen, oder Sie müssen Daten im Repository extrahieren, um eine neue Visualisierung zu ermöglichen oder die Daten in ein anderes Verwaltungstool zu integrieren.

Jedes Datenelement im Repository wird als Datensatz gespeichert.  Wenn Sie das Repository auffüllen, sollten Sie Ihren Benutzern den von Ihrer Lösung verwendeten Datensatztyp und eine Beschreibung der Eigenschaften des Typs bereitstellen.  Beim Abrufen werden diese Informationen benötigt, um mit den Daten arbeiten zu können.

![Auffüllen des OMS-Repositorys](media/operations-management-suite-integration/repository.png)

### <a name="populate-the-log-analytics-repository"></a>Auffüllen des Log Analytics-Repositorys
Es gibt verschiedene Methoden zum Auffüllen des OMS-Repositorys.  Welche Methode Sie verwenden, hängt von verschiedenen Faktoren ab, z.B. vom Speicherort der Quelldaten, dem Datenformat und den Clients, die Sie unterstützen müssen.  Sobald die Daten im Repository gespeichert sind, spielt es keine Rolle mehr, wie sie gesammelt wurden.

Die folgenden Abschnitte beschreiben die verschiedenen Optionen zum Auffüllen des OMS-Repositorys.

#### <a name="connected-sources-and-data-sources"></a>Verbundene Quellen und Datenquellen
Verbundene Quellen sind die Speicherorte, aus denen Daten für das OMS-Repository abgerufen werden können.  Datenquellen und Lösungen basieren auf verbundenen Quellen und definieren die spezifischen Daten, die gesammelt werden.  Wenn Ihre Anwendung Daten in eine dieser Datenquellen schreibt, können Sie die Daten sammeln, indem Sie die Datenquelle konfigurieren.  Wenn Ihre Anwendung z.B. Syslog-Ereignisse erstellt, können diese von der Syslog-Datenquelle auf einem Linux-Agent gesammelt werden.

* [Datenquellen in Log Analytics](../log-analytics/log-analytics-data-sources.md)

#### <a name="solutions"></a>Lösungen
Lösungen erweitern die Funktionen der OMS.  Eine Lösung kann Daten aus der verbundenen Quelle sammeln oder Datensätze analysieren, die bereits im Repository gesammelt wurden.  Für jede von Microsoft bereitgestellte Lösung gibt es einen separaten Artikel, der Details zu den von der Lösung gesammelten Daten bereitstellt.

* [Lösungen in Log Analytics](../log-analytics/log-analytics-add-solutions.md)

#### <a name="http-data-collector-api"></a>HTTP-Datensammler-API
Die HTTP-Datensammler-API von Log Analytics ist eine REST-API, die Ihnen das Hinzufügen von JSON-Daten zum Log Analytics-Repository ermöglicht.  Sie können diese API nutzen, wenn Sie über eine Anwendung verfügen, die Daten nicht über eine der anderen Datenquellen oder Lösungen bereitstellt.  Mit dieser API kann das Repository von jedem Client aufgefüllt werden, der eine API aufrufen kann, und die API ist nicht vom Sammlungszeitplan einer Datenquelle oder Lösung abhängig.

* [HTTP-Datensammler-API von Log Analytics](../log-analytics/log-analytics-data-collector-api.md)

### <a name="retrieve-data-from-the-log-analytics-repository"></a>Abrufen von Daten aus dem Log Analytics-Repository
Es gibt verschiedene Methoden zum Abrufen von Daten aus dem OMS-Repository.  Sie können Benutzern ermöglichen, Daten über die OMS-Konsole abzurufen, und ihnen verschiedene Arten von Visualisierungen und Analysen bereitstellen.  Daten können auch über einen externen Prozess wie z.B. eine andere Verwaltungslösung abgerufen werden.

#### <a name="log-searches"></a>Protokollsuchvorgänge
Alle im OMS-Repository gespeicherten Daten sind über Protokollsuchvorgänge verfügbar.  Benutzer können selbst Ad-hoc-Analysen in der OMS-Konsole durchführen oder ein Dashboard mit der Visualisierung eines bestimmten Protokollsuchvorgangs erstellen.  Lösungen können benutzerdefinierte Ansichten mit auf vordefinierten Suchvorgängen basierenden Visualisierungen enthalten.  Sie können die Protokollsuch-API verwenden, um über eine externe Anwendung oder ein externes Verwaltungstool auf Daten im OMS-Repository zuzugreifen.  

* [Protokollsuchvorgänge in Log Analytics](../log-analytics/log-analytics-log-searches.md)
* [Log Analytics-REST-API für die Protokollsuche](../log-analytics/log-analytics-log-search-api.md)
* [Log Analytics-Cmdlets](https://msdn.microsoft.com/library/mt188224.aspx)

#### <a name="custom-views"></a>Benutzerdefinierte Ansichten
Mit dem Ansicht-Designer können Sie benutzerdefinierte Ansichten in der OMS-Konsole erstellen, die es Benutzern ermöglichen, die Daten in Ihrer Lösung zu visualisieren und zu analysieren.  Jede Ansicht enthält eine Kachel, die auf der Hauptseite der Konsole angezeigt wird, sowie beliebig viele Visualisierungsdetails, die auf den von Ihnen definierten Protokollsuchvorgängen basieren.

* [Ansicht-Designer in Log Analytics](../log-analytics/log-analytics-view-designer.md)

#### <a name="power-bi"></a>Power BI
Log Analytics kann Daten automatisch aus dem OMS-Repository nach Power BI exportieren, sodass Sie die Visualisierungsfunktionen und Analysetools von Power BI nutzen können.  Dieser Export erfolgt nach einem Zeitplan, sodass die Daten auf dem neuesten Stand gehalten werden. 

* [Exportieren von Log Analytics-Daten nach Power BI](../log-analytics/log-analytics-powerbi.md)

## <a name="automation"></a>Automation
Die OMS kann Prozesse automatisieren, um auf gesammelte Daten zu reagieren oder andere Verwaltungsfunktionen auszuführen.  Mit solchen Prozessen können Sie Daten aus Ihrer Anwendung sammeln und in das OMS-Repository einfügen oder die Behebung eines bekannten Problems automatisieren, wenn bestimmte Daten im Repository gefunden werden. 

![Automation](media/operations-management-suite-integration/automate.png)

### <a name="runbooks"></a>Runbooks
Runbooks in Azure Automation führen PowerShell-Skripts und Workflows in der Azure-Cloud aus.  Sie können Runbooks verwenden, um Ressourcen in Azure oder andere Ressourcen zu verwalten, auf die aus Cloud zugegriffen werden kann.  Runbook können über einen Hybrid Runbook Worker auch in einem lokalen Rechenzentrum ausgeführt werden.  Sie können ein Runbook über das Azure-Portal oder über externe Prozesse starten und dabei verschiedene Methoden wie z.B. PowerShell oder die Automation-API verwenden.

* [Starten eines Runbooks in Azure Automation](../automation/automation-starting-a-runbook.md)
* [Azure Automation-Cmdlets](https://msdn.microsoft.com/library/dn690262.aspx)
* [Automation-REST-API](https://msdn.microsoft.com/library/mt662285.aspx)
* [Automation .NET](https://msdn.microsoft.com//library/mt465763.aspx)

### <a name="alerts"></a>Warnungen
Warnregeln führen Protokollsuchvorgänge automatisch nach einem bestimmten Zeitplan aus.  Wenn die Ergebnisse bestimmte Kriterien erfüllen, kann die resultierende Warnung ein Runbook in Azure Automation starten oder einen Webhook aufrufen, der einen externen Prozess startet.  In beiden Fällen können Informationen zur Warnung enthalten sein, einschließlich der von der Protokollsuche zurückgegebenen Daten.

* [Warnungen in Log Analytics](../log-analytics/log-analytics-alerts.md)
* [Log Analytics-Warnungs-API](../log-analytics/log-analytics-api-alerts.md)

## <a name="backup-and-site-recovery"></a>Backup und Site Recovery
Azure Backup und Site Recovery stellen Dienste bereit, mit denen Sie Ihre Unternehmensdaten schützen und die Verfügbarkeit von Servern und Anwendungen sicherstellen können.  Sie können diese Dienste für verschiedene Szenarien nutzen, z.B. zum Bereitstellen von Sicherungsdiensten für Ihre Anwendung oder zum Initiieren des Failovers eines virtuellen Computers.

* [Azure Backup-Cmdlets](https://msdn.microsoft.com/library/mt619253.aspx)
* [Azure Site Recovery-REST-API](https://msdn.microsoft.com/library/azure/mt750497.aspx)
* [Azure Site Recovery-Cmdlets](https://msdn.microsoft.com/library/mt637930.aspx)

## <a name="custom-solutions"></a>Benutzerdefinierte Lösungen
Sie können Integrationslogik in eine benutzerdefinierte Lösung kapseln, die in Ihrem Arbeitsbereich oder dem Arbeitsbereich eines Kunden ausgeführt wird.  Zusätzlich zu anderen Ressourcen kann Ihre Lösung jede der in diesem Artikel beschriebenen Integrationsmethoden enthalten und so ein vollständiges Verwaltungsszenario bereitstellen.  Die Ressourcen in der Lösung sind im Paket enthalten, sodass beim Entfernen der Lösung alle von ihr erstellten Ressourcen aus dem OMS-Arbeitsbereich und dem Azure-Abonnement entfernt werden.

Ihre Lösung kann beispielsweise ein Automation-Runbook enthalten, um mithilfe der HTTP-Datensammler-API Daten zu erfassen und zu verarbeiten und anschließend das Log Analytics-Repository mit den Daten aufzufüllen.  Sie können auch eine benutzerdefinierte Ansicht hinzufügen, in der die gesammelten Daten dargestellt und analysiert werden.  

* Erstellen benutzerdefinierter Lösungen (in Kürze verfügbar)    

## <a name="next-steps"></a>Nächste Schritte
* Technische Informationen zum Automatisieren von OMS-Diensten finden Sie im [OMS SDK](operations-management-suite-sdk.md).  


