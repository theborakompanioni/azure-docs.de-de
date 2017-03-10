---
title: "Übersicht über Warnungen in OMS Log Analytics | Microsoft-Dokumentation"
description: "Mit Warnungen in Log Analytics werden wichtige Informationen in Ihrem OMS-Repository identifiziert, und Sie können proaktiv über Probleme informiert werden oder Aktionen aufrufen, um zu versuchen, die Probleme zu beheben.  In diesem Artikel wird beschrieben, wie Sie eine Warnungsregel erstellen, und es werden die verschiedenen Aktionen vorgestellt, die Sie durchführen können."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: ec7167fb36e54219957629699f33dfce950f86d5
ms.openlocfilehash: 48d921650dbbf3f9cc2a8a0e265e0c7deaebdafd
ms.lasthandoff: 03/01/2017


---
# <a name="respond-to-issues-in-log-analytics-using-alerts"></a>Reagieren auf Probleme in Log Analytics mithilfe von Warnungen

Mit Warnungen werden in Log Analytics wichtige Informationen in Ihrem Log Analytics-Repository identifiziert.  Sie können dies nutzen, um sich proaktiv über ein Problem informieren zu lassen oder als Reaktion auf das Problem automatisierte Aktionen durchzuführen.  Dieser Artikel enthält eine Übersicht darüber, wie Warnungen erstellt und verwendet werden.  


>[!NOTE]
> Informationen zu Warnungsregeln für Metrikmessungen, die derzeit in der öffentlichen Vorschau verfügbar sind, finden Sie unter [New metric measurement alert rule type in Public Preview!](https://blogs.technet.microsoft.com/msoms/2016/11/22/new-metric-measurement-alert-rule-type-in-public-preview/) (Neuer Warnungsregeltyp für Metrikmessungen in der öffentlichen Vorschau!).

## <a name="alert-rules"></a>Warnungsregeln

Warnungen werden mithilfe von Warnungsregeln erstellt, für die in regelmäßigen Abständen automatisch Protokollsuchen durchgeführt werden.  Wenn die Ergebnisse der Protokollsuche bestimmte Kriterien erfüllen, wird ein Warnungsdatensatz erstellt.  Die Regel kann auch automatisch eine oder mehrere Aktionen durchführen, um Sie proaktiv über die Warnung zu informieren oder einen anderen Prozess aufzurufen.  

![Log Analytics-Warnungen](media/log-analytics-alerts/overview.png)


Warnungsregeln werden anhand der folgenden Details definiert:

- **Protokollsuche**:  Dies ist die Abfrage, die bei jeder Auslösung der Warnungsregel ausgeführt wird.  Mit den von dieser Abfrage zurückgegebenen Datensätzen wird ermittelt, ob eine Warnung erstellt werden muss.
- **Zeitfenster**:  Gibt den Zeitraum für die Abfrage an.  Die Abfrage gibt nur Datensätze zurück, die innerhalb dieses aktuellen Zeitbereichs erstellt wurden.  Dies kann ein beliebiger Wert zwischen 5 Minuten und 24 Stunden sein. Wenn das Zeitfenster beispielsweise auf 60 Minuten festgelegt ist und die Abfrage um 13:15 Uhr ausgeführt wird, werden nur Datensätze zurückgegeben, die zwischen 12:15 und 13:15 Uhr erstellt wurden.
- **Häufigkeit**:  Gibt an, wie oft die Abfrage ausgeführt werden soll. Dies kann ein beliebiger Wert zwischen 5 Minuten und 24 Stunden sein. Er sollte kleiner als oder gleich dem Zeitfensterwert sein.  Wenn der Wert größer als das Zeitfenster ist, besteht das Risiko, dass Datensätze ausgelassen werden.<br>Angenommen, Sie verwenden ein Zeitfenster von 30 Minuten und eine Häufigkeit von 60 Minuten.  Wenn die Abfrage um 13:00 Uhr ausgeführt wird, gibt sie die Datensätze für den Zeitraum zwischen 12:30 und 13:00 Uhr zurück.  Wenn die Abfrage dann das nächste Mal um 14 Uhr ausgeführt wird, gibt sie die Datensätze für den Zeitraum zwischen 13:30 und 14:00 Uhr zurück.  Alle Datensätze, die zwischen 13:00 und 13:30 erstellt werden, werden also nicht ausgewertet.
- **Schwellenwert**:  Wenn die Anzahl von Datensätzen, die für die Protokollsuche zurückgegeben werden, den Schwellenwert überschreitet, wird eine Warnung erstellt.

## <a name="creating-alert-rules"></a>Erstellen von Warnungsregeln
Es gibt mehrere Möglichkeiten, wie Sie Warnungsregeln erstellen und ändern können.  Die folgenden Artikel enthalten hierzu ausführliche Anleitungen.  

- Erstellen von Warnungsregeln mithilfe des [OMS-Portals](log-analytics-alerts-creating.md)
- Erstellen von Warnungsregeln mithilfe einer [Resource Manager-Vorlage](log-analytics-template-workspace-configuration.md)
- Erstellen von Warnungsregeln mithilfe der [REST-API](log-analytics-api-alerts.md)

## <a name="alert-actions"></a>Warnungsaktionen

Zusätzlich zur Erstellung eines Warnungsdatensatzes können mit einer Warnungsregel beim Erstellen einer Warnung eine oder mehrere Aktionen durchgeführt werden.  Sie können Aktionen verwenden, um als Reaktion auf die Warnung eine E-Mail zu senden oder einen Prozess zu starten, mit dem versucht wird, Korrekturmaßnahmen einzuleiten.  

Außerdem können Sie Aktionen nutzen, um die Log Analytics-Funktionalität um andere Dienste zu erweitern.  Beispielsweise sind derzeit keine Funktionen für die Benachrichtigung per SMS oder Telefon vorhanden.  Sie können aber eine Webhookaktion in einer Warnungsregel verwenden, um einen Dienst wie [PagerDuty](https://www.pagerduty.com/) aufzurufen, mit dem diese Funktionen bereitgestellt werden.  Unter [Webhooks in Log Analytics-Warnungen](log-analytics-alerts-webhooks.md) können Sie die Schritte eines Beispiels für die Erstellung eines Webhooks zum Senden einer Nachricht per [Slack](https://slack.com/) durchgehen.

In der folgenden Tabelle sind die Aktionen aufgeführt, die Sie durchführen können.  Weitere Informationen hierzu finden Sie unter [Add actions to alert rules in Log Analytics](log-analytics-alerts-actions.md) (Hinzufügen von Warnungsregeln in Log Analytics). 

| Aktion | Beschreibung |
|:--|:--|
| E-Mail  |     Senden einer E-Mail mit den Details der Warnung an einen oder mehrere Empfänger |
| Webhook | Aufrufen eines externen Prozesses mit einer einzelnen HTTP POST-Anforderung |
| Runbook | Starten eines Runbooks in Azure Automation |


## <a name="alerting-scenarios"></a>Warnungsszenarien

### <a name="events"></a>Ereignisse
Erstellen Sie zum Auslösen einer Warnung aufgrund eines einzelnen Ereignisses eine Warnungsregel mit einer Anzahl von Ergebnissen, die **Größer als 0** ist, und legen Sie sowohl die Häufigkeit als auch das Zeitfenster auf **5 Minuten** fest.  Die Abfrage wird dann alle 5 Minuten ausgeführt. Außerdem wird eine Prüfung auf das Vorhandensein eines einzelnen Ereignisses durchgeführt, das seit der letzten Ausführung der Abfrage erstellt wurde.  Bei einem längeren Intervall verlängert sich ggf. der Zeitraum zwischen der Ereigniserfassung und der Warnungserstellung.  Sie verwenden in diesem Fall eine Abfrage, die dem folgenden Beispiel ähnelt, um das entsprechende Ereignis anzugeben.

    Type=Event Source=MyApplication EventID=7019 

Für einige Anwendungen wird unter Umständen gelegentlich ein Fehler protokolliert, für den nicht unbedingt eine Warnung ausgelöst werden muss.  Es kann beispielsweise sein, dass die Anwendung versucht, den Vorgang mit dem Fehler erneut durchzuführen, und dass der Vorgang dann erfolgreich ist.  In diesem Fall sollten Sie die Warnung ggf. nur erstellen, wenn mehrere Ereignisse innerhalb eines bestimmten Zeitfensters erstellt werden.  Hierzu verwenden Sie die gleiche Abfrage und legen den Schwellenwert auf einen höheren Wert fest.  Beispiel: Für eine Warnung bei fünf Ereignissen in 30 Minuten legen Sie die Häufigkeit auf **5 Minuten**, das Zeitfenster auf **30 Minuten** und die Anzahl von Ergebnissen auf **Größer als 4** fest.    

Es kann auch vorkommen, dass Sie eine Warnung erstellen möchten, ohne dass ein entsprechendes Ereignis vorliegt.  Für einen Prozess können beispielsweise regelmäßig Ereignisse protokolliert werden, um anzugeben, dass er richtig funktioniert.  Wenn innerhalb eines bestimmten Zeitfensters nicht eines dieser Ereignisse protokolliert wird, sollte eine Warnung erstellt werden.  In diesem Fall legen Sie die Anzahl von Ergebnissen auf **Kleiner als 1** fest.

### <a name="performance-alerts"></a>Leistungswarnungen
[Leistungsdaten](log-analytics-data-sources-performance-counters.md) werden ähnlich wie Ereignisse als Datensätze im Log Analytics-Repository gespeichert.  Falls gewarnt werden soll, wenn ein Leistungsindikator einen bestimmten Schwellenwert überschreitet, sollte dieser Schwellenwert in die Abfrage einbezogen werden.

Falls beispielsweise eine Warnung erfolgen soll, wenn der Prozessor zu mehr als 90 Prozent ausgelastet ist, können Sie eine Abfrage wie die folgende verwenden und dabei die Anzahl von Ergebnissen für die Warnungsregel auf **Größer als 0** festlegen:

    Type=Perf ObjectName=Processor CounterName="% Processor Time" CounterValue>90

Falls eine Warnung erfolgen soll, wenn der Prozessor innerhalb eines bestimmten Zeitfensters im Schnitt zu mehr als 90 Prozent ausgelastet ist, können Sie wie im folgenden Beispiel eine Abfrage mit dem [measure-Befehl](log-analytics-search-reference.md#commands) verwenden und dabei den Schwellenwert für die Warnungsregel auf **Größer als 0** festlegen: 

    Type=Perf ObjectName=Processor CounterName="% Processor Time" | measure avg(CounterValue) by Computer | where AggregatedValue>90


## <a name="alert-records"></a>Warnungsdatensätze
Für Warnungsdatensätze, die in Log Analytics mit Warnungsregeln erstellt wurden, ist **Warnung** als **Typ** und **OMS** als **SourceSystem** festgelegt.  Sie verfügen über die Eigenschaften, die in der folgenden Tabelle aufgeführt sind.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Typ |*Warnung* |
| SourceSystem |*OMS* |
| AlertName |Name der Warnung. |
| AlertSeverity |Schweregrad der Warnung. |
| LinkToSearchResults |Link zur Log Analytics-Protokollsuche, mit der die Datensätze aus der Abfrage zurückgegeben werden, mit der die Warnung erstellt wurde |
| Abfrage |Der Text der Abfrage, die ausgeführt wurde. |
| QueryExecutionEndTime |Gibt das Ende des Zeitraums für die Abfrage an. |
| QueryExecutionStartTime |Gibt den Beginn des Zeitraums für die Abfrage an. |
| ThresholdOperator | Operator, der von der Warnungsregel verwendet wird. |
| ThresholdValue | Wert, der von der Warnungsregel verwendet wird. |
| TimeGenerated |Datum und Uhrzeit der Warnungserstellung. |

Es gibt noch andere Arten von Warnungsdatensätzen, die von der [Alert Management-Lösung](log-analytics-solution-alert-management.md) und von [Power BI-Exporten](log-analytics-powerbi.md) erstellt werden.  Für diese ist **Warnung** als **Typ** festgelegt, aber sie unterscheiden sich durch ihr **SourceSystem**.


## <a name="next-steps"></a>Nächste Schritte
* Erstellen Sie eine Warnungsregel über das [OMS-Portal](log-analytics-alerts-creating.md).
* Installieren Sie die [Alert Management-Lösung](log-analytics-solution-alert-management.md) , um die in Log Analytics erstellten Warnungen und die über System Center Operations Manager (SCOM) gesammelten Warnungen zu analysieren.
* Informieren Sie sich weiter über [Protokollsuchen](log-analytics-log-searches.md) , bei denen Warnungen generiert werden können.
* Arbeiten Sie eine exemplarische Vorgehensweise für das [Konfigurieren eines Webooks](log-analytics-alerts-webhooks.md) mit einer Warnungsregel durch.  
* Informieren Sie sich darüber, wie Sie [Runbooks in Azure Automation](https://azure.microsoft.com/documentation/services/automation) schreiben, um von Warnungen identifizierte Probleme zu beheben.


