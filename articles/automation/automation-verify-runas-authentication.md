---
title: "Überprüfen der Konfiguration des Azure Automation-Kontos | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie überprüfen, ob die Konfiguration Ihres Automation-Kontos richtig eingerichtet ist."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/14/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 5bed2616e15a2e5f52e79d0c28159b568e7a1de3
ms.lasthandoff: 04/15/2017

---

# <a name="test-azure-automation-run-as-account-authentication"></a>Testen der Authentifizierung für das ausführende Azure Automation-Konto
Nachdem die erfolgreiche Erstellung eines Automation-Kontos abgeschlossen ist, können Sie einen einfachen Test durchführen. Auf diese Weise können Sie bestätigen, dass die Authentifizierung für die Azure Resource Manager- oder klassische Azure-Bereitstellung möglich ist, indem Sie Ihr neu erstelltes oder aktualisiertes ausführendes Automation-Konto verwenden.    

## <a name="automation-run-as-authentication"></a>Authentifizierung mit ausführendem Automation-Konto

1. Öffnen Sie im Azure-Portal das zuvor erstellte Automation-Konto.  
2. Klicken Sie auf die Kachel **Runbooks** , um die Liste mit den Runbooks zu öffnen.
3. Wählen Sie das Runbook **AzureAutomationTutorialScript** aus, und klicken Sie auf **Starten**, um das Runbook zu starten.  Sie werden gefragt, ob Sie das Runbook starten möchten.
4. Es wird ein [Runbook-Auftrag](automation-runbook-execution.md) erstellt, das Blatt „Auftrag“ geöffnet und der Auftragsstatus auf der Kachel **Auftragszusammenfassung** angezeigt.  
5. Der Auftrag besitzt zunächst den Status *In Warteschlange* , um anzugeben, dass der Auftrag darauf wartet, dass in der Cloud ein Runbook Worker verfügbar wird. Wird der Auftrag von einem Worker übernommen, wechselt der Status zu *Wird gestartet* und anschließend zu *Wird ausgeführt*, wenn die Ausführung des Runbooks tatsächlich gestartet wurde.  
6. Wenn der Runbook-Auftrag abgeschlossen ist, sollte der Status **Abgeschlossen** lauten.<br> ![Sicherheitsprinzipal-Runbooktest](media/automation-verify-runas-authentication/job-summary-automationtutorialscript.png)<br>
7. Um die detaillierten Ergebnisse des Runbooks anzuzeigen, klicken Sie auf die Kachel **Ausgabe** .
8. Im Blatt **Ausgabe** sollte angezeigt werden, dass die Authentifizierung erfolgreich war und eine Liste aller verfügbaren Ressourcen in der Ressourcengruppe zurückgegeben wurde.
9. Schließen Sie das Blatt **Ausgabe**, um zum Blatt **Auftragszusammenfassung** zurückzukehren.
10. Schließen Sie das Blatt **Auftragszusammenfassung** und das entsprechende Runbook-Blatt **AzureAutomationTutorialScript**.

## <a name="classic-run-as-authentication"></a>Authentifizierung mit einem klassischen ausführenden Konto
Führen Sie die folgenden Schritte aus, wenn Sie Ressourcen unter dem klassischen Bereitstellungsmodell verwalten, um zu bestätigen, dass die Authentifizierung mit dem neuen klassischen ausführenden Konto möglich ist.     

1. Öffnen Sie im Azure-Portal das zuvor erstellte Automation-Konto.  
2. Klicken Sie auf die Kachel **Runbooks** , um die Liste mit den Runbooks zu öffnen.
3. Wählen Sie das Runbook **AzureClassicAutomationTutorialScript** aus, und klicken Sie auf **Starten**, um das Runbook zu starten.  Sie werden gefragt, ob Sie das Runbook starten möchten.
4. Es wird ein [Runbook-Auftrag](automation-runbook-execution.md) erstellt, das Blatt „Auftrag“ geöffnet und der Auftragsstatus auf der Kachel **Auftragszusammenfassung** angezeigt.  
5. Der Auftrag besitzt zunächst den Status *In Warteschlange* , um anzugeben, dass der Auftrag darauf wartet, dass in der Cloud ein Runbook Worker verfügbar wird. Wird der Auftrag von einem Worker übernommen, wechselt der Status zu *Wird gestartet* und anschließend zu *Wird ausgeführt*, wenn die Ausführung des Runbooks tatsächlich gestartet wurde.  
6. Wenn der Runbook-Auftrag abgeschlossen ist, sollte der Status **Abgeschlossen** lauten.<br><br> ![Sicherheitsprinzipal-Runbooktest](media/automation-verify-runas-authentication/job-summary-automationclassictutorialscript.png)<br>  
7. Um die detaillierten Ergebnisse des Runbooks anzuzeigen, klicken Sie auf die Kachel **Ausgabe** .
8. Auf dem Blatt **Ausgabe** sollte angezeigt werden, dass die Authentifizierung erfolgreich war und eine Liste mit allen klassischen VMs im Abonnement zurückgegeben wurde.
9. Schließen Sie das Blatt **Ausgabe**, um zum Blatt **Auftragszusammenfassung** zurückzukehren.
10. Schließen Sie das Blatt **Auftragszusammenfassung** und das entsprechende Runbook-Blatt **AzureClassicAutomationTutorialScript**.

## <a name="next-steps"></a>Nächste Schritte
* Erste Schritte mit PowerShell-Runbooks werden unter [Mein erstes PowerShell-Runbook](automation-first-runbook-textual-powershell.md) beschrieben.
* Weitere Informationen zur grafischen Inhaltserstellung finden Sie unter [Grafische Erstellung in Azure Automation](automation-graphical-authoring-intro.md).

