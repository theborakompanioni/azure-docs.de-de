---
title: "Sperren von Azure-Ressourcen zum Verhindern von Änderungen | Microsoft-Dokumentation"
description: "Verhindern Sie, dass Benutzer kritische Azure-Ressourcen aktualisieren oder löschen, indem Sie eine Sperre für alle Benutzer und Rollen anwenden."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 53c57e8f-741c-4026-80e0-f4c02638c98b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/05/2017
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 710d20d82b72938de6f6b54c2506276f408664d4
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="lock-resources-to-prevent-unexpected-changes"></a>Sperren von Ressourcen, um unerwartete Änderungen zu verhindern 
Als Administrator möchten Sie möglicherweise ein Abonnement, eine Ressourcengruppe oder eine Ressource sperren, um zu verhindern, dass andere Benutzer in Ihrer Organisation versehentlich wichtige Ressourcen löschen oder ändern. Sie können die Sperrebene auf **CanNotDelete** oder **ReadOnly** festlegen. 

* **CanNotDelete** bedeutet, dass autorisierte Benutzer weiterhin eine Ressource lesen und ändern, aber nicht löschen können. 
* **ReadOnly** bedeutet, dass autorisierte Benutzer eine Ressource zwar lesen, aber nicht löschen oder aktualisieren können. Mit dieser Sperre erzielen Sie einen ähnlichen Effekt wie durch die Beschränkung sämtlicher autorisierter Benutzer auf die Berechtigungen der **Leserolle**. 

## <a name="how-locks-are-applied"></a>Anwenden von Sperren

Wenn Sie eine Sperre in einem übergeordneten Bereich anwenden, erben alle Ressourcen in diesem Bereich die entsprechende Sperre. Auch Ressourcen, die Sie später hinzufügen, erben die Sperre aus dem übergeordneten Element. Die restriktivste Sperre in der Vererbung hat Vorrang.

Im Gegensatz zur rollenbasierten Zugriffssteuerung verwenden Sie Verwaltungssperren, um eine Einschränkung für alle Benutzer und Rollen zu aktivieren. Informationen zum Festlegen von Benutzer- und Rollenberechtigungen finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-control-configure.md).

Resource Manager-Sperren gelten nur für Vorgänge auf der Verwaltungsebene (also für Vorgänge, die an `https://management.azure.com` gesendet werden). Die Ausführung ressourceneigener Funktionen wird durch die Sperren nicht begrenzt. Die Ressourcenänderungen sind eingeschränkt, die Ressourcenvorgänge jedoch nicht. So verhindert beispielsweise eine ReadOnly-Sperre für eine SQL-Datenbank-Instanz zwar, dass die Datenbank gelöscht oder angepasst wird, nicht aber das Erstellen, Aktualisieren oder Löschen von Daten in der Datenbank. Datentransaktionen sind zulässig, da diese Vorgänge nicht an `https://management.azure.com` gesendet werden.

Die Anwendung von **ReadOnly** kann zu unerwarteten Ergebnissen führen, da einige Vorgänge, bei denen es sich scheinbar um Lesevorgänge handelt, zusätzliche Aktionen erfordern. Das Festlegen von **ReadOnly** für ein Speicherkonto hindert beispielsweise alle Benutzer am Auflisten der Schlüssel. Der Vorgang zum Auflisten von Schlüsseln wird über eine POST-Anforderung behandelt, da die zurückgegebenen Schlüssel für Schreibvorgänge zur Verfügung stehen. Weiteres Beispiel: Das Festlegen von **ReadOnly** für eine App Service-Ressource verhindert, dass der Server-Explorer von Visual Studio Dateien für die Ressource anzeigen kann, da für diese Interaktion Schreibzugriff erforderlich ist.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Personen in Ihrer Organisation, die Sperren erstellen oder löschen können
Zum Erstellen oder Löschen von Verwaltungssperren benötigen Sie Zugriff auf `Microsoft.Authorization/*`- oder `Microsoft.Authorization/locks/*`-Aktionen. Unter den integrierten Rollen können nur **Besitzer** und **Benutzerzugriffsadministrator** diese Aktionen ausführen.

## <a name="portal"></a>Portal
[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="template"></a>Vorlage
Das folgende Beispiel zeigt eine Vorlage, mit der eine Sperre für ein Speicherkonto erstellt wird. Das Speicherkonto, für das die Sperre gelten soll, wird als Parameter bereitgestellt. Der Name der Sperre wird erstellt, indem der Ressourcenname mit **/Microsoft.Authorization/** und dem Namen der Sperre, in diesem Fall **myLock**, verkettet wird.

Der bereitgestellte Typ ist ressourcentypspezifisch. Legen Sie für Speicher den Typ „Microsoft.Storage/storageaccounts/providers/locks“ fest.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## <a name="powershell"></a>PowerShell
Sie sperren bereitgestellte Ressourcen mit Azure PowerShell, indem Sie den Befehl [New-AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) verwenden.

Geben Sie zum Sperren einer Ressource den Namen der Ressource, ihren Ressourcentyp und ihren Ressourcengruppennamen an.

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite `
  -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

Geben Sie zum Sperren einer Ressourcengruppe ihren Namen an.

```powershell
New-AzureRmResourceLock -LockName LockGroup -LockLevel CanNotDelete `
  -ResourceGroupName exampleresourcegroup
```

Rufen Sie Informationen zu einer Sperre mithilfe von [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock) ab. Rufen Sie alle Sperren im Abonnement mit dem folgenden Befehl ab:

```powershell
Get-AzureRmResourceLock
```

Rufen Sie alle Sperren für eine Ressource mit dem folgenden Befehl ab:

```powershell
New-AzureRmResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

Rufen Sie alle Sperren für eine Ressourcengruppe mit dem folgenden Befehl ab:

```powershell
Get-AzureRmResourceLock -ResourceGroupName exampleresourcegroup
```

Azure PowerShell stellt weitere Befehle für die Arbeit mit Sperren bereit, z.B. [Set-AzureRmResourceLock](/powershell/module/azurerm.resources/set-azurermresourcelock) zum Aktualisieren einer Sperre und [Remove-AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) zum Löschen einer Sperre.

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Sperren Sie bereitgestellte Ressourcen mit der Azure CLI, indem Sie den Befehl [az lock create](/cli/azure/lock#create) verwenden.

Geben Sie zum Sperren einer Ressource den Namen der Ressource, ihren Ressourcentyp und ihren Ressourcengruppennamen an.

```azurecli
az lock create --name LockSite --lock-type CanNotDelete \
  --resource-group exampleresourcegroup --resource-name examplesite \
  --resource-type Microsoft.Web/sites
```

Geben Sie zum Sperren einer Ressourcengruppe ihren Namen an.

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete \
  --resource-group exampleresourcegroup
```

Verwenden Sie zum Abrufen von Informationen zu einer Sperre [az lock list](/cli/azure/lock#list). Rufen Sie alle Sperren im Abonnement mit dem folgenden Befehl ab:

```azurecli
az lock list
```

Rufen Sie alle Sperren für eine Ressource mit dem folgenden Befehl ab:

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite \
  --namespace Microsoft.Web --resource-type sites --parent ""
```

Rufen Sie alle Sperren für eine Ressourcengruppe mit dem folgenden Befehl ab:

```azurecli
az lock list --resource-group exampleresourcegroup
```

Die Azure CLI stellt weitere Befehle für die Arbeit mit Sperren bereit, z.B. [az lock update](/cli/azure/lock#update) zum Aktualisieren einer Sperre und [az lock delete](/cli/azure/lock#delete) zum Löschen einer Sperre.

## <a name="rest-api"></a>REST-API
Sie können bereitgestellte Ressourcen mit der [REST-API für Verwaltungssperren](https://docs.microsoft.com/rest/api/resources/managementlocks) sperren. Die REST-API ermöglicht es Ihnen, Sperren zu erstellen und zu löschen sowie Informationen zu vorhandenen Sperren abzurufen.

Führen Sie zum Erstellen einer Sperre Folgendes durch:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

Bei dem Bereich kann es sich um ein Abonnement, die Ressourcengruppe oder die Ressource handeln. Geben Sie für "lock-name" den jeweiligen Namen der Sperre ein. Verwenden Sie als „api-version“ die Einstellung **2015-01-01**.

Schließen Sie in die Anforderung ein JSON-Objekt ein, das die Eigenschaften für die Sperre angibt.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Arbeiten mit Ressourcensperren finden Sie unter [Sperren Ihrer Azure-Ressourcen](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
* Informationen zum logischen Organisieren von Ressourcen finden Sie unter [Verwenden von Tags zum Organisieren von Ressourcen](resource-group-using-tags.md)
* Informationen dazu, wie Sie die Ressourcengruppe für eine Ressource ändern, finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe](resource-group-move-resources.md)
* Sie können mithilfe benutzerdefinierter Richtlinien Einschränkungen und Konventionen für Ihr Abonnement festlegen. Weitere Informationen finden Sie unter [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](resource-manager-policy.md).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).


