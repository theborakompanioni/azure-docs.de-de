---
title: Runbookeinstellungen | Microsoft Docs
description: "Beschreibt die Konfigurationseinstellungen für ein Runbook in Azure Automation und stellt Informationen dazu bereit, wie diese Einstellungen mit dem Azure-Verwaltungsportal und Windows PowerShell geändert werden können."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
ms.assetid: a726f20c-a952-48b8-88ee-36d76aa3ac61
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/11/2016
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: ac8b5372aa06eac8c9a701f0621dbb675fbb565c
ms.openlocfilehash: 20ecbc270e61d234e026e6ba2634c7aad63b3355


---
# <a name="runbook-settings"></a>Runbookeinstellungen
Jedes Runbook in Azure Automation verfügt über mehrere Einstellungen, mit denen das Runbook identifiziert und sein Protokollierungsverhalten geändert werden kann. Nachfolgend wird jede dieser Einstellungen beschrieben, und Sie erfahren, wie die Einstellungen geändert werden können.

## <a name="settings"></a>Einstellungen
### <a name="name-and-description"></a>Name und Beschreibung
Sie können den Namen eines Runbooks nach dessen Erstellung nicht mehr ändern. Die Beschreibung ist optional und kann bis zu 512 Zeichen umfassen.

### <a name="tags"></a>Tags
Tags ermöglichen es Ihnen, bestimmte Wörter oder Ausdrücke zuzuweisen, um ein Runbook zu identifizieren. Wenn Sie beispielsweise ein Runbook zum [PowerShell-Katalog](https://www.powershellgallery.com/) hinzufügen, geben Sie mithilfe bestimmter Tags an, in welchen Kategorien das Runbook gelistet werden soll. Sie können mehrere Tags für ein Runbook angeben, wenn sie diese durch Kommas voneinander trennen.

### <a name="logging"></a>Protokollierung
Standardmäßig werden keine Verbose- und Progress-Datensätze in den Auftragsverlauf geschrieben. Sie können die Einstellungen für ein bestimmtes Runbook ändern, um diese Datensätze zu protokollieren. Weitere Informationen zu diesen Datensätzen finden Sie unter [Runbookausgabe und Meldungen](automation-runbook-output-and-messages.md).

## <a name="changing-runbook-settings"></a>Ändern von Runbookeinstellungen

### <a name="changing-runbook-settings-with-the-azure-portal"></a>Ändern von Runbookeinstellungen mit dem Azure-Portal
Sie können die Einstellungen für ein Runbook im Azure-Portal oder über das Blatt **Einstellungen** für das Runbook ändern.

1. Wählen Sie im Azure-Portal die Option **Automation** , und klicken Sie auf den Namen eines Automation-Kontos.
2. Wählen Sie die Registerkarte **Runbooks** .
3. Wenn Sie auf den Namen eines Runbooks klicken, werden Sie auf das Blatt „Einstellungen“ für das Runbook weitergeleitet. Hier können Sie Tags und die Runbookbeschreibung angeben oder ändern, die Protokollierung und Ablaufverfolgungseinstellungen konfigurieren und auf die Supporttools zum Lösen von Problemen zugreifen.     

### <a name="changing-runbook-settings-with-windows-powershell"></a>Ändern von Runbookeinstellungen mit Windows PowerShell
Sie können mit dem Cmdlet [Set-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603786.aspx) die Einstellungen für ein Runbook ändern. Wenn Sie mehrere Tags angeben möchten, können Sie entweder ein Array oder eine einzelne Zeichenfolge mit durch Kommas getrennten Werten zum Tags-Parameter hinzufügen. Sie rufen die aktuellen Tags mit [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx) ab.

Die folgenden Beispielbefehle zeigen, wie Sie die Eigenschaften für ein Runbook festlegen. In diesem Beispiel werden drei Tags zu den vorhandenen Tags hinzugefügt, und es wird festgelegt, dass Verbose-Datensätze protokolliert werden sollen.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="next-steps"></a>Nächste Schritte
* Unter [Runbookausgabe und -meldungen](automation-runbook-output-and-messages.md) erfahren Sie, wie Sie Ausgabe- und Fehlermeldungen erstellen und diese von Runbooks abrufen. 
* Grundlegendes zum Hinzufügen eines Runbooks, das bereits von der Community oder einer anderen Quelle entwickelt wurde, oder zum Erstellen eigener Runbooks finden Sie unter [Erstellen oder Importieren eines Runbooks](automation-creating-importing-runbook.md) 




<!--HONumber=Nov16_HO3-->


