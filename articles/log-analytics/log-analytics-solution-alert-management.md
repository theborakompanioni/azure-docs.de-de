---
title: "Alert Management-Lösung in der Operations Management Suite (OMS) | Microsoft Docs"
description: "Die Alert Management-Lösung in Log Analytics unterstützt Sie beim Analysieren aller Warnungen in Ihrer Umgebung.  Zusätzlich zur Konsolidierung von in OMS generierten Warnungen werden Warnungen aus verbundenen Verwaltungsgruppen von System Center Operations Manager (SCOM) in Log Analytics importiert."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: fe5d534e-0418-4e2f-9073-8025e13271a8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/07/2017
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: 885de1e94e3ce520621dc8dc7a4a495501f6a429
ms.openlocfilehash: 35b4c30de20c46312bd7e4524a4264450184138a
ms.lasthandoff: 02/18/2017


---
# <a name="alert-management-solution-in-operations-management-suite-oms"></a>Alert Management-Lösung in der Operations Management Suite (OMS)

![Symbol „Alert Management“](media/log-analytics-solution-alert-management/icon.png)

Die Alert Management-Lösung unterstützt Sie beim Analysieren aller Warnungen in Ihrem Log Analytics-Repository.  Diese Warnungen können aus einer Vielzahl von Quellen stammen, einschließlich der [von Log Analytics erstellten](log-analytics-alerts.md) oder [aus Nagios oder Zabbix importierten](log-analytics-linux-agents.md#linux-alerts).  Die Lösung importiert auch Warnungen aus jeder [verbundenen System Center Operations Manager-Verwaltungsgruppe (SCOM)](log-analytics-om-agents.md).

## <a name="prerequisites"></a>Voraussetzungen
Die Lösung funktioniert mit den Einträgen im Log Analytics-Repository mit einem Typ der **Warnung**, sodass Sie die jeweilige erforderliche Konfiguration ausführen müssen, um diese Datensätze zu sammeln.

- Für Log Analytics-Warnungen müssen Sie [Warnungsregeln erstellen](log-analytics-alerts.md), um Warnungsdatensätze direkt im Repository zu erstellen.
- Für Warnungen von Nagios und Zabbix müssen Sie [diese Server konfigurieren](log-analytics-linux-agents.md#linux-alerts), sodass sie Warnungen an Log Analytics senden.
- Für SCOM-Warnungen müssen Sie [Ihre Operations Manager-Verwaltungsgruppe mit Ihrem Log Analytics-Arbeitsbereich verbinden](log-analytics-om-agents.md).  Alle in SCOM erstellten Warnungen werden in Log Analytics importiert.  

## <a name="configuration"></a>Konfiguration
Fügen Sie die Alert Management-Lösung dem OMS-Arbeitsbereich hinzu, indem Sie den unter [Hinzufügen von Lösungen](log-analytics-add-solutions.md)beschriebenen Prozess verwenden.  Es ist keine weitere Konfiguration erforderlich.

## <a name="management-packs"></a>Management Packs
Falls Ihre SCOM-Verwaltungsgruppe mit dem OMS-Arbeitsbereich verbunden ist, werden in SCOM die folgenden Management Packs installiert, wenn Sie diese Lösung hinzufügen.  Für diese Management Packs ist keine Konfiguration oder Wartung erforderlich.  

* Microsoft System Center Advisor Alert Management (Microsoft.IntelligencePacks.AlertManagement)

Weitere Informationen zur Aktualisierung von Management Packs finden Sie unter [Herstellen einer Verbindung zwischen Operations Manager und Log Analytics](log-analytics-om-agents.md).

## <a name="data-collection"></a>Datensammlung
### <a name="agents"></a>Agents
In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von der Lösung unterstützt werden.

| Verbundene Quelle | Support | Beschreibung |
|:--- |:--- |:--- |
| [Windows-Agents](log-analytics-windows-agents.md) | Nein |Direkte Windows-Agents generieren keine Warnungen.  Log Analytics-Warnungen können aus Ereignissen und Leistungsdaten erstellt werden, die von Windows-Agents gesammelt wurden. |
| [Linux-Agents](log-analytics-linux-agents.md) | Nein |Direkte Linux-Agents generieren keine Warnungen.  Log Analytics-Warnungen können aus Ereignissen und Leistungsdaten erstellt werden, die von Linux-Agents gesammelt wurden.  Nagios- und Zabbix-Warnungen werden von diesen Servern gesammelt, die den Linux-Agent erfordern. |
| [SCOM-Verwaltungsgruppe](log-analytics-om-agents.md) |Ja |Warnungen, die auf SCOM-Agents generiert werden, werden für die Verwaltungsgruppe und dann an Log Analytics weitergeleitet.<br><br>Eine direkte Verbindung zwischen SCOM-Agents und Log Analytics ist nicht erforderlich. Warnungsdaten werden von der Verwaltungsgruppe an das Log Analytics-Repository weitergeleitet. |


### <a name="collection-frequency"></a>Sammlungshäufigkeit
- Warnungsdatensätze stehen der Lösung zur Verfügung, sobald sie im Repository gespeichert werden.
- Warnungsdaten werden alle drei Minuten von der SCOM-Verwaltungsgruppe an Log Analytics gesendet.  

## <a name="using-the-solution"></a>Verwenden der Lösung
Wenn Sie dem OMS-Arbeitsbereich die Alert Management-Lösung hinzufügen, wird Ihrem OMS-Dashboard die Kachel **Alert Management** (Alert Management) hinzugefügt.  Auf dieser Kachel werden die Anzahl und eine grafische Darstellung der Anzahl von derzeit aktiven Warnungen angezeigt, die innerhalb der letzten 24 Stunden generiert wurden.  Sie können diesen Zeitraum nicht ändern.

![Kachel „Alert Management“](media/log-analytics-solution-alert-management/tile.png)

Klicken Sie auf die Kachel **Alert Management**, um das Dashboard **Alert Management** zu öffnen.  Das Dashboard enthält die Spalten, die in der folgenden Tabelle angegeben sind.  In jeder Spalte sind die zehn wichtigsten Warnungen nach ihrer Anzahl aufgeführt. Dies richtet sich jeweils nach den Spaltenkriterien für den angegebenen Bereich und Zeitraum.  Sie können eine Protokollsuche durchführen, mit der die gesamte Liste ausgegeben wird, indem Sie unten in der Spalte auf **Alle anzeigen** oder auf die Spaltenüberschrift klicken.

| Column | Beschreibung |
|:--- |:--- |
| Kritische Warnungen |Alle Warnungen mit dem Schweregrad „Kritisch“, gruppiert nach Warnungsname.  Klicken Sie auf einen Warnungsnamen, um eine Protokollsuche durchzuführen, mit der alle Datensätze für die Warnung zurückgegeben werden. |
| Warnungen |Alle Warnungen mit dem Schweregrad „Warnung“, gruppiert nach Warnungsname.  Klicken Sie auf einen Warnungsnamen, um eine Protokollsuche durchzuführen, mit der alle Datensätze für die Warnung zurückgegeben werden. |
| Aktive SCOM-Warnungen |Alle von SCOM gesammelten Warnungen mit einem anderen Zustand als *Geschlossen*, gruppiert nach der Quelle, die die Warnung generiert hat. |
| Alle aktiven Warnungen |Alle Warnungen mit einem beliebigen Schweregrad, gruppiert nach dem Namen der Warnung. Umfasst nur SCOM-Warnungen mit einem anderen Zustand als *Geschlossen*. |

Wenn Sie einen Bildlauf nach rechts durchführen, werden im Dashboard mehrere allgemeine Abfragen aufgeführt, auf die Sie klicken können, um eine [Protokollsuche](log-analytics-log-searches.md) nach Warnungsdaten durchzuführen.

![Alert Management-Dashboard](media/log-analytics-solution-alert-management/dashboard.png)


## <a name="log-analytics-records"></a>Log Analytics-Datensätze
Mit der Alert Management-Lösung werden alle Datensätze vom Typ **Warnung**analysiert.  Von Log Analytics erstellte oder von Nagios bzw. Zabbix gesammelte Warnungen werden nicht direkt von der Lösung gesammelt.

Die Lösung importiert Warnungen aus SCOM, und es werden jeweils ein entsprechender Datensatz vom Typ **Warnung** und der SourceSystem-Eintrag **OpsManager** erstellt.  Die Eigenschaften der Datensätze sind in der folgenden Tabelle aufgeführt.  

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Typ |*Warnung* |
| SourceSystem |*OpsManager* |
| AlertContext |Details des Datenelements, durch das die Warnung verursacht wurde, im XML-Format. |
| AlertDescription |Detaillierte Beschreibung der Warnung. |
| AlertId |GUID der Warnung. |
| AlertName |Name der Warnung. |
| AlertPriority |Prioritätsstufe der Warnung. |
| AlertSeverity |Schweregrad der Warnung. |
| AlertState |Letzter Lösungszustand der Warnung. |
| LastModifiedBy |Name des Benutzers, der die Warnung zuletzt geändert hat. |
| ManagementGroupName |Name der Verwaltungsgruppe, in der die Warnung generiert wurde. |
| RepeatCount |Angabe, wie oft die gleiche Warnung für dasselbe überwachte Objekt seit der Lösung generiert wurde. |
| ResolvedBy |Name des Benutzers, der die Lösung für die Warnung durchgeführt hat. Ist leer, wenn die Warnung noch nicht gelöst wurde. |
| SourceDisplayName |Anzeigename des Überwachungsobjekts, von dem die Warnung generiert wurde. |
| SourceFullName |Vollständiger Name des Überwachungsobjekts, von dem die Warnung generiert wurde. |
| TicketId |Ticket-ID für die Warnung, wenn die SCOM-Umgebung in einen Prozess zum Zuweisen von Tickets für Warnungen integriert ist.  Ist leer, wenn keine Ticket-ID zugewiesen wurde. |
| TimeGenerated |Datum und Uhrzeit der Warnungserstellung. |
| TimeLastModified |Datum und Uhrzeit der letzten Änderung der Warnung. |
| TimeRaised |Datum und Uhrzeit der Warnungsgenerierung. |
| TimeResolved |Datum und Uhrzeit der Warnungslösung. Ist leer, wenn die Warnung noch nicht gelöst wurde. |

## <a name="sample-log-searches"></a>Beispiele für Protokollsuchen
Die folgende Tabelle enthält Beispiele für Protokollsuchen für Warnungsdatensätze, die mit dieser Lösung erfasst wurden.  

| Abfrage | Beschreibung |
|:--- |:--- |
| Type=Alert SourceSystem=OpsManager AlertSeverity=error TimeRaised>NOW-24HOUR |Kritische Warnungen, die innerhalb der letzten 24 Stunden ausgelöst wurden |
| Type=Alert AlertSeverity=warning TimeRaised>NOW-24HOUR |Warnungen vom Typ „Warnung“, die innerhalb der letzten 24 Stunden ausgelöst wurden |
| Type=Alert SourceSystem=OpsManager AlertState!=Closed TimeRaised>NOW-24HOUR &#124; measure count() as Count by SourceDisplayName |Quellen mit aktiven Warnungen, die während der letzten 24 Stunden ausgelöst wurden |
| Type=Alert SourceSystem=OpsManager AlertSeverity=error TimeRaised>NOW-24HOUR AlertState!=Closed |Kritische Warnungen, die während der letzten 24 Stunden ausgelöst wurden und noch aktiv sind |
| Type=Alert SourceSystem=OpsManager TimeRaised>NOW-24HOUR AlertState=Closed |Warnungen, die während der letzten 24 Stunden ausgelöst wurden und jetzt geschlossen wurden |
| Type=Alert SourceSystem=OpsManager TimeRaised>NOW-1DAY &#124; measure count() as Count by AlertSeverity |Warnungen, die während des letzten Tages ausgelöst wurden, gruppiert nach Schweregrad |
| Type=Alert SourceSystem=OpsManager TimeRaised>NOW-1DAY &#124; sort RepeatCount desc |Warnungen, die während des letzten Tages ausgelöst wurden, sortiert nach dem Wert der Wiederholungsanzahl |

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie sich die Details zum Generieren von Warnungen aus Log Analytics unter [Warnungen in Log Analytics](log-analytics-alerts.md) durch.


