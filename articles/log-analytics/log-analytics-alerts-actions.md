---
title: Antworten auf Warnungen in OMS Log Analytics | Microsoft-Dokumentation
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
ms.sourcegitcommit: fdf22ff85a3a76be5de50632c4948df44c2312df
ms.openlocfilehash: b8731e1fe48b7d809b113eb5273e3962542b8f34
ms.lasthandoff: 03/01/2017


---

# <a name="add-actions-to-alert-rules-in-log-analytics"></a>Hinzufügen von Aktionen zu Warnungsregeln in Log Analytics
Wenn eine [Warnung in Log Analytics erstellt wird](log-analytics-alerts.md), können Sie optional [eine Warnungsregel konfigurieren](log-analytics-alerts.md), um eine oder mehrere Aktionen auszuführen.  Dieser Artikel beschreibt die verschiedenen verfügbaren Aktionen und Details zu ihrer jeweiligen Konfiguration.

| Aktion | Beschreibung |
|:--|:--|
| [E-Mail](#email-actions) |    Senden einer E-Mail mit den Details der Warnung an einen oder mehrere Empfänger |
| [Webhook](#webhook-actions) | Aufrufen eines externen Prozesses mit einer einzelnen HTTP POST-Anforderung |
| [Runbook](#runbook-actions) | Starten eines Runbooks in Azure Automation |


## <a name="email-actions"></a>E-Mail-Aktionen
Bei E-Mail-Aktionen wird eine E-Mail mit den Details der Warnung an einen oder mehrere Empfänger gesendet.  Sie können den Betreff der E-Mail angeben, aber der Inhalt ist ein von Log Analytics erstelltes Standardformat.  Darin sind zusammenfassende Informationen enthalten, z.B. der Name der Warnung sowie die Details von bis zu zehn Datensätzen, die von der Protokollsuche zurückgegeben werden.  Außerdem ist ein Link zu einer Protokollsuche in Log Analytics enthalten, mit der alle Datensätze dieser Abfrage zurückgegeben werden.   Der Absender der E-Mail lautet *Microsoft Operations Management Suite Team &lt;noreply@oms.microsoft.com&gt;*. 

E-Mail-Aktionen erfordern die in der folgenden Tabelle aufgeführten Eigenschaften.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Betreff |Der Betreff der E-Mail.  Sie können den Haupttext der E-Mail nicht ändern. |
| Empfänger |Adressen aller E-Mail-Empfänger.  Verwenden Sie als Trennzeichen ein Semikolon (;), wenn Sie mehrere Adressen angeben. |


## <a name="webhook-actions"></a>Webhookaktionen

Mit Webhookaktionen können Sie einen externen Prozess über eine HTTP POST-Anforderung aufrufen.  Der aufgerufene Dienst sollte Webhooks unterstützen, und es sollte festgelegt werden, wie empfangene Nutzlasten verwendet werden.  Sie können auch eine REST-API aufrufen, die keine spezielle Unterstützung für Webhooks bietet, solange die Anforderung ein für die API verständliches Format hat.  Beispiele für die Verwendung eines Webhooks als Antwort auf eine Warnung sind das Senden einer Nachricht in [Slack](http://slack.com) oder Erstellen eines Vorfalls in [PagerDuty](http://pagerduty.com/).  Eine vollständige exemplarische Vorgehensweise für die Erstellung einer Warnungsregel mit einem Webhook zum Aufrufen von Slack finden Sie unter [Webhooks in Log Analytics-Warnungen](log-analytics-alerts-webhooks.md).

Webhook-Aktionen erfordern die in der folgenden Tabelle aufgeführten Eigenschaften.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Webhook-URL |Die URL des Webhooks. |
| Benutzerdefinierte JSON-Nutzlast |Benutzerdefinierte Nutzlast, die mit dem Webhook gesendet wird.  Weitere Informationen siehe unten. |


Webhooks enthalten eine URL und eine Nutzlast im JSON-Format, wobei es sich um die an den externen Dienst gesendeten Daten handelt.  Die Nutzlast enthält standardmäßig die Werte in der folgenden Tabelle.  Sie können diese Nutzlast auch durch eine benutzerdefinierte eigene Nutzlast ersetzen.  In diesem Fall können Sie die Variablen in der Tabelle für die einzelnen Parameter verwenden, um deren Wert in die benutzerdefinierte Nutzlast einzubinden.

| Parameter | Variable | Beschreibung |
|:--- |:--- |:--- |
| AlertRuleName |#AlertRuleName |Name der Warnungsregel |
| AlertThresholdOperator |#thresholdoperator |Schwellenwertoperator für die Warnungsregel  *Größer als* oder *Kleiner als* |
| AlertThresholdValue |#thresholdvalue |Wert des Schwellenwerts für die Warnungsregel |
| LinkToSearchResults |#LinkToSearchResults |Link zur Log Analytics-Protokollsuche, mit der die Datensätze aus der Abfrage zurückgegeben werden, mit der die Warnung erstellt wurde |
| ResultCount |#searchresultcount |Anzahl von Datensätzen in den Suchergebnissen |
| SearchIntervalEndtimeUtc |#SearchIntervalEndtimeUtc |Endzeit für die Abfrage im UTC-Format |
| SearchIntervalInSeconds |#searchinterval |Zeitfenster für die Warnungsregel |
| SearchIntervalStartTimeUtc |#SearchIntervalStartTimeUtc |Startzeit für die Abfrage im UTC-Format |
| SearchQuery |#SearchQuery |Von der Warnungsregel verwendete Protokollsuchabfrage |
| SearchResults |Siehe unten |Von der Abfrage im JSON-Format zurückgegebene Datensätze.  Auf die ersten 5.000 Datensätze beschränkt. |
| WorkspaceID |#WorkspaceID |ID Ihres OMS-Arbeitsbereichs |

Sie können beispielsweise die folgende benutzerdefinierte Nutzlast angeben, die einen einzelnen Parameter wie *text*enthält.  Der Dienst, der von diesem Webhook aufgerufen wird, erwartet diesen Parameter.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

Diese Beispielnutzlast wird ähnlich wie hier dargestellt aufgelöst, wenn sie an den Webhook gesendet wird.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

Um die Suchergebnisse in eine benutzerdefinierte Nutzlast einzubinden, fügen Sie der JSON-Nutzlast die folgende Zeile als Eigenschaft der obersten Ebene hinzu.  

    "IncludeSearchResults":true

Sie können beispielsweise Folgendes verwenden, um eine benutzerdefinierte Nutzlast zu erstellen, die nur den Warnungsnamen und die Suchergebnisse enthält. 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


Ein vollständiges Beispiel für die Erstellung einer Warnungsregel mit einem Webhook, um einen externen Dienst zu starten, können Sie unter [Erstellen einer Warnungs-Webhook-Aktion in OMS Log Analytics zum Senden einer Nachricht an Slack](log-analytics-alerts-webhooks.md) durchgehen.

## <a name="runbook-actions"></a>Runbookaktionen
Bei Runbookaktionen wird ein Runbook in Azure Automation gestartet.  Zum Verwenden dieser Art von Aktion muss die [Automation-Lösung](log-analytics-add-solutions.md) im OMS-Arbeitsbereich installiert und konfiguriert sein.  Sie können eines der Runbooks im Automation-Konto auswählen, die Sie in der Automation-Lösung konfiguriert haben.

Runbook-Aktionen erfordern die in der folgenden Tabelle aufgeführten Eigenschaften.

| Eigenschaft | Beschreibung |
|:--- |:---|
| Runbook | Runbook, das Sie starten möchten, wenn eine Warnung erstellt wird. |
| Run on (Ausführen auf) | Geben Sie **Azure** an, um das Runbook in der Cloud auszuführen.  Geben Sie **Hybrid Worker** an, um das Runbook auf einem Agent auszuführen, auf dem [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md ) installiert ist.  |

Bei Runbookaktionen wird das Runbook mit einem [Webhook](../automation/automation-webhooks.md) gestartet.  Wenn Sie die Warnungsregel erstellen, wird für das Runbook automatisch ein neuer Webhook mit dem Namen **OMS Alert Remediation** gefolgt von einer GUID erstellt.  

Sie können die Parameter des Runbooks nicht direkt ausfüllen, aber der [$WebhookData-Parameter](../automation/automation-webhooks.md) enthält die Details zur Warnung, einschließlich der Ergebnisse der Protokollsuche, die sie ausgelöst hat.  Das Runbook muss **$WebhookData** als Parameter definieren, damit ein Zugriff auf die Eigenschaften der Warnung möglich ist.  Die Warnungsdaten sind im JSON-Format in der Eigenschaft **SearchResults** der **RequestBody**-Eigenschaft des **$WebhookData**-Parameters verfügbar.  Darin enthalten sind die Eigenschaften, die in der folgenden Tabelle aufgeführt sind.

| Knoten | Beschreibung |
|:--- |:--- |
| id |Pfad und GUID der Suche |
| __metadata |Informationen zur Warnung, einschließlich der Anzahl der Datensätze und des Status der Suchergebnisse |
| value |Separater Eintrag für jeden Datensatz in den Suchergebnissen.  Die Details des Eintrags stimmen mit den Eigenschaften und Werten des Datensatzes überein. |

Das folgende Runbook würde beispielsweise die von der Protokollsuche zurückgegebenen Datensätze extrahieren und basierend auf dem Typ der einzelnen Datensätze verschiedene Eigenschaften zuweisen.  Beachten Sie, dass das Runbook mit der Konvertierung von **RequestBody** aus dem JSON-Format beginnt, damit es in PowerShell als Objekt verarbeitet werden kann.

    param ( 
        [object]$WebhookData
    )

    $RequestBody = ConvertFrom-JSON -InputObject $WebhookData.RequestBody
    $Records     = $RequestBody.SearchResults.value

    foreach ($Record in $Records)
    {
        $Computer = $Record.Computer

        if ($Record.Type -eq 'Event')
        {
            $EventNo    = $Record.EventID
            $EventLevel = $Record.EventLevelName
            $EventData  = $Record.EventData
        }

        if ($Record.Type -eq 'Perf')
        {
            $Object    = $Record.ObjectName
            $Counter   = $Record.CounterName
            $Instance  = $Record.InstanceName
            $Value     = $Record.CounterValue
        }
    }


## <a name="next-steps"></a>Nächste Schritte
- Arbeiten Sie eine exemplarische Vorgehensweise für das [Konfigurieren eines Webooks](log-analytics-alerts-webhooks.md) mit einer Warnungsregel durch.  
- Informieren Sie sich darüber, wie Sie [Runbooks in Azure Automation](https://azure.microsoft.com/documentation/services/automation) schreiben, um von Warnungen identifizierte Probleme zu beheben.
