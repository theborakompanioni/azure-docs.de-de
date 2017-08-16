---
title: Erstellen oder Importieren eines Runbooks in Azure Automation
description: Dieser Artikel beschreibt, wie Sie in Azure Automation ein neues Runbook erstellen oder ein Runbook aus einer Datei importieren.
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 24414362-b690-4474-8ca7-df18e30fc31d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2017
ms.author: magoedte;bwren
ms.translationtype: HT
ms.sourcegitcommit: caaf10d385c8df8f09a076d0a392ca0d5df64ed2
ms.openlocfilehash: 0264de12caaf62e976673a423df731ad27ab01e0
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="creating-or-importing-a-runbook-in-azure-automation"></a>Erstellen oder Importieren eines Runbooks in Azure Automation
Sie können Azure Automation ein Runbook hinzufügen, indem Sie entweder [ein neues Runbook erstellen](#creating-a-new-runbook) oder ein vorhandenes Runbook aus einer Datei oder aus dem [Runbook-Katalog](automation-runbook-gallery.md) importieren. Dieser Artikel enthält Informationen zum Erstellen und Importieren von Runbooks aus einer Datei.  Einzelheiten zum Zugreifen auf Community-Runbooks und Module erfahren Sie in [Runbook und Modulkataloge für Azure Automation](automation-runbook-gallery.md).

## <a name="creating-a-new-runbook"></a>Erstellen eines neuen Runbooks
Sie können ein neues Runbook in Azure Automation mithilfe eines der Azure-Portale oder Windows PowerShell erstellen. Sobald das Runbook erstellt wurde, können Sie es mithilfe der Informationen in [Grundlagen des PowerShell-Workflows](automation-powershell-workflow.md) und [Grafische Erstellung in Azure Automation](automation-graphical-authoring-intro.md) bearbeiten.

### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-classic-portal"></a>So erstellen Sie mit dem klassischen Azure-Portal ein neues Azure Automation-Runbook
Sie können im Azure-Portal nur [PowerShell-Workflow-Runbooks](automation-runbook-types.md#powershell-workflow-runbooks) verwenden.

1. Klicken Sie im klassischen Azure-Portal auf **Neu** > **App Services** > **Automation** > **Runbook** > **Schnellerfassung**.
2. Geben Sie die erforderlichen Informationen ein, und klicken Sie dann auf **Erstellen**. Der Runbook-Name muss mit einem Buchstaben beginnen und kann Buchstaben, Zahlen, Unterstriche und Bindestriche enthalten.
3. Wenn Sie das Runbook jetzt bearbeiten möchten, klicken Sie auf **Runbook bearbeiten**. Klicken Sie andernfalls auf **OK**.
4. Ihr neues Runbook wird auf der Registerkarte **Runbooks** angezeigt.

### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-portal"></a>So erstellen Sie ein neues Azure Automation-Runbook mit dem Azure-Portal
1. Öffnen Sie im Azure-Portal Ihr Automation-Konto.
2. Wählen Sie im Hub **Runbooks** aus, um die Liste der Runbooks zu öffnen.
3. Klicken Sie auf die Schaltfläche **Runbook hinzufügen** und anschließend auf **Neues Runbook erstellen**.
4. Geben Sie einen **Namen** für das Runbook ein, und wählen den [Typ](automation-runbook-types.md)aus. Der Runbook-Name muss mit einem Buchstaben beginnen und kann Buchstaben, Zahlen, Unterstriche und Bindestriche enthalten.
5. Klicken Sie auf **Erstellen** , um das Runbook zu erstellen und den Editor zu öffnen.

### <a name="to-create-a-new-azure-automation-runbook-with-windows-powershell"></a>So erstellen Sie ein neues Azure Automation-Runbook mit Windows PowerShell
Sie können das Cmdlet [New-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt619376.aspx) verwenden, um ein leeres [PowerShell-Workflow-Runbook](automation-runbook-types.md#powershell-workflow-runbooks) zu erstellen. Sie können den Parameter **Name** verwenden, um ein leeres Runbook zu erstellen, das Sie später bearbeiten können, oder Sie können den Parameter **Path** angeben, um eine Runbookdatei zu importieren. Der Parameter **Type** sollte auch eingeschlossen werden, um einen der vier Runbooktypen anzugeben.

Die folgenden Beispiele zeigen, wie Sie ein neues Automation-Anmeldeinformationsobjekt erstellen.

    New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
    -Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell

## <a name="importing-a-runbook-from-a-file-into-azure-automation"></a>Importieren eines Runbooks aus einer Datei in Azure Automation
Sie können ein neues Runbook in Azure Automation erstellen, indem Sie ein PowerShell-Skript, einen PowerShell-Workflow (.ps1-Erweiterung) oder ein exportiertes grafisches Runbook (.graphrunbook) importieren.  Geben Sie den [Runbook-Typ](automation-runbook-types.md) an, der beim Import erstellt wird. Berücksichtigen Sie dabei die folgenden Aspekte.

* Eine .graphrunbook-Datei kann nur in ein neues [grafisches Runbook](automation-runbook-types.md#graphical-runbooks)importiert werden, und grafische Runbooks können nur aus einer .graphrunbook-Datei erstellt werden.
* Eine .ps1-Datei mit einem PowerShell-Workflow kann nur in ein [PowerShell-Workflow- Runbook](automation-runbook-types.md#powershell-workflow-runbooks)importiert werden.  Enthält die Datei mehrere PowerShell-Workflows, schlägt der Import fehl. Sie müssen jeden Workflow in einer eigenen Datei speichern und einzeln importieren.
* Eine .ps1-Datei, die keinen Workflow enthält, kann in ein [PowerShell Runbook](automation-runbook-types.md#powershell-runbooks) oder ein [PowerShell-Workflow-Runbook](automation-runbook-types.md#powershell-workflow-runbooks) importiert werden.  Beim Importieren in einen PowerShell-Workflow Runbook wird es in einen Workflow konvertiert, und Kommentare zu den vorgenommenen Änderungen werden in das Runbook aufgenommen.

### <a name="to-import-a-runbook-from-a-file-with-the-azure-classic-portal"></a>So importieren Sie mit dem klassischen Azure-Portal ein Runbook aus einer Datei
Mit dem folgenden Verfahren können eine Skriptdatei in Azure Automation importieren.  Beachten Sie, dass Sie mit diesem Portal nur eine .ps1-Datei in ein PowerShell-Workflow-Runbook importieren können.  Für andere Typen müssen Sie das Azure-Portal verwenden.

1. Wählen Sie im Azure-Verwaltungsportal die Option **Automation** , und wählen Sie ein Automation-Konto aus.
2. Klicken Sie auf **Importieren**.
3. Klicken Sie auf **Datei suchen** und suchen Sie die zu importierende Skriptdatei.
4. Wenn Sie das Runbook jetzt bearbeiten möchten, klicken Sie auf **Runbook bearbeiten**. Klicken Sie andernfalls auf OK.
5. Das neue Runbook wird auf der **Runbooks** -Registerkarte für das Automation-Konto angezeigt.
6. Sie müssen [das Runbook veröffentlichen](#publishing-a-runbook) , bevor Sie es ausführen können.

### <a name="to-import-a-runbook-from-a-file-with-the-azure-portal"></a>So importieren Sie ein Runbook aus einer Datei mit dem Azure-Portal
Mit dem folgenden Verfahren können eine Skriptdatei in Azure Automation importieren.  

> [!NOTE]
> Beachten Sie, dass Sie über das Portal nur eine PS1-Datei in ein PowerShell-Workflow-Runbook importieren können.
> 
> 

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto.
2. Wählen Sie im Hub **Runbooks** aus, um die Liste der Runbooks zu öffnen.
3. Klicken Sie auf die Schaltfläche **Runbook hinzufügen** und anschließend auf **Importieren**.
4. Klicken Sie auf **Runbook-Datei** , um die zu importierende Datei auszuwählen.
5. Wenn das Feld **Name** aktiviert ist, haben Sie die Möglichkeit, ihn zu ändern.  Der Runbook-Name muss mit einem Buchstaben beginnen und kann Buchstaben, Zahlen, Unterstriche und Bindestriche enthalten.
6. Der [Runbooktyp](automation-runbook-types.md) wird automatisch ausgewählt, Sie können den Typ jedoch unter Berücksichtigung der geltenden Einschränkungen ändern. 
7. Das neue Runbook in der Liste der Runbooks für das Automation-Konto angezeigt.
8. Sie müssen [das Runbook veröffentlichen](#publishing-a-runbook) , bevor Sie es ausführen können.

> [!NOTE]
> Nach dem Import eines grafischen Runbooks oder eines grafischen PowerShell-Workflow-Runbooks können Sie diese Runbooks in den anderen Runbooktyp konvertieren. Sie können die Runbooks nicht in einen Texttyp konvertieren.
> 
> 

### <a name="to-import-a-runbook-from-a-script-file-with-windows-powershell"></a>So importieren Sie ein Runbook aus einer Skriptdatei mit Windows PowerShell
Sie können das Cmdlet [Import-AzureRMAutomationRunbook](https://msdn.microsoft.com/library/mt603735.aspx) verwenden, um eine Skriptdatei als PowerShell-Workflow-Runbookentwurf zu importieren. Wenn das Runbook bereits vorhanden ist, tritt beim Import ein Fehler auf, sofern Sie nicht den Parameter *-Force* verwenden. 

Die folgenden Beispielbefehle zeigen, wie Sie eine Skriptdatei in ein Runbook importieren.

    $automationAccountName =  "AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
    $RGName = "ResourceGroup"

    Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
    -ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
    -Type PowerShellWorkflow 


## <a name="publishing-a-runbook"></a>Veröffentlichen eines Runbooks
Wenn Sie ein neues Runbooks erstellen oder importieren, müssen Sie es veröffentlichen, bevor Sie es ausführen können.  Jedes Runbook in Automation umfasst eine Entwurfsversion und eine veröffentlichte Version. Nur die veröffentlichte Version kann ausgeführt werden, und nur die Entwurfsversion kann bearbeitet werden. Die veröffentlichte Version bleibt von Änderungen an der Entwurfsversion unberührt. Wenn die Entwurfsversion zur Verfügung gestellt werden soll, können Sie sie veröffentlichen und auf diese Weise die veröffentlichte Version mit der Entwurfsversion überschreiben.

## <a name="to-publish-a-runbook-using-the-azure-classic-portal"></a>So veröffentlichen Sie ein Runbook mithilfe des klassischen Azure-Portals
1. Öffnen Sie das Runbook im klassischen Azure-Portal.
2. Klicken Sie am oberen Bildschirmrand auf **Autor**.
3. Klicken Sie am unteren Bildschirmrand auf **Veröffentlichen**, und wählen Sie bei der Überprüfungsmeldung **Ja** aus.

## <a name="to-publish-a-runbook-using-the-azure-portal"></a>So veröffentlichen Sie ein Runbook mithilfe des Azure-Portals
1. Öffnen Sie das Runbook im Azure-Portal.
2. Klicken Sie auf die Schaltfläche **Edit** .
3. Klicken Sie auf **Veröffentlichen**, und wählen Sie bei der Überprüfungsmeldung **Ja** aus.

## <a name="to-publish-a-runbook-using-windows-powershell"></a>So veröffentlichen Sie ein Runbook mit Windows PowerShell
Sie können das Cmdlet [Publish-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603705.aspx) verwenden, um ein Runbook mit Windows PowerShell zu veröffentlichen. Die folgenden Beispielbefehle zeigen, wie Sie ein neues Beispiel-Runbook veröffentlichen.

    $automationAccountName =  AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $RGName = "ResourceGroup"

    Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
    -Name $runbookName -ResourceGroupName $RGName


## <a name="next-steps"></a>Nächste Schritte
* Informationen dazu, wie Sie vom Katalog mit Runbooks und PowerShell-Modulen profitieren, finden Sie unter [Runbook und Modulkataloge für Azure Automation](automation-runbook-gallery.md).
* Weitere Informationen zum Bearbeiten von PowerShell- und PowerShell-Workflow-Runbooks mit einem Text-Editor finden Sie unter [Bearbeiten von Textrunbooks in Azure Automation](automation-edit-textual-runbook.md)
* Weitere Informationen zum Erstellen von grafischen Runbooks finden Sie unter [Grafische Erstellung in Azure Automation](automation-graphical-authoring-intro.md)


