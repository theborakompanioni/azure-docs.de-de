---
title: "Häufig gestellte Fragen zur neuen Protokollsuche von Log Analytics | Microsoft-Dokumentation"
description: "Dieser Artikel behandelt häufig gestellte Fragen im Hinblick auf das Upgrade von Log Analytics auf die neue Abfragesprache."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: eeed445631885093a8e1799a8a5e1bcc69214fe6
ms.openlocfilehash: 507136beef9718dc6a7f42a4b84f8030d4a60563
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---

# <a name="log-analytics-new-log-search-faq-and-known-issues"></a>Neue Protokollsuche in Log Analytics – häufig gestellte Fragen und bekannte Probleme

Dieser Artikel enthält häufig gestellte Fragen und bekannte Probleme im Hinblick auf das Upgrade von [Log Analytics auf die neue Abfragesprache](log-analytics-log-search-upgrade.md).  Sie sollten diesen Artikel lesen, bevor Sie sich entscheiden, Ihren Arbeitsbereich zu aktualisieren.


## <a name="alerts"></a>Warnungen

### <a name="question-i-have-a-lot-of-alert-rules-do-i-need-to-create-them-again-in-the-new-language-after-i-upgrade"></a>Frage: Ich habe viele Warnungsregeln. Muss ich diese erneut in der neuen Sprache erstellen, nachdem ich das Upgrade durchgeführt habe?  
Nein, Ihre Warnungsregeln werden während des Upgrades automatisch in die neue Suchsprache konvertiert.  


## <a name="computer-groups"></a>Computergruppen

### <a name="question-im-getting-errors-when-trying-to-use-computer-groups--has-their-syntax-changed"></a>Frage: Ich erhalte Fehler, wenn ich versuche, Computergruppen zu verwenden.  Hat sich deren Syntax geändert?
Ja, die Syntax für die Verwendung von Computergruppen hat sich geändert, wenn Sie ein Upgrade Ihres Arbeitsbereichs durchgeführt haben.  Weitere Informationen finden Sie unter [Computergruppen in Log Analytics-Protokollsuchen](log-analytics-computer-groups.md).

### <a name="known-issue-groups-imported-from-active-directory"></a>Bekanntes Problem: Aus Active Directory importierte Gruppen
Sie können derzeit keine Abfrage erstellen, die eine aus Active Directory importierte Computergruppe verwendet.  Bis dieses Problem behoben wurde, erstellen Sie als Umgehung eine neue Computergruppe anhand der aus Active Directory importierten Gruppe und verwenden diese neue Gruppe in der Abfrage.

Im Folgenden sehen Sie eine Beispielabfrage zum Erstellen einer neuen Computergruppe, die eine aus Active Directory importierte Gruppe enthält:

    ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "AD Group Name" and TimeGenerated >= ago(24h) | distinct Computer


## <a name="dashboards"></a>Dashboards

### <a name="question-can-i-still-use-dashboards-in-an-upgraded-workspace"></a>Frage: Kann ich in einem aktualisierten Arbeitsbereich weiterhin Dashboards verwenden?
Sie können weiterhin alle Kacheln verwenden, die Sie unter **Mein Dashboard** hinzugefügt haben, bevor Ihr Arbeitsbereich aktualisiert wurde, aber Sie können diese Kacheln nicht bearbeiten und auch keine neuen hinzufügen.  Sie können weiterhin mit dem [Ansicht-Designer](log-analytics-view-designer.md) Ansichten erstellen und bearbeiten und auch Dashboards im Azure-Portal erstellen.


## <a name="log-searches"></a>Protokollsuchvorgänge

### <a name="question-i-have-saved-searches-outside-of-my-upgraded-workspace-can-i-convert-them-to-the-new-search-language-automatically"></a>Frage: Ich habe Suchen außerhalb meines aktualisierten Arbeitsbereichs gespeichert. Kann ich diese automatisch in die neue Suchsprache konvertieren?
Sie können den das Tool zum Konvertieren der Sprache auf der Seite der Protokollsuche verwenden, um sie zu konvertieren.  Es gibt keine Möglichkeit, mehrere Suchen automatisch zu konvertieren, ohne ein Upgrade auf den Arbeitsbereich durchzuführen.

### <a name="question-why-are-my-query-results-not-sorted"></a>Frage: Warum sind meine Abfrageergebnisse nicht sortiert?
Ergebnisse werden in der neuen Abfragesprache standardmäßig nicht sortiert.  Verwenden Sie den [sort-Operator](https://go.microsoft.com/fwlink/?linkid=856079), um die Ergebnisse nach einer oder mehreren Eigenschaften zu sortieren.

### <a name="known-issue-search-results-in-a-list-may-include-properties-with-no-data"></a>Bekanntes Problem: Suchergebnisse in einer Liste können Eigenschaften ohne Daten enthalten.
In den Suchergebnissen in einer Liste können Eigenschaften ohne Daten angezeigt werden.  Vor dem Upgrade wären solche Eigenschaften nicht enthalten gewesen.  Dieses Problem wird behoben, damit leere Eigenschaften nicht angezeigt werden.

### <a name="known-issue-selecting-a-value-in-a-chart-doesnt-display-detailed-results"></a>Bekanntes Problem: Durch Auswählen eines Werts in einem Diagramm werden keine ausführlichen Ergebnisse angezeigt.
Wenn Sie vor dem Upgrade einen Wert in einem Diagramm ausgewählt hatten, wurde eine detaillierte Liste der Datensätze zurückgeben, die dem ausgewählten Wert entsprachen.  Nach dem Upgrade wird nur eine zusammengefasste Zeile zurückgegeben.  Dieses Problem wird zurzeit untersucht.

## <a name="log-search-api"></a>Protokollsuch-API

### <a name="question-does-the-log-search-api-get-updated-after-i-upgrade"></a>Frage: Wird die Protokollsuch-API nach dem Upgrade aktualisiert?
Die Legacy-[Protokollsuch-API](log-analytics-log-search-api.md) funktioniert nicht mehr, wenn Sie den Arbeitsbereich aktualisiert haben.  Nähere Informationen zur neuen API finden Sie unter [Azure Log Analytics REST API](https://dev.loganalytics.io/) (Azure Log Analytics-REST-API).


## <a name="portals"></a>Portale

### <a name="question-should-i-use-the-new-advanced-analytics-portal-or-keep-using-the-log-search-portal"></a>Frage: Sollte ich das neue Advanced Analytics-Portal oder weiterhin das Portal für die Protokollsuche verwenden?
Einen Vergleich der beiden Portale finden Sie unter [Portale zum Erstellen und Bearbeiten von Protokollabfragen in Azure Log Analytics](log-analytics-log-search-portals.md).  Jedes Portal verfügt über spezielle Vorteile, daher können Sie das am besten für Ihre Anforderungen geeignete verwenden.  Häufig werden Abfragen im Advanced Analytics-Portal geschrieben und an anderen Stellen eingefügt, z.B. im Ansicht-Designer.  Lesen Sie in diesem Fall die [zu berücksichtigenden Aspekte](log-analytics-log-search-portals.md#advanced-analytics-portal).


## <a name="power-bi"></a>Power BI

### <a name="question-does-anything-change-with-powerbi-integration"></a>Frage: Ändert sich etwas bei der Power BI-Integration?
Ja.  Sobald für Ihren Arbeitsbereich ein Upgrade durchgeführt wurde, funktioniert dieses Verfahren zum Exportieren von Log Analytics-Daten in Power BI nicht mehr.  Alle vorhandenen Zeitpläne, die Sie vor dem Upgrade erstellt haben, werden deaktiviert.  Nach dem Upgrade verwendet Azure Log Analytics dieselbe Plattform wie Application Insights, und Sie verwenden zum Exportieren von Log Analytics-Abfragen nach Power BI dasselbe Verfahren wie zum [Exportieren von Application Insights-Abfragen nach Power BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).

### <a name="known-issue-power-bi-request-size-limit"></a>Bekanntes Problem: Größenbeschränkung für Power BI-Anforderungen
Derzeit gilt eine Größenbeschränkung von 8 MB für Log Analytics-Abfragen, die nach Power BI exportiert werden können.  Dieses Limit wird demnächst erhöht.


##<a name="powershell-cmdlets"></a>PowerShell-Cmdlets

### <a name="question-does-the-log-search-powershell-cmdlet-get-updated-after-i-upgrade"></a>Frage: Wird das PowerShell-Cmdlet für die Protokollsuche nach dem Upgrade aktualisiert?
Das Cmdlet [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/Get-AzureRmOperationalInsightsSearchResults) wurde noch nicht auf die neue Suchsprache aktualisiert.  Verwenden Sie deshalb auch nach einem Upgrade Ihres Arbeitsbereichs weiterhin die alte Abfragesprache mit diesem Cmdlet.  Eine aktualisierte Dokumentation wird nach dem Update des Cmdlets verfügbar gemacht.


## <a name="resource-manager-templates"></a>Resource Manager-Vorlagen

### <a name="question-can-i-create-an-upgraded-workspace-with-a-resource-manager-template"></a>Frage: Kann ich einen aktualisierten Arbeitsbereich mithilfe einer Resource Manager-Vorlage erstellen?
Ja.  Sie müssen die API-Version 2017-03-15-preview verwenden und wie im folgenden Beispiel gezeigt einen Abschnitt **features** in die Vorlage einfügen.

    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "apiVersion": "2017-03-15-preview",
            "name": "[parameters('workspaceName')]",
            "location": "[parameters('workspaceRegion')]",
            "properties": {
                "sku": {
                    "name": "Free"
                },
                "features": {
                    "legacy": 0,
                    "searchVersion": 1
                }
            }
        }
    ],



## <a name="solutions"></a>Lösungen

### <a name="question-will-my-solutions-continue-to-work"></a>Frage: Bleiben meine Lösungen weiterhin funktionsfähig?
Alle Lösungen bleiben in einem aktualisierten Arbeitsbereich weiterhin funktionsfähig. Ihre Leistung verbessert sich jedoch, wenn sie in die neue Abfragesprache konvertiert werden.  Es gibt bekannte Probleme mit einigen vorhandenen Lösungen, die in diesem Abschnitt beschrieben werden.

### <a name="known-issue-capacity-and-performance-solution"></a>Bekanntes Problem: Lösung „Kapazität und Leistung“
Einige der Teile in der Ansicht [Kapazität und Leistung](log-analytics-capacity.md) sind eventuell leer.  Eine Korrektur für dieses Problem wird in Kürze verfügbar sein.

### <a name="known-issue-device-health-solution"></a>Bekanntes Problem: Lösung „Integrität für Geräte“
Die Lösung [Integrität für Geräte](https://docs.microsoft.com/windows/deployment/update/device-health-monitor) sammelt in einem aktualisierten Arbeitsbereich keine Daten.  Eine Korrektur für dieses Problem wird in Kürze verfügbar sein.

### <a name="known-issue-application-insights-connector"></a>Bekanntes Problem: Application Insights-Connector
In einem aktualisierten Arbeitsbereich werden Perspektiven in der Lösung [Application Insights-Connector](log-analytics-app-insights-connector.md) derzeit nicht unterstützt.  Eine Korrektur für dieses Problem wird derzeit untersucht.

## <a name="upgrade-process"></a>Upgradeprozess

### <a name="question-i-have-several-workspaces-can-i-upgrade-all-workspaces-at-the-same-time"></a>Frage: Ich besitze mehrere Arbeitsbereiche. Kann ich alle Arbeitsbereiche gleichzeitig upgraden?  
Nein.  Das Upgrade gilt für jeweils nur einen Arbeitsbereich. Aktuell können Sie nicht mehrere Arbeitsbereiche gleichzeitig upgraden. Beachten Sie, dass andere Benutzer des upgegradeten Arbeitsbereichs ebenfalls betroffen sind.  

### <a name="question-will-existing-log-data-collected-in-my-workspace-be-modified-if-i-upgrade"></a>Frage: Werden vorhandene Protokolldaten, die in meinem Arbeitsbereich gesammelt wurden, geändert, wenn ich ein Upgrade durchführe?  
Nein. Die verfügbaren Protokolldaten, die für Ihre Arbeitsbereich-Suchvorgänge verfügbar sind, sind durch das Upgrade nicht betroffen. Gespeicherte Suchvorgänge, Warnungen und Ansichten, werden automatisch in die neue Suchsprache konvertiert.  

### <a name="question-what-happens-if-i-dont-upgrade-my-workspace"></a>Frage: Was passiert, wenn ich kein Upgrade für meinen Arbeitsbereich durchführe?  
Die ältere Protokollsuche wird in den kommenden Monaten als veraltet gekennzeichnet. Arbeitsbereiche, die zu dieser Zeit noch nicht upgegradet worden sind, erhalten automatisch ein Upgrade.

### <a name="question-i-didnt-choose-to-upgrade-but-my-workspace-has-been-upgraded-anyway-what-happened"></a>Frage: Ich wollte kein Upgrade durchführen, jedoch wurde trotzdem eines für meinen Arbeitsbereich durchgeführt. Was ist passiert?  
Es kann sein, dass ein anderer Administrator dieses Arbeitsbereichs ein Upgrade durchgeführt hat. Beachten Sie, dass ein Arbeitsbereich automatisch upgegradet wird, wenn die neue Sprache allgemein verfügbar wird.  

### <a name="question-i-have-upgraded-by-mistake-and-now-need-to-cancel-it-and-restore-everything-back-what-should-i-do"></a>Frage: Ich habe aus Versehen ein Upgrade durchgeführt und muss es jetzt abbrechen und alles wiederherstellen. Wie soll ich vorgehen?  
Kein Problem.  Wir erstellen vor dem Upgrade eine Momentaufnahme Ihres Arbeitsbereichs, damit Sie ihn wiederherstellen können. Bedenken Sie, dass Suchvorgänge, Warnungen oder Ansichten, die Sie nach dem Upgrade gespeichert haben, trotzdem verloren gehen.  Um Ihre Arbeitsbereichsumgebung wiederherzustellen, befolgen Sie die Vorgehensweise unter [Kann ich nach einem Upgrade die vorherige Version wiederherstellen?](log-analytics-log-search-upgrade.md#can-i-go-back-after-i-upgrade).


## <a name="views"></a>Ansichten

### <a name="question-how-do-i-create-a-new-view-with-view-designer"></a>Frage: Wie erstelle ich mit dem Ansicht-Designer eine neue Ansicht?
Vor dem Upgrade konnten Sie mit dem Ansicht-Designer eine neue Ansicht aus einer Kachel im Hauptdashboard erstellen.  Nach dem Upgrade Ihres Arbeitsbereichs wird diese Kachel entfernt.  Sie können eine neue Ansicht mit dem Ansicht-Designer im OMS-Portal erstellen, indem Sie auf das grüne Pluszeichen (+) im linken Menü klicken.

### <a name="known-issue-see-all-option-for-line-charts-in-views-doesnt-result-in-a-line-chart"></a>Bekanntes Problem: Die Option „Alle anzeigen“ für Liniendiagramme in Ansichten führt nicht zu einem Liniendiagramm.
Wenn Sie in einer Ansicht unten im Bereich des Liniendiagramms auf die Option *Alle anzeigen* klicken, wird eine Tabelle angezeigt.  Vor dem Upgrade wurde anschließend ein Liniendiagramm angezeigt.  Dieses Problem wird auf eine potenzielle Änderung hin analysiert.


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum [Upgrade Ihres Arbeitsbereichs auf die neue Log Analytics-Abfragesprache](log-analytics-log-search-upgrade.md).

