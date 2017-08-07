---
title: "Lösung für die Agent-Integritätsdiagnose in OMS | Microsoft-Dokumentation"
description: "Dieser Artikel soll Ihnen einen besseren Einblick ermöglichen, wie Sie diese Lösung zum Überwachen der Integrität Ihrer Agents verwenden können, die Daten direkt an OMS oder System Center Operations Manager melden."
services: operations-management-suite
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: magoedte
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: b810e37e393ddab55500f636b72450789285a4f0
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---
#  <a name="agent-health-solution-in-oms"></a>Lösung für die Agent-Integritätsdiagnose in OMS
Mit der Lösung für die Agent-Integritätsdiagnose in OMS können Sie für alle Agents, die ihre Daten direkt an den OMS-Arbeitsbereich oder eine mit OMS verbundene System Center Operations Manager-Verwaltungsgruppe melden, die Agents ermitteln, die nicht mehr reagieren und Betriebsdaten übermitteln.  Außerdem können Sie nachverfolgen, wie viele Agents bereitgestellt werden und wie sie geografisch verteilt sind, und andere Abfragen durchführen, um zu ermitteln, wie Agents, die in Azure, anderen Cloudumgebungen oder lokal bereitgestellt wurden, verteilt sind.    

## <a name="prerequisites"></a>Voraussetzungen
Vergewissern Sie sich vor dem Bereitstellen dieser Lösung, dass Sie über derzeit unterstützte [Windows-Agents](../log-analytics/log-analytics-windows-agents.md) verfügen, die Daten an den OMS-Arbeitsbereich oder an eine [Operations Manager-Verwaltungsgruppe](../log-analytics/log-analytics-om-agents.md), die in Ihren OMS-Arbeitsbereich integriert ist, melden.    

## <a name="solution-components"></a>Lösungskomponenten
Diese Lösung besteht aus den folgenden Ressourcen, die Ihrem Arbeitsbereich hinzugefügt werden, und direkt verbundenen Agents oder mit Operations Manager verbundenen Verwaltungsgruppen.

### <a name="management-packs"></a>Management Packs
Wenn Ihre System Center Operations Manager-Verwaltungsgruppe mit einem OMS-Arbeitsbereich verbunden ist, werden in Operations Manager die folgenden Management Packs installiert.  Diese Management Packs werden nach dem Hinzufügen dieser Lösung auch auf direkt verbundenen Windows-Computern installiert. Für diese Management Packs fällt kein Konfigurations- oder Verwaltungsaufwand an.

* Microsoft System Center Advisor HealthAssessment Direct Channel Intelligence Pack (Microsoft.IntelligencePacks.HealthAssessmentDirect)
* Microsoft System Center Advisor HealthAssessment Server Channel Intelligence Pack (Microsoft.IntelligencePacks.HealthAssessmentViaServer).  

Weitere Informationen zur Aktualisierung von Management Packs finden Sie unter [Herstellen einer Verbindung zwischen Operations Manager und Log Analytics](../log-analytics/log-analytics-om-agents.md).

## <a name="configuration"></a>Konfiguration
Fügen Sie die Lösung für die Agent-Integritätsdiagnose dem OMS-Arbeitsbereich hinzu, indem Sie den unter [Hinzufügen von Lösungen](../log-analytics/log-analytics-add-solutions.md) beschriebenen Prozess verwenden. Es ist keine weitere Konfiguration erforderlich.


## <a name="data-collection"></a>Datensammlung
### <a name="supported-agents"></a>Unterstützte Agents
In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von der Lösung unterstützt werden.

| Verbundene Quelle | Unterstützt | Beschreibung |
| --- | --- | --- |
| Windows-Agents | Ja | Heartbeat-Ereignisse werden von direkten Windows-Agents erfasst.|
| System Center Operations Manager-Verwaltungsgruppe | Ja | Heartbeat-Ereignisse werden von Agents, die ihre Daten an die Verwaltungsgruppe melden, alle 60 Sekunden erfasst und dann an Log Analytics weitergeleitet. Es ist keine direkte Verbindung von Operations Manager-Agents mit Log Analytics erforderlich. Daten von Heartbeat-Ereignissen werden von der Verwaltungsgruppe an das Log Analytics-Repository weitergeleitet.|

## <a name="using-the-solution"></a>Verwenden der Lösung
Wenn Sie die Lösung dem OMS-Arbeitsbereich hinzufügen, wird Ihrem OMS-Dashboard die Kachel **Agent-Integrität** hinzugefügt. Auf dieser Kachel werden die Gesamtzahl von Agents und die Anzahl von nicht mehr reagierenden Agents innerhalb der letzten 24 Stunden angezeigt.<br><br> ![Kachel für die Lösung zur Agent-Integritätsdiagnose im Dashboard](./media/oms-solution-agenthealth/agenthealth-solution-tile-homepage.png)

Klicken Sie auf die Kachel **Agent-Integrität**, um das Dashboard **Agent-Integrität** zu öffnen.  Das Dashboard enthält die Spalten, die in der folgenden Tabelle angegeben sind. In jeder Spalte sind die zehn häufigsten Ereignisse entsprechend den Kriterien der Spalte für den angegebenen Zeitbereich aufgeführt. Sie können eine Protokollsuche durchführen, mit der die gesamte Liste ausgegeben wird, indem Sie rechts unten in jeder Spalte die Option **Alle anzeigen** wählen oder auf die Spaltenüberschrift klicken.

| Column | Beschreibung |
|--------|-------------|
| Agent-Anzahl über die Zeit | Ein Trend der Agent-Anzahl für einen Zeitraum von sieben Tagen für Linux- und Windows-Agents.|
| Anzahl der nicht reagierenden Agents | Eine Liste mit Agents, die in den letzten 24 Stunden keinen Heartbeat gesendet haben.|
| Verteilung der Agents nach Betriebssystemtyp | Eine Partition dazu, über wie viele Windows- und Linux-Agents Sie in Ihrer Umgebung verfügen.|
| Verteilung der Agents nach Version | Eine Partition mit den unterschiedlichen Agent-Versionen, die in Ihrer Umgebung installiert sind, und der jeweiligen Anzahl.|
| Verteilung der Agents nach Agent-Kategorie | Eine Partition mit den unterschiedlichen Agent-Kategorien, die Heartbeat-Ereignisse senden: direkte Agents, OpsMgr-Agents oder der OpsMgr Management Server.|
| Verteilung nach Verwaltungsgruppe | Eine Partition mit den unterschiedlichen SCOM-Verwaltungsgruppen in Ihrer Umgebung.|
| Geografischer Standort der Agents | Eine Partition mit den unterschiedlichen Ländern, in denen Sie über Agents verfügen, und die Gesamtzahl von Agents, die in einem Land jeweils installiert sind.|
| Anzahl installierter Gateways | Die Anzahl von Servern, auf denen das OMS-Gateway installiert ist, und eine Liste mit diesen Servern.|

![Beispiel: Dashboard für die Lösung zur Agent-Integritätsdiagnose](./media/oms-solution-agenthealth/agenthealth-solution-dashboard.png)  

## <a name="log-analytics-records"></a>Log Analytics-Datensätze
Die Lösung erstellt im OMS-Repository eine Art von Datensatz.  

### <a name="heartbeat-records"></a>Heartbeat-Datensätze
Ein Datensatz vom Typ **Heartbeat** wird erstellt.  Die Eigenschaften der Datensätze sind in der folgenden Tabelle aufgeführt.  

| Eigenschaft | Beschreibung |
| --- | --- |
| Typ | *Heartbeat*|
| Kategorie | Der Wert lautet *Direct Agent*, *SCOM Agent* oder *SCOM Management Server*.|
| Computer | Name des Computers|
| OSType | Windows- oder Linux-Betriebssystem|
| OSMajorVersion | Hauptversion des Betriebssystems|
| OSMinorVersion | Nebenversion des Betriebssystems|
| Version | OMS-Agent- oder Operations Manager-Agent-Version|
| SCAgentChannel | Der Wert lautet *Direct* bzw. *SCManagementServer*.|
| IsGatewayInstalled | Wenn das OMS-Gateway installiert ist, lautet der Wert *true*, andernfalls *false*.|
| ComputerIP | IP-Adresse des Computers|
| RemoteIPCountry | Geografischer Standort, an dem der Computer bereitgestellt wird|
| ManagementGroupName | Name der Operations Manager-Verwaltungsgruppe|
| SourceComputerId | Eindeutige ID des Computers|
| RemoteIPLongitude | Längengrad des geografischen Standorts des Computers|
| RemoteIPLatitude | Breitengrad des geografischen Standorts des Computers|

Jeder Agent, der Daten an einen Operations Manager-Verwaltungsserver meldet, sendet zwei Heartbeats, und der Wert der SCAgentChannel-Eigenschaft kann **Direct** oder **SCManagementServer** lauten. Dies richtet sich danach, welche Log Analytics-Datenquellen und -Lösungen Sie in Ihrem OMS-Abonnement aktiviert haben. Wie Sie wissen, werden Daten aus Lösungen entweder direkt von einem Operations Manager-Verwaltungsserver oder (aufgrund des Umfangs der Daten, die auf dem Agent gesammelt werden) direkt vom Agent an den OMS-Webdienst gesendet. Für Heartbeat-Ereignisse, die über den Wert **SCManagementServer** verfügen, ist der Wert „ComputerIP“ die IP-Adresse des Verwaltungsservers, da die Daten tatsächlich darüber hochgeladen werden.  Für Heartbeats, für die SCAgentChannel auf **Direkt** festgelegt ist, ist dies die öffentliche IP-Adresse des Agents.  

## <a name="sample-log-searches"></a>Beispiele für Protokollsuchen
Die folgende Tabelle enthält Beispiele für Protokollsuchen für Datensätze, die mit dieser Lösung erfasst wurden.

| Abfrage | Beschreibung |
| --- | --- |
| Type=Heartbeat &#124; distinct Computer |Gesamtanzahl von Agents |
| Type=Heartbeat &#124; measure max(TimeGenerated) as LastCall by Computer &#124; where LastCall < NOW-24HOURS |Anzahl der nicht reagierenden Agents innerhalb der letzten 24 Stunden |
| Type=Heartbeat &#124; measure max(TimeGenerated) as LastCall by Computer &#124; where LastCall < NOW-15MINUTES |Anzahl der nicht reagierenden Agents innerhalb der letzten 15 Minuten |
| Type=Heartbeat TimeGenerated>NOW-24HOURS Computer IN {Type=Heartbeat TimeGenerated>NOW-24HOURS &#124; distinct Computer} &#124; measure max(TimeGenerated) as LastCall by Computer |Computer im Onlinezustand (in den letzten 24 Stunden) |
| Type=Heartbeat TimeGenerated>NOW-24HOURS Computer NOT IN {Type=Heartbeat TimeGenerated>NOW-30MINUTES &#124; distinct Computer} &#124; measure max(TimeGenerated) as LastCall by Computer |Gesamtzahl der Agents im Offlinezustand in den letzten 30 Minuten (innerhalb der letzten 24 Stunden) |
| Type=Heartbeat &#124; measure countdistinct(Computer) by OSType |Abrufen eines Trends zur Anzahl von Agents in Abhängigkeit der Zeit nach Betriebssystemtyp|
| Type=Heartbeat&#124;measure countdistinct(Computer) by OSType |Verteilung der Agents nach Betriebssystemtyp |
| Type=Heartbeat&#124;measure countdistinct(Computer) by Version |Verteilung der Agents nach Version |
| Type=Heartbeat&#124;measure count() by Category |Verteilung der Agents nach Agent-Kategorie |
| Type=Heartbeat&#124;measure countdistinct(Computer) by ManagementGroupName | Verteilung nach Verwaltungsgruppe |
| Type=Heartbeat&#124;measure countdistinct(Computer) by RemoteIPCountry |Geografischer Standort der Agents |
| Type=Heartbeat IsGatewayInstalled=true&#124;Distinct Computer |Anzahl von installierten OMS-Gateways |


>[!NOTE]
> Falls für Ihren Arbeitsbereich ein Upgrade auf die [neue Log Analytics-Abfragesprache](../log-analytics/log-analytics-log-search-upgrade.md) durchgeführt wurde, müssen die obigen Abfragen wie folgt geändert werden:
>
>| Abfrage | Beschreibung |
|:---|:---|
| Heartbeat &#124; distinct Computer |Gesamtanzahl von Agents |
| Heartbeat &#124; summarize LastCall = max(TimeGenerated) by Computer &#124; where LastCall < ago(24h) |Anzahl der nicht reagierenden Agents innerhalb der letzten 24 Stunden |
| Heartbeat &#124; summarize LastCall = max(TimeGenerated) by Computer &#124; where LastCall < ago(15m) |Anzahl der nicht reagierenden Agents innerhalb der letzten 15 Minuten |
| Heartbeat &#124; where TimeGenerated > ago(24h) and Computer in ((Heartbeat &#124; where TimeGenerated > ago(24h) &#124; distinct Computer)) &#124; summarize LastCall = max(TimeGenerated) by Computer |Computer im Onlinezustand (in den letzten 24 Stunden) |
| Heartbeat &#124; where TimeGenerated > ago(24h) and Computer !in ((Heartbeat &#124; where TimeGenerated > ago(30m) &#124; distinct Computer)) &#124; summarize LastCall = max(TimeGenerated) by Computer |Gesamtzahl der Agents im Offlinezustand in den letzten 30 Minuten (innerhalb der letzten 24 Stunden) |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by OSType |Abrufen eines Trends zur Anzahl von Agents in Abhängigkeit der Zeit nach Betriebssystemtyp|
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by OSType |Verteilung der Agents nach Betriebssystemtyp |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by Version |Verteilung der Agents nach Version |
| Heartbeat &#124; summarize AggregatedValue = count() by Category |Verteilung der Agents nach Agent-Kategorie |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by ManagementGroupName | Verteilung nach Verwaltungsgruppe |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by RemoteIPCountry |Geografischer Standort der Agents |
| Heartbeat &#124; where iff(isnotnull(toint(IsGatewayInstalled)), IsGatewayInstalled == true, IsGatewayInstalled == "true") == true &#124; distinct Computer |Anzahl von installierten OMS-Gateways |

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie sich die Details zum Generieren von Warnungen aus Log Analytics unter [Warnungen in Log Analytics](../log-analytics/log-analytics-alerts.md) durch.

