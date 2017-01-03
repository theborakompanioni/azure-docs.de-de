---
title: Sperren von Ressourcen mit dem Resource Manager | Microsoft Docs
description: "Verhindern Sie, dass Benutzer bestimmte Ressourcen aktualisieren oder löschen, indem Sie eine Einschränkung auf alle Benutzer und Rollen anwenden."
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
ms.date: 08/15/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: e841c21a15c47108cbea356172bffe766003a145
ms.openlocfilehash: fd22c8e0a02a3d0b1286d51d6506a82de2296574


---
# <a name="lock-resources-with-azure-resource-manager"></a>Sperren von Ressourcen mit dem Azure-Ressourcen-Manager
Als Administrator möchten Sie möglicherweise ein Abonnement, eine Ressourcengruppe oder eine Ressource sperren, um zu verhindern, dass andere Benutzer in Ihrer Organisation versehentlich wichtige Ressourcen löschen oder ändern. Sie können die Sperrebene auf **CanNotDelete** oder **ReadOnly** festlegen. 

* **CanNotDelete** bedeutet, dass autorisierte Benutzer weiterhin eine Ressource lesen und ändern, aber nicht löschen können. 
* **ReadOnly** bedeutet, dass autorisierte Benutzer eine Ressource lesen, sie aber nicht ändern oder Aktionen für sie ausführen können. Die Berechtigung für die Ressource ist auf die Rolle **Leser** beschränkt. 

Die Anwendung von **ReadOnly** kann zu unerwarteten Ergebnissen führen, da einige Vorgänge, bei denen es sich scheinbar um Lesevorgänge handelt, zusätzliche Aktionen erfordern. Das Festlegen von **ReadOnly** für ein Speicherkonto hindert beispielsweise alle Benutzer am Auflisten der Schlüssel. Der Vorgang zum Auflisten von Schlüsseln wird über eine POST-Anforderung behandelt, da die zurückgegebenen Schlüssel für Schreibvorgänge zur Verfügung stehen. Weiteres Beispiel: Das Festlegen von **ReadOnly** für eine App Service-Ressource verhindert, dass der Server-Explorer von Visual Studio Dateien für die Ressource anzeigen kann, da für diese Interaktion Schreibzugriff erforderlich ist.

Im Gegensatz zur rollenbasierten Zugriffssteuerung verwenden Sie Verwaltungssperren, um eine Einschränkung für alle Benutzer und Rollen zu aktivieren. Informationen zum Festlegen von Benutzer- und Rollenberechtigungen finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-control-configure.md).

Wenn Sie eine Sperre in einem übergeordneten Bereich anwenden, erben alle untergeordneten Ressourcen diese Sperre. Auch Ressourcen, die Sie später hinzufügen, erben die Sperre aus dem übergeordneten Element. Die restriktivste Sperre in der Vererbung hat Vorrang.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Personen in Ihrer Organisation, die Sperren erstellen oder löschen können
Zum Erstellen oder Löschen von Verwaltungssperren müssen Sie Zugriff auf Aktionen vom Typ **Microsoft.Authorization/\*** oder **Microsoft.Authorization/locks/\*** haben. Unter den integrierten Rollen können nur **Besitzer** und **Benutzerzugriffsadministrator** diese Aktionen ausführen.

## <a name="creating-a-lock-through-the-portal"></a>Erstellen einer Sperre über das Portal
[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="creating-a-lock-in-a-template"></a>Erstellen einer Sperre in einer Vorlage
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

## <a name="creating-a-lock-with-rest-api"></a>Erstellen einer Sperre mit der REST-API
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


## <a name="creating-a-lock-with-azure-powershell"></a>Erstellen einer Sperre mit Azure PowerShell
Sie können bereitgestellte Ressourcen mit Azure PowerShell sperren, indem Sie wie im folgenden Beispiel gezeigt das **New-AzureRmResourceLock** -Cmdlet verwenden.

    New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup

Azure PowerShell bietet auch andere Befehle für die Arbeit mit Sperren, z.B. **Set-AzureRmResourceLock**, um eine Sperre zu aktualisieren, und **Remove-AzureRmResourceLock**, um eine Sperre zu löschen.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Arbeiten mit Ressourcensperren finden Sie unter [Sperren Ihrer Azure-Ressourcen](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
* Informationen zum logischen Organisieren von Ressourcen finden Sie unter [Verwenden von Tags zum Organisieren von Ressourcen](resource-group-using-tags.md)
* Informationen dazu, wie Sie die Ressourcengruppe für eine Ressource ändern, finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe](resource-group-move-resources.md)
* Sie können mithilfe benutzerdefinierter Richtlinien Einschränkungen und Konventionen für Ihr Abonnement festlegen. Weitere Informationen finden Sie unter [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](resource-manager-policy.md).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).




<!--HONumber=Nov16_HO3-->


