---
title: Untergeordnete Runbooks in Azure Automation | Microsoft Docs
description: Beschreibt die verschiedenen Methoden, die verwendet werden können, um ein Runbook in Azure Automation über ein anderes Runbook zu starten und Informationen zwischen über- und untergeordnetem Runbook freizugeben.
services: automation
documentationcenter: ''
author: mgoedtel
manager: jwhit
editor: tysonn

ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/17/2016
ms.author: magoedte;bwren

---
# Untergeordnete Runbooks in Azure Automation
Es ist eine bewährte Methode in Azure Automation, wieder verwendbare, modulare Runbooks mit einer diskreten Funktion zu schreiben, die von anderen Runbooks verwendet werden können. Ein übergeordnetes Runbook ruft häufig untergeordnete Runbooks zum Durchführen erforderlicher Funktionen auf. Zum Aufrufen eines untergeordneten Runbooks stehen zwei Methoden zur Auswahl, die sich erheblich unterscheiden. Um die beste Methode für Ihre verschiedenen Szenarien bestimmen zu können, sollten Sie sich daher zunächst mit diesen Unterschieden vertraut machen.

## Aufrufen eines untergeordneten Runbooks mittels Inlineausführung
Um ein Runbook inline über ein anderes Runbook aufzurufen, verwenden Sie den Namen des Runbooks und geben Werte für die zugehörigen Parameter wie bei einer Aktivität oder einem Cmdlet an. Alle Runbooks im selben Automation-Konto können auf diese Weise von allen anderen Runbooks verwendet werden. Das übergeordnete Runbook wartet auf den Abschluss des untergeordneten Runbooks, bevor es mit der nächsten Zeile fortfährt, und sämtliche Ausgaben werden direkt an das übergeordnete Runbook zurückgegeben.

Wenn Sie ein Runbook inline aufrufen, wird es im selben Auftrag ausgeführt wie das übergeordnete Runbook. Das ausgeführte untergeordnete Runbook wird nicht im Auftragsverlauf angezeigt. Alle Ausnahmen und Datenstromausgaben des untergeordneten Runbooks werden dem übergeordneten Runbook zugeordnet. Dies verringert die Anzahl von Aufträgen und vereinfacht die Nachverfolgung sowie die Problembehandlung, da alle vom untergeordneten Runbook ausgelösten Ausnahmen und seine Datenstromausgaben dem übergeordneten Auftrag zugeordnet werden.

Beim Veröffentlichen eines Runbooks müssen alle von ihm aufgerufenen untergeordneten Runbooks bereits veröffentlicht sein. Dies ist erforderlich, weil Azure Automation beim Kompilieren des Runbooks eine Zuordnung zu allen untergeordneten Runbooks erstellt. Sind diese nicht veröffentlicht, lässt sich das übergeordnete Runbook scheinbar ohne Fehler veröffentlichen, bei seinem Start wird jedoch eine Ausnahme generiert. In diesem Fall können Sie das übergeordnete Runbook erneut veröffentlichen, um korrekt auf die untergeordneten Runbooks zu verweisen. Sie müssen das übergeordnete Runbook nicht erneut veröffentlichen, wenn eines der untergeordneten Runbooks geändert wird, da die Zuordnung bereits erstellt wurde.

Bei den Parametern eines inline aufgerufenen untergeordneten Runbooks kann es sich um beliebige Datentypen handeln, auch um komplexe Objekte. Zudem findet anders als beim Starten des Runbooks über das Azure-Verwaltungsportal oder mit dem Cmdlet „Start-AzureRmAutomationRunbook“ keine [JSON-Serialisierung](automation-starting-a-runbook.md#runbook-parameters) statt.

### Runbooktypen
Diese Runbooktypen können sich gegenseitig aufrufen:

* Ein [PowerShell-Runbook](automation-runbook-types.md#powershell-runbooks) und [grafische Runbooks](automation-runbook-types.md#graphical-runbooks) können sich gegenseitig inline aufrufen (beide sind PowerShell-basiert).
* Ein [PowerShell-Workflow-Runbook](automation-runbook-types.md#powershell-workflow-runbooks) und grafische PowerShell-Workflow-Runbooks können sich gegenseitig inline aufrufen (beide sind PowerShell-Workflow-basiert).
* Die PowerShell-Runbooktypen und die PowerShell-Workflow-Runbooktypen können sich nicht gegenseitig inline aufrufen und müssen Start-AzureRmAutomationRunbook verwenden.

Bedeutung der Reihenfolge der Veröffentlichung:

* Die Veröffentlichungsreihenfolge von Runbooks spielt nur für PowerShell-Workflow- und grafische PowerShell-Workflow-Runbooks eine Rolle.

Wenn Sie ein untergeordnetes grafisches oder PowerShell-Workflow-Runbook mit Inlineausführung aufrufen, verwenden Sie einfach den Namen des Runbooks. Beim Aufrufen eines untergeordneten PowerShell-Runbooks muss seinem Namen *.\* vorangestellt werden, um anzugeben, dass sich das Skript im lokalen Verzeichnis befindet.

### Beispiel
Im folgenden Beispiel wird ein untergeordnetes Testrunbook aufgerufen, das drei Parameter akzeptiert – ein komplexes Objekt, eine ganze Zahl und einen booleschen Wert. Die Ausgabe des untergeordneten Runbooks wird einer Variablen zugewiesen. In diesem Fall ist das untergeordnete Runbook ein PowerShell-Workflow-Runbook.

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true

Das folgende Beispiel ähnelt dem vorherigen Beispiel, mit dem Unterschied, dass ein PowerShell-Runbook als untergeordnetes Runbook verwendet wird.

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = .\PS-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true



## Starten eines untergeordneten Runbooks mithilfe eines Cmdlets
Sie können das Cmdlet [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) verwenden, um ein Runbook wie unter [Starten eines Runbooks mit Windows PowerShell](automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell) beschrieben zu starten. Für die Verwendung dieses Cmdlets gibt es zwei Modi. Im ersten Modus gibt das Cmdlet die Auftrags-ID zurück, sobald der untergeordnete Auftrag für das untergeordnete Runbook erstellt wird. Im zweiten Modus, den Sie durch Angeben des Parameters **-wait** aktivieren, wartet das Cmdlet, bis der untergeordnete Auftrag abgeschlossen ist, und gibt erst dann die Ausgabe aus dem untergeordneten Runbook zurück.

Der Auftrag eines mit einem Cmdlet gestarteten untergeordneten Runbooks wird in einem vom übergeordneten Runbook getrennten Auftrag ausgeführt. Im Vergleich zum Inlineaufruf des Runbooks führt dies zu einer größeren Anzahl von Aufträgen und erschwert die Nachverfolgung. Das übergeordnete Runbook kann mehrere untergeordnete Runbooks asynchron starten, ohne auf ihren Abschluss zu warten. Für diese Art der parallelen Ausführung, bei der untergeordnete Runbooks inline aufgerufen werden, muss das übergeordnete Runbook das Schlüsselwort [parallel](automation-powershell-workflow.md#parallel-processing) verwenden.

Parameter für ein mittels Cmdlet gestartetes untergeordnetes Runbook werden wie unter [Runbook-Parameter](automation-starting-a-runbook.md#runbook-parameters) beschrieben als Hashtabelle bereitgestellt. Es können nur einfache Datentypen verwendet werden. Enthält das Runbook einen Parameter mit einem komplexen Datentyp, muss es inline aufgerufen werden.

### Beispiel
Im folgenden Beispiel wird ein untergeordnetes Runbook mit Parametern gestartet und anschließend auf seinen Abschluss gewartet. Dabei wird „Start-AzureRmAutomationRunbook“ mit dem Parameter „-wait“ verwendet. Danach wird die Ausgabe aus dem untergeordneten Runbook abgerufen.

    $params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true} 
    $joboutput = Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-ChildRunbook" -ResouceGroupName "LabRG" –Parameters $params –wait


## Vergleich der Methoden zum Aufrufen eines untergeordneten Runbooks
Die folgende Tabelle enthält eine Zusammenfassung der Unterschiede zwischen den beiden Methoden zum Aufrufen eines untergeordneten Runbooks.

|  | Inline | Cmdlet |
|:--- |:--- |:--- |
| Auftrag |Untergeordnete Runbooks werden im selben Auftrag ausgeführt wie das übergeordnete Runbook. |Für das untergeordnete Runbook wird ein separater Auftrag erstellt. |
| Ausführung |Das übergeordnete Runbook wird erst nach Abschluss des untergeordneten Runbooks fortgesetzt. |Das übergeordnete Runbook wird sofort fortgesetzt, nachdem das untergeordnete Runbook gestartet wurde, *oder* das übergeordnete Runbook wartet, bis der untergeordnete Auftrag abgeschlossen ist. |
| Ausgabe |Ausgaben des untergeordneten Runbooks werden direkt an das übergeordnete Runbook zurückgegeben. |Das übergeordnete Runbook muss die Ausgabe aus dem untergeordneten Runbookauftrag abrufen, *oder* das übergeordnete Runbook kann die Ausgabe direkt aus dem untergeordneten Runbook abrufen. |
| Parameter |Werte für die Parameter des untergeordneten Runbooks werden separat angegeben, und es können beliebige Datentypen verwendet werden. |Werte für die Parameter des untergeordneten Runbooks müssen in einer einzigen Hashtabelle kombiniert werden, und es können nur einfache, Array- und Objektdatentypen, die JSON-Serialisierung nutzen, verwendet werden. |
| Automation-Konto |Das übergeordnete Runbook kann nur im selben Automation-Konto enthaltene untergeordnete Runbooks verwenden. |Das übergeordnete Runbook kann untergeordnete Runbooks aus allen Automation-Konten desselben Azure-Abonnements und sogar eines anderen Abonnements verwenden, wenn eine Verbindung damit hergestellt wurde. |
| Veröffentlichung |Das untergeordnete Runbook muss vor der Veröffentlichung des übergeordneten Runbooks veröffentlicht werden. |Das untergeordnete Runbook muss vor dem Start des übergeordneten Runbooks veröffentlicht werden. |

## Nächste Schritte
* [Starten eines Runbooks in Azure Automation](automation-starting-a-runbook.md)
* [Runbookausgabe und -meldungen in Azure Automation](automation-runbook-output-and-messages.md)

<!---HONumber=AcomDC_0817_2016-->