---
title: Konfigurieren von MSI auf einem virtuellen Azure-Computer mithilfe einer Vorlage
description: "Schrittweise Anweisungen zum Konfigurieren einer verwalteten Dienstidentität (Managed Service Identity, MSI) auf einem virtuellen Azure-Computer mithilfe einer Azure Resource Manager-Vorlage."
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
ms.openlocfilehash: 266458323ca54d9805aea12108faed79e69d30b0
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="configure-a-vm-managed-service-identity-msi-using-a-template"></a>Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) mithilfe einer Vorlage

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie MSI für einen virtuellen Azure-Windows-Computer mithilfe einer Azure Resource Manager-Bereitstellungsvorlage aktivieren und entfernen.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../includes/msi-qs-configure-prereqs.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Aktivieren von MSI beim Erstellen eines virtuellen Azure-Computers oder auf einem vorhandenen virtuellen Computer

Wie beim Azure-Portal und der Skripterstellung bieten Azure Resource Manager-Vorlagen die Möglichkeit, von einer Azure-Ressourcengruppe definierte neue bzw. geänderte Ressourcen bereitzustellen. Für die Vorlagenbearbeitung und -bereitstellung sind mehrere Optionen sowohl lokal als auch portal-/webbasiert verfügbar. Einige Beispiele:

   - Verwenden einer [benutzerdefinierten Vorlage vom Azure Marketplace](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template). Dies ermöglicht es Ihnen, eine ganz neue Vorlage zu erstellen oder eine Vorlage zu erstellen, die auf einer vorhandenen, oft verwendeten Vorlage oder einer [Schnellstartvorlage](https://azure.microsoft.com/documentation/templates/) basiert.
   - Ableiten von einer vorhandenen Ressourcengruppe, indem eine Vorlage aus [der ursprünglichen Bereitstellung](../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history) oder aus dem [aktuellen Status der Bereitstellung](../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group) exportiert wird.
   - Verwenden eines lokalen [JSON-Editors (z.B. VS Code)](../azure-resource-manager/resource-manager-create-first-template.md) und anschließendes Hochladen/Bereitstellen mithilfe von PowerShell oder CLI.
   - Verwenden des [Azure-Ressourcengruppenprojekts](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) von Visual Studio für das Erstellen und das Bereitstellen der Vorlage.  

Die Vorlagensyntax ist während der ursprünglichen Bereitstellung und der erneuten Bereitstellung identisch, unabhängig davon, für welche Möglichkeit Sie sich entscheiden. Das bedeutet, die Aktivierung von MSI auf einem neuen oder einem vorhandenen virtuellen Computer erfolgt auf dieselbe Weise. Standardmäßig führt Azure Resource Manager außerdem ein [inkrementelles Update](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) für Bereitstellungen durch:

1. Nachdem die Vorlage in einen Editor geladen wurde, suchen Sie die gewünschte `Microsoft.Compute/virtualMachines`-Ressource im Abschnitt `resources`. Dieser Screenshot weicht möglicherweise geringfügig von der Darstellung bei Ihnen ab. Dies hängt davon ab, welchen Editor Sie verwenden und ob Sie eine Vorlage für eine neue oder eine vorhandene Bereitstellung bearbeiten:

   >[!NOTE] 
   > Für Schritt 2 wird davon ausgegangen, dass die Variablen `vmName`, `storageAccountName` und `nicName` in der Vorlage definiert werden.
   >

   ![Vorlage vor dem Screenshot – Suchen des virtuellen Computers](./media/msi-qs-configure-template-windows-vm/template-file-before.png) 

2. Fügen Sie die `"identity"`-Eigenschaft auf derselben Ebene wie die `"type": "Microsoft.Compute/virtualMachines"`-Eigenschaft hinzu. Verwenden Sie hierfür die folgende Syntax:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

3. Fügen Sie dann die VM-MSI-Erweiterung als `resources`-Element hinzu. Verwenden Sie hierfür die folgende Syntax:

   >[!NOTE] 
   > Im folgenden Beispiel wird angenommen, dass eine Windows-VM-Erweiterung (`ManagedIdentityExtensionForWindows`) bereitgestellt wird. Mithilfe von `ManagedIdentityExtensionForLinux` ist stattdessen auch eine Konfiguration für Linux möglich.
   >

   ```JSON
   { 
       "type": "Microsoft.Compute/virtualMachines/extensions",
       "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
       "apiVersion": "2016-03-30",
       "location": "[resourceGroup().location]",
       "dependsOn": [
           "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
       ],
       "properties": {
           "publisher": "Microsoft.ManagedIdentity",
           "type": "ManagedIdentityExtensionForWindows",
           "typeHandlerVersion": "1.0",
           "autoUpgradeMinorVersion": true,
           "settings": {
               "port": 50342
           },
           "protectedSettings": {}
       }
   }
   ```

4. Wenn Sie fertig sind, sollte Ihre Vorlage dem folgenden Beispiel ähneln:

   ![Vorlage nach dem Screenshot](./media/msi-qs-configure-template-windows-vm/template-file-after.png) 

## <a name="remove-msi-from-an-azure-vm"></a>Entfernen von MSI von einem virtuellen Azure-Computer

Wenn MSI auf einem virtuellen Computer nicht mehr benötigt wird, entfernen Sie einfach die beiden Elemente, die im vorherigen Beispiel hinzugefügt wurden: die `"identity"`-Eigenschaft des virtuellen Computers und die `"Microsoft.Compute/virtualMachines/extensions"`-Ressource.

## <a name="related-content"></a>Verwandte Inhalte

- [Übersicht über verwaltete Dienstidentitäten](msi-overview.md)

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.


