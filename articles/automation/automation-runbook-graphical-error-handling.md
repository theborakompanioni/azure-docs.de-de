---
title: Fehlerbehandlung in grafischen Azure Automation-Runbooks | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie die Fehlerbehandlungslogik in grafischen Azure Automation-Runbooks implementieren.
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/26/2016
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: f9b359691122da9e5d93e51f3085cad51e55d8f2
ms.openlocfilehash: 13c3e1693badcf4148738cb63666f34546d1696c

---

# <a name="error-handling-in-azure-automation-graphical-runbooks"></a>Fehlerbehandlung in grafischen Azure Automation-Runbooks

Eine wichtige Regel beim Entwerfen eines Runbooks, die Sie berücksichtigen sollten, ist die Identifizierung verschiedener möglicher Probleme für Runbooks, z.B. Erfolg, erwartete Fehlerzustände und unerwartete Fehlerbedingungen.  Runbooks sollten über eine entsprechende Fehlerbehandlung zur Erkennung von Fehlern verfügen.  Wenn Sie bei grafischen Runbooks die Ausgabe einer Aktivität überprüfen oder einen Fehler auf entsprechende Weise behandeln möchten, würden Sie vermutlich eine PowerShell-Codeaktivität folgen lassen, Bedingungslogik für den Ausgabelink der Aktivität definieren oder eine andere Methode anwenden.          

Wenn bei der Ausführung von Runbooks für eine Aktivität ein Fehler ohne Abbruch auftritt, wird die darauffolgende Aktivität häufig trotzdem verarbeitet.  Natürlich wird normalerweise eine Ausnahme generiert, aber der entscheidende Punkt ist, dass die Ausführung der nächsten Aktivität zugelassen wird. Der Grund für dieses Verhalten ist der Entwurf der PowerShell-Sprache in Bezug auf die Behandlung von Fehlern.    

Die PowerShell-Fehler, die während der Ausführung auftreten können, können mit oder ohne Abbruch erfolgen.  Unterschied:

* Fehler mit Abbruch: Ein schwerwiegender Fehler während der Ausführung, durch den der Befehl (bzw. die Skriptausführung) vollständig unterbrochen wird. Beispiele hierfür sind nicht vorhandene Cmdlets, Syntaxfehler, die die Ausführung eines Cmdlets verhindern, oder andere schwerwiegende Fehler.

* Fehler ohne Abbruch: Ein nicht schwerwiegender Fehler, bei dem die Ausführung fortgesetzt werden kann. Beispiele hierfür sind Ablauffehler wie „Datei nicht gefunden“, Berechtigungsprobleme usw.

Grafische Azure Automation-Runbooks wurden verbessert und um die Fehlerbehandlung erweitert. Sie können Ausnahmen jetzt in Fehler ohne Abbruch ändern und Fehlerlinks zwischen Aktivitäten erstellen. Autoren eines Runbooks können so Fehler abfangen und verfügen über einen Pfad zum Verwalten der erkannten bzw. unerwarteten Bedingung.  

## <a name="when-to-use"></a>Einsatzgebiete

Die Steuerung der Workflowausführung ist wichtig, um Folgendes sicherzustellen: Wenn bei einer kritischen Aktivität ein Fehler oder eine Ausnahme ausgelöst wird, können Sie das Fortfahren mit der nächsten Aktivität im Runbook verhindern und den Fehler entsprechend behandeln.  Dies ist vor allem erforderlich, wenn Sie mit Ihren Runbooks einen Unternehmens- oder Dienstleistungsprozess unterstützen.

Für jede Aktivität, für die ein Fehler erzeugt werden kann, kann der Autor des Runbooks einen Fehlerlink zu einer anderen Aktivität hinzufügen.  Die Zielaktivität kann einen beliebigen Typ haben: Codeaktivität, Aufruf eines Cmdlets, Aufruf eines anderen Runbooks oder etwas anderes. 

Außerdem kann die Zielaktivität auch über ausgehende Links verfügen. Dies können reguläre Links oder Fehlerlinks sein, sodass der Autor des Runbooks eine komplexe Fehlerbehandlungslogik implementieren kann, ohne eine Codeaktivität einbinden zu müssen.  Die empfohlene Vorgehensweise ist die Erstellung eines dedizierten Runbooks für die Fehlerbehandlung mit allgemeinen Funktionen. Dies ist aber nicht obligatorisch, und die Verwendung einer Fehlerbehandlungslogik in einer PowerShell-Codeaktivität ist nicht die einzige Alternative.  

Stellen Sie sich beispielsweise ein Runbook vor, mit dem versucht wird, eine VM zu starten und darauf eine Anwendung zu installieren. Wenn die VM nicht richtig gestartet wird, werden zwei Aktionen durchgeführt: 

1. Es wird eine Benachrichtigung zu diesem Problem gesendet.
2. Es wird ein anderes Runbook gestartet, mit dem automatisch eine neue VM als Ersatz bereitgestellt wird. 

Eine Lösung besteht darin, einen Fehlerlink zu verwenden, der auf eine Aktivität zur Behandlung von Schritt 1 zeigt, z.B. das **Write-Warning**-Cmdlet, das wiederum mit einer Aktivität für Schritt 2 verbunden ist, z.B. dem **Start-AzureRmAutomationRunbook**-Cmdlet. 

Sie können dieses Verhalten auch für die Verwendung in vielen Runbooks verallgemeinern und diese beiden Aktivitäten in ein separates Runbook für die Fehlerbehandlung einfügen, indem Sie die weiter oben vorgeschlagene Anleitung verwenden.  Vor dem Aufrufen dieses Runbooks für die Fehlerbehandlung können Sie beispielsweise aus den Daten im Originalrunbook eine benutzerdefinierte Nachricht erstellen und diese dann als Parameter an das Runbook für die Fehlerbehandlung übergeben. 

## <a name="how-to-use"></a>Gewusst wie

Jede Aktivität verfügt über eine Konfiguration zum Umwandeln von Ausnahmen in Fehler ohne Abbruch. Diese Einstellung ist standardmäßig deaktiviert.  Es ist ratsam, die Einstellung für alle Aktivitäten zu aktivieren, für die Sie Fehler behandeln möchten.  Durch das Aktivieren dieser Konfiguration stellen Sie sicher, dass sowohl Fehler mit und ohne Abbruch in der Aktivität als Fehler ohne Abbruch behandelt werden und dann per Fehlerlink verarbeitet werden können.  Nach dem Konfigurieren dieser Einstellung erstellen Sie eine Aktivität zum Behandeln des Fehlers.  Wenn eine Aktivität einen beliebigen Fehler erzeugt, werden die ausgehenden Fehlerlinks verwendet. Die regulären Links werden auch dann nicht verwendet, wenn mit der Aktivität auch eine reguläre Ausgabe erzeugt wurde.<br><br> ![Fehlerlink für Automation-Runbook – Beispiel](media/automation-runbook-graphical-error-handling/error-link-example.png)

Im folgenden einfachen Beispiel verwenden wir ein Runbook, mit dem eine Variable abgerufen wird. Sie enthält den Computernamen eines virtuellen Computers und versucht dann, den virtuellen Computer mit der nächsten Aktivität zu starten.<br><br> ![Fehlerbehandlung mit Automation-Runbook – Beispiel](media/automation-runbook-graphical-error-handling/runbook-example-error-handling.png)<br><br>      

Die **Get-AutomationVariable**-Aktivität und **Start-AzureRmVm** werden konfiguriert, um Ausnahmen in Fehler zu verwandeln.  Falls beim Abrufen der Variablen oder Starten der VM Probleme auftreten, werden Fehler generiert.<br><br> ![Fehlerbehandlung mit Automation-Runbook – Aktivitätseinstellungen](media/automation-runbook-graphical-error-handling/activity-blade-convertexception-option.png)

Fehlerlinks zeigen von diesen Aktivitäten auf eine Aktivität zur **Fehlerverwaltung** (Codeaktivität), die mit einem einfachen PowerShell-Ausdruck konfiguriert ist. Das Schlüsselwort *Throw* wird verwendet, um die Verarbeitung zu beenden, und mit *$Error.Exception.Message* wird die Meldung abgerufen, in der die aktuelle Ausnahme beschrieben wird.<br><br> ![Fehlerbehandlung mit Automation-Runbook – Codebeispiel](media/automation-runbook-graphical-error-handling/runbook-example-error-handling-code.png)


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Links und Grundlagen von Linktypen in grafischen Runbooks finden Sie unter [Grafische Erstellung in Azure Automation](automation-graphical-authoring-intro.md#links-and-workflow).

* Weitere Informationen zum Ausführen von Runbooks, zum Überwachen von Runbookaufträgen sowie andere technische Details finden Sie unter [Verfolgen eines Runbookauftrags](automation-runbook-execution.md) 


<!--HONumber=Jan17_HO1-->


