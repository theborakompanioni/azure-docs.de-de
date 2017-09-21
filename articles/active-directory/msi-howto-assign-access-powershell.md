---
title: "Zuweisen des Zugriffs auf eine Azure-Ressource für eine MSI mithilfe von PowerShell"
description: "Schrittweise Anweisungen für das Zuweisen von MSI zu einer Ressource und das Zuweisen des Zugriffs für eine andere Ressource mithilfe von PowerShell."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: bryanla
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: 3d032593b8eba89479829121a9745692e0f1f456
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-powershell"></a>Zuweisen des Zugriffs auf eine Ressource für eine MSI (Managed Service Identity, verwaltete Dienstidentität) mithilfe von PowerShell

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Nachdem Sie eine Azure-Ressource mit einer MSI konfiguriert haben, können Sie der MSI genau wie bei allen anderen Sicherheitsprinzipalen den Zugriff auf eine andere Ressource erteilen. In diesem Beispiel wird gezeigt, wie Sie der MSI eines virtuellen Azure-Computers mithilfe von PowerShell den Zugriff auf ein Azure-Speicherkonto gewähren.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../includes/msi-qs-configure-prereqs.md)]

Installieren Sie [Azure PowerShell, Version 4.3.1](https://www.powershellgallery.com/packages/AzureRM/4.3.1), sofern noch nicht geschehen.

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Verwenden Sie RBAC, um der MSI den Zugriff auf eine andere Ressource zuzuweisen.

Nachdem Sie MSI für eine Azure-Ressource aktiviert haben, [zum Beispiel für einen virtuellen Azure-Computer](msi-qs-configure-powershell-windows-vm.md), gehen Sie wie folgt vor:

1. Melden Sie sich bei Azure mithilfe des Cmdlets `Login-AzureRmAccount` an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, unter dem Sie die MSI konfiguriert haben:

   ```powershell
   Login-AzureRmAccount
   ```
2. In diesem Beispiel wird einem virtuellen Azure-Computer der Zugriff auf ein Speicherkonto gewährt. Zunächst wird [Get-AzureRMVM](/powershell/module/azurerm.compute/get-azurermvm) verwendet, um den Dienstprinzipal für den virtuellen Computer mit dem Namen „myVM“ abzurufen, der beim Aktivieren von MSI erstellt wurde. Dann wird [New-AzureRmRoleAssignment](/powershell/module/AzureRM.Resources/New-AzureRmRoleAssignment) verwendet, um dem virtuellen Computer „Reader“ den Zugriff auf ein Speicherkonto namens „MyStorageAcct“ zu erteilen:

    ```powershell
    $spID = (Get-AzureRMVM -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="troubleshooting"></a>Problembehandlung

Wenn die MSI für die Ressource nicht in der Liste der verfügbaren Identitäten angezeigt wird, stellen Sie sicher, dass die MSI ordnungsgemäß aktiviert wurde. In unserem Fall können wir zurück zum virtuellen Azure-Computer im [Azure-Portal](https://portal.azure.com) wechseln und wie folgt vorgehen:

- Überprüfen Sie auf der Seite „Konfiguration“, ob für die MSI-Aktivierung „Ja“ ausgewählt ist.
- Vergewissern Sie sich auf der Seite „Erweiterungen“, dass die MSI-Erweiterung erfolgreich bereitgestellt wurde.

Wenn einer dieser Punkte nicht zutrifft, müssen Sie die MSI möglicherweise erneut für die Ressource bereitstellen oder den Fehler bei der Bereitstellung beheben.

## <a name="related-content"></a>Verwandte Inhalte

- Einen Überblick über MSI finden Sie unter [Übersicht über verwaltete Dienstidentitäten](msi-overview.md).
- Informationen zum Aktivieren von MSI auf einem virtuellen Azure-Computer finden Sie unter [Konfigurieren einer Azure-VM-MSI (Managed Service Identity, verwaltete Dienstidentität) mithilfe von PowerShell](msi-qs-configure-powershell-windows-vm.md).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.


