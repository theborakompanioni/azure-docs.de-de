---
title: "Ausführen eines Runbooks in Azure Automation | Microsoft Docs"
description: "Beschreibt ausführlich, wie ein Runbook in Azure Automation verarbeitet wird."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: d10c8ce2-2c0b-4ea7-ba3c-d20e09b2c9ca
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/17/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: 847eb792064bd0ee7d50163f35cd2e0368324203
ms.openlocfilehash: 14f923e3f08dd3b286218ae56012ce14edcc4058
ms.contentlocale: de-de
ms.lasthandoff: 08/19/2017

---
# <a name="runbook-execution-in-azure-automation"></a>Ausführen von Runbooks in Azure Automation
Wenn Sie ein Runbook in Azure Automation starten, wird ein Auftrag erstellt. Ein Auftrag ist eine einzelne Ausführungsinstanz eines Runbooks. Für die Ausführung jedes Auftrags wird ein Azure Automation-Worker zugewiesen. Wenngleich Worker von mehreren Azure-Konten gemeinsam genutzt werden, sind die Aufträge von verschiedenen Automation-Konten voneinander isoliert. Sie können nicht steuern, welcher Worker die Anforderung für Ihren Auftrag verarbeitet.  Für ein einzelnes Runbook können mehrere Aufträge gleichzeitig ausgeführt werden. Wenn Sie die Liste der Runbooks im Azure-Portal anzeigen, wird der Status aller Aufträge aufgelistet, die für jedes Runbook gestartet wurden. Sie können die Liste der Aufträge für jedes Runbook anzeigen, um den Status der einzelnen Aufträge nachzuverfolgen. Eine Beschreibung der verschiedenen Auftragsstatusangaben finden Sie unter [Auftragsstatuswerte](#job-statuses).

Das folgende Diagramm zeigt den Lebenszyklus eines Runbookauftrags für [grafische Runbooks](automation-runbook-types.md#graphical-runbooks) und [PowerShell-Workflow-Runbooks](automation-runbook-types.md#powershell-workflow-runbooks).

![Auftragsstatus – PowerShell-Workflow](./media/automation-runbook-execution/job-statuses.png)

Das folgende Diagramm zeigt den Lebenszyklus eines Runbookauftrags für [PowerShell-Runbooks](automation-runbook-types.md#powershell-runbooks).

![Auftragsstatus – PowerShell-Skript](./media/automation-runbook-execution/job-statuses-script.png)

Ihre Aufträge können auf Ihre Azure-Ressourcen zugreifen, indem sie eine Verbindung mit Ihrem Azure-Abonnement herstellen. Sie besitzen nur Zugriff auf Ressourcen in Ihrem Rechenzentrum, wenn aus der öffentlichen Cloud auf diese Ressourcen zugegriffen werden kann.

## <a name="job-statuses"></a>Auftragsstatuswerte
Die folgende Tabelle beschreibt die verschiedenen Status, die für einen Auftrag möglich sind.

| Status | Beschreibung |
|:--- |:--- |
| Abgeschlossen |Der Auftrag wurde erfolgreich abgeschlossen. |
| Fehler |Bei [grafischen und PowerShell-Workflow-Runbooks](automation-runbook-types.md)ist die Kompilierung des Runbooks fehlgeschlagen.  Bei [PowerShell-Skript-Runbooks](automation-runbook-types.md)konnte das Runbook nicht gestartet werden, oder bei dem Auftrag ist eine Ausnahme aufgetreten. |
| Fehler, auf Ressourcen wird gewartet |Beim Auftrag ist ein Fehler aufgetreten, da der Auftrag dreimal den Grenzwert für die [gleichmäßige Verteilung](#fairshare) erreicht hat und jedes Mal vom gleichen Prüfpunkt oder vom Anfang des Runbooks gestartet wurde. |
| In Warteschlange |Der Auftrag wartet darauf, dass Ressourcen für einen Automation-Worker verfügbar werden, damit er gestartet werden kann. |
| Wird gestartet |Der Auftrag wurde einem Worker zugewiesen, und das System ist in Begriff, ihn zu starten. |
| Wird fortgesetzt |Das System ist in Begriff, den Auftrag fortzusetzen, nachdem er angehalten wurde. |
| Wird ausgeführt |Der Auftrag wird ausgeführt. |
| Wird ausgeführt, auf Ressourcen wird gewartet |Der Auftrag wurde entladen, da er den Grenzwert für die [gleichmäßige Verteilung](#fairshare) erreicht hat. Er wird in Kürze vom letzten Prüfpunkt wiederaufgenommen. |
| Beendet |Der Auftrag wurde vom Benutzer beendet, bevor er abgeschlossen wurde. |
| Wird beendet |Das System ist in Begriff, den Auftrag zu beenden. |
| Ausgesetzt |Der Auftrag wurde vom Benutzer, vom System oder von einem Befehl im Runbook angehalten. Ein Auftrag, der angehalten wurde, kann erneut gestartet werden und wird vom letzten Prüfpunkt oder vom Anfang des Runbooks fortgesetzt, wenn er keine Prüfpunkte besitzt. Das Runbook wird nur vom System angehalten, wenn eine Ausnahme auftritt. Standardmäßig ist ErrorActionPreference auf **Continue** festgelegt. Dies bedeutet, dass der Auftrag bei einem Fehler weiterhin ausgeführt wird. Wenn diese Einstellungsvariable auf **Stop** festgelegt wird, wird der Auftrag bei einem Fehler angehalten.  Gilt nur für [grafische und PowerShell-Workflow-Runbooks](automation-runbook-types.md) . |
| Wird angehalten |Das System versucht, den Auftrag auf Anforderung des Benutzers anzuhalten. Das Runbook muss den nächsten Prüfpunkt erreichen, bevor es angehalten werden kann. Wenn der letzte Prüfpunkt bereits verstrichen ist, wird das Runbook abgeschlossen, bevor es angehalten werden kann.  Gilt nur für [grafische und PowerShell-Workflow-Runbooks](automation-runbook-types.md) . |

## <a name="viewing-job-status-from-the-azure-portal"></a>Anzeigen des Auftragsstatus im Azure-Portal
Im Azure-Portal können Sie eine Statuszusammenfassung aller Runbookaufträge oder Details zu einem bestimmten Runbookauftrag anzeigen. Per Konfiguration können Sie zu diesem Zweck außerdem die Integration mit Ihrem Microsoft Operations Management Suite (OMS) Log Analytics-Arbeitsbereich herstellen und dann Statusinformationen zu Runbookaufträgen sowie Auftragsdatenströme weiterleiten.  Weitere Informationen finden Sie unter [Weiterleiten von Auftragsstatus und Auftragsdatenströmen von Automation an Log Analytics (OMS)](automation-manage-send-joblogs-log-analytics.md).  

### <a name="automation-runbook-jobs-summary"></a>Automation-Runbookaufträge als Zusammenfassung
Rechts in Ihrem ausgewählten Automation-Konto sehen Sie auf der Kachel **Auftragsstatistik** die Zusammenfassung aller Runbookaufträge für das ausgewählte Automation-Konto.<br><br> Die Kachel ![Auftragsstatistik](./media/automation-runbook-execution/automation-account-job-status-summary.png)<br> Auf dieser Kachel wird die Anzahl aller ausgeführten Jobs neben einer grafischen Darstellung des Auftragsstatus angezeigt.  

Durch Klicken auf die Kachel wird das Blatt **Aufträge** sichtbar, dem Sie eine zusammenfassende Liste aller ausgeführten Aufträge ebenso wie den Status, den Beginn der Ausführung und den Abschluss entnehmen können.<br><br> ![Das Blatt „Aufträge“ im Automation-Konto](./media/automation-runbook-execution/automation-account-jobs-status-blade.png)<br><br>  Sie können die Auftragsliste filtern, indem Sie die Option **Filteraufträge** auswählen. Aufträge lassen sich zum Durchsuchen nach bestimmten Runbooks, dem Auftragsstatus oder, über die Dropdownliste, nach dem Datums- und Uhrzeitbereich filtern.<br><br> ![Status von Filteraufträgen](./media/automation-runbook-execution/automation-account-jobs-filter.png)

Alternativ können Sie für ein bestimmtes Runbook eine Übersicht anzeigen, indem Sie in Ihrem Automation-Konto zuerst auf dem Blatt **Runbooks** das Runbook und dann die Kachel **Aufträge** auswählen.  Dadurch rufen Sie das Blatt **Aufträge** auf. Wenn Sie dort auf einen Auftragsdatensatz klicken, werden Details und die Ausgabe angezeigt.<br><br> ![Das Blatt „Aufträge“ im Automation-Konto](./media/automation-runbook-execution/automation-runbook-job-summary-blade.png)<br> 

### <a name="job-summary"></a>API-Zusammenfassung
Sie können eine Liste aller Aufträge, die für ein bestimmtes Runbook erstellt wurden, sowie deren aktuellen Status anzeigen. Sie können diese Liste nach Auftragsstatus und dem Datumsbereich für die letzte Änderung des Auftrags filtern. Um ausführliche Informationen und die Auftragsausgabe anzuzeigen, klicken Sie auf den Namen eines Auftrags. Die Detailansicht des Auftrags enthält die Werte für die Runbookparameter, die für diesen Auftrag bereitgestellt wurden.

Zeigen Sie die Aufträge für ein Runbook mithilfe der folgenden Schritte an.

1. Wählen Sie im Azure-Portal die Option **Automation** aus, und wählen Sie anschließend den Namen eines Automation-Kontos aus.
2. Wählen Sie im Hub die Option **Runbooks** aus, und wählen Sie dann auf dem Blatt **Runbooks** ein Runbook in der Liste aus.
3. Klicken Sie auf dem Blatt für das ausgewählte Runbook auf die Kachel **Aufträge**.
4. Wenn Sie auf einen der Aufträge in der Liste klicken, können Sie auf dem Blatt für die Runbook-Auftragsdetails die Details und die Ausgabe anzeigen.

## <a name="retrieving-job-status-using-windows-powershell"></a>Abrufen des Auftragsstatus mithilfe von Windows PowerShell
Sie können [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) verwenden, um die für ein Runbook erstellten Aufträge und die Details zu einem bestimmten Auftrag anzuzeigen. Wenn Sie ein Runbook über Windows PowerShell mithilfe des Befehls [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx)starten, wird der resultierende Auftrag zurückgegeben. Verwenden Sie [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx), um die Ausgabe eines Auftrags abzurufen.

Die folgenden Beispielbefehle rufen den letzten Auftrag für ein Beispielrunbook ab und zeigen seinen Status, die für die Runbookparameter bereitgestellten Werte und die Ausgabe des Auftrags an.

    $job = (Get-AzureRmAutomationJob –AutomationAccountName "MyAutomationAccount" `
    –RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate –desc)[0]
    $job.Status
    $job.JobParameters
    Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAcct" -Id $job.JobId –Stream Output

## <a name="fair-share"></a>gleichmäßige Verteilung
Damit Ressourcen von allen Runbooks in der Cloud verwendet werden können, entlädt Azure Automation jeden Auftrag vorübergehend, nachdem er drei Stunden lang ausgeführt wurde.  Während dieser Zeit werden Aufträge für [PowerShell-Runbooks](automation-runbook-types.md#powershell-runbooks) beendet und nicht neu gestartet.  Als Auftragsstatus wird **Beendet** angezeigt.  Diese Art Runbooks wird immer von Beginn an neu gestartet, da sie keine Prüfpunkte unterstützt.  

[PowerShell-Workflow-Runbooks](automation-runbook-types.md#powershell-workflow-runbooks) werden ab ihrem letzten [Prüfpunkt](https://docs.microsoft.com/system-center/sma/overview-powershell-workflows#bk_Checkpoints) fortgesetzt.  Nach dem Ausführen über drei Stunden wird der Runbook-Auftrag vom Dienst angehalten, und als Status wird **Wird ausgeführt, warten auf Ressourcen** angezeigt.  Wird eine Sandbox verfügbar, startet Automation-Dienst das Runbook automatisch neu und setzt es ab dem letzten Prüfpunkt fort.  Dies ist ein normales Verhalten des PowerShell-Workflows für Anhalten/Neustarten.  Wenn das Runbook wieder drei Stunden Ausführungszeit überschreitet, wird der Vorgang bis zu dreimal wiederholt.  Wenn das Runbook nach dem dritten Neustart nicht in drei Stunden abgeschlossen wird, wird ein Fehler für den Runbook-Auftrag ausgegeben, und als Auftragsstatus wird **Fehler, warten auf Ressourcen** angezeigt.  In diesem Fall wird ein Ausnahmefehler mit der folgenden Meldung angezeigt.

*Die Ausführung des Auftrags kann nicht fortgesetzt werden, da er wiederholt am selben Prüfpunkt entfernt wurde. Stellen Sie sicher, dass Ihr Runbook Vorgänge mit langer Ausführungsdauer nicht ausführt, ohne deren Zustand dauerhaft zu speichern.*

Dieses Verhalten schützt den Dienst davor, dass Runbooks unbegrenzt ausgeführt werden, da der nächste Prüfpunkt nicht ohne Entladen erreicht werden kann.

Wenn das Runbook keine Prüfpunkte enthält oder der Auftrag den ersten Prüfpunkt vor dem Entladen nicht erreicht hat, wird der Auftrag vom Anfang neu gestartet.  

Sorgen Sie beim Erstellen eines Runbooks dafür, dass die Zeit zum Ausführen von Aktivitäten zwischen zwei Prüfpunkten drei Stunden nicht überschreitet. Sie müssen Ihrem Runbook ggf. Prüfpunkte hinzufügen oder Vorgänge mit langen Ausführungszeiten aufteilen, um sicherzustellen, dass dieser Grenzwert von drei Stunden nicht erreicht wird. Angenommen, Ihr Runbook führt eine Neuindizierung für eine große SQL-Datenbank durch. Wenn dieser einzelne Vorgang nicht innerhalb des Grenzwerts für die gleichmäßige Verteilung abgeschlossen wird, wird der Auftrag entladen und vom Anfang neu gestartet. In diesem Fall sollten Sie den Neuindizierungsvorgang in mehrere Schritte unterteilen (z. B. nur jeweils eine Tabelle gleichzeitig neu indizieren) und dann einen Prüfpunkt nach jedem Vorgang einfügen, damit der Auftrag nach dem letzten Vorgang bis zum Abschluss fortgesetzt werden kann.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu den verschiedenen Methoden, die zum Starten eines Runbooks in Azure Automation verwendet werden können, finden Sie unter [Starten eines Runbooks in Azure Automation](automation-starting-a-runbook.md).


