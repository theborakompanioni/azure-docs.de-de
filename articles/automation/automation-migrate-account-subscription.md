---
title: Migrieren von Automation-Konto und Ressourcen | Microsoft Docs
description: "Dieser Artikel beschreibt, wie Sie in Azure Automation ein Automation-Konto und zugehörige Ressourcen von einem Abonnement zu einem anderen verschieben."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
ms.assetid: 9c2db4a2-f324-48dc-8ce7-3343bf7230d5
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/21/2016
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: 98f5a017221f0aaba04e2b90afc674e4c42d1bb3
ms.openlocfilehash: 1c768e4e09f2b452804c65f751963a8d0c312c64


---
# <a name="migrate-automation-account-and-resources"></a>Migrieren von Automation-Konto und Ressourcen
Für Automation-Konten und die zugehörigen Ressourcen (d.h. Assets, Runbooks, Module usw.), die Sie im Azure-Portal erstellt haben und aus einer Ressourcengruppe in eine andere oder aus einem Abonnement in ein anderes migrieren möchten, können Sie dies problemlos mit der im Azure-Portal verfügbaren Option [Ressourcen verschieben](../resource-group-move-resources.md) realisieren. Bevor Sie allerdings mit dieser Aktion fortfahren, sollten Sie zunächst die folgende [Prüfliste vor dem Verschieben der Ressourcen](../azure-resource-manager/resource-group-move-resources.md#checklist-before-moving-resources) und darüber hinaus die spezifische Liste zur Automatisierung unten durchsehen.   

1. Das Zielabonnement/die Zielressourcengruppe muss sich in der gleichen Region wie die Quelle befinden.  Automation-Konten können also nicht Regionen übergreifend verschoben werden.
2. Beim Verschieben von Ressourcen (z.B. Runbooks, Aufträge, etc.) werden die Quellgruppe und die Zielgruppe für die Dauer des Vorgangs gesperrt. Schreib- und Löschvorgänge in den Gruppen werden bis zum Abschluss der Verschiebung blockiert.  
3. Alle Runbooks oder Variablen, die auf eine Ressource oder Abonnement-ID aus dem vorhandenen Abonnement verweisen, müssen nach Abschluss der Migration aktualisiert werden.   

> [!NOTE]
> Dieses Feature unterstützt keine Verschiebung klassischer Automatisierungsressourcen.
>
>

## <a name="to-move-the-automation-account-using-the-portal"></a>So verschieben Sie das Automation-Konto mithilfe des Portals
1. Klicken Sie in Ihrem Automation-Konto im oberen Blattbereich auf **Verschieben**.<br> ![Verschiebungsoption](media/automation-migrate-account-subscription/automation-menu-move.png)<br>
2. Beachten Sie auf dem Blatt **Ressourcen verschieben** , dass es Ressourcen anzeigt, die sowohl mit Ihrem Automation-Konto als auch mit Ihrer/Ihren Ressourcengruppe(n) in Zusammenhang stehen.  Wählen Sie **Abonnement** und **Ressourcengruppe** in den Dropdownlisten aus, oder wählen Sie die Option **Neue Ressourcengruppe erstellen** aus, und geben Sie einen neuen Ressourcengruppennamen in das dafür vorgesehene Feld ein.  
3. Überprüfen Sie das Kontrollkästchen, und aktivieren Sie es, um zu bestätigen, dass Sie *verstehen, dass Tools und Skripts nach dem Verschieben der Ressourcen aktualisiert werden müssen, um neue Ressourcen-IDs zu verwenden*, und klicken Sie dann auf **OK**.<br> ![Blatt „Ressourcen verschieben“](media/automation-migrate-account-subscription/automation-move-resources-blade.png)<br>   

Diese Aktion kann mehrere Minuten in Anspruch nehmen.  In **Benachrichtigungen**wird der Status jeder durchgeführten Aktion angezeigt – Validierung, Migration und schließlich der Abschluss.     

## <a name="to-move-the-automation-account-using-powershell"></a>So verschieben Sie das Automation-Konto mithilfe von PowerShell
Um vorhandene Automation-Ressourcen in eine andere Ressourcengruppe oder ein anderes Abonnement zu verschieben, verwenden Sie das Cmdlet **Get-AzureRmResource** zum Abrufen des bestimmten Automation-Kontos und dann das Cmdlet **Move-AzureRmResource** zum Ausführen des Verschiebevorgangs.

Das erste Beispiel zeigt das Verschieben eines Automation-Kontos in eine neue Ressourcengruppe.

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup"
   ```

Nachdem Sie das obige Codebeispiel ausgeführt haben, werden Sie aufgefordert, zu überprüfen, ob Sie diese Aktion ausführen möchten.  Wenn Sie auf **Ja** klicken und das Skript fortsetzen, erhalten Sie während der Migration keine Benachrichtigungen.  

Um Ressourcen in ein neues Abonnement zu verschieben, schließen Sie einen Wert für den *DestinationSubscriptionId* -Parameter ein.

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup" -DestinationSubscriptionId "SubscriptionId"
   ```

Wie im vorherigen Beispiel werden Sie aufgefordert, die Verschiebung zu bestätigen.  

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../resource-group-move-resources.md).
* Weitere Informationen zur rollenbasierten Zugriffssteuerung in Azure Automation finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md).
* Informationen zu PowerShell-Cmdlets zum Verwalten Ihres Abonnements finden Sie unter [Verwenden von Azure PowerShell mit Resource Manager](../powershell-azure-resource-manager.md)
* Informationen zu Portalfeatures zum Verwalten Ihres Abonnements finden Sie unter [Verwenden des Azure-Portals zum Verwalten von Ressourcen](../azure-portal/resource-group-portal.md).



<!--HONumber=Nov16_HO4-->


