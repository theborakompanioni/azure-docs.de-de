---
title: "Verwenden eines geheimen Schlüssels aus einem Schlüsseltresor mit Resource Manager-Vorlage | Microsoft Docs"
description: "Informationen zum Übergeben eines geheimen Schlüssels aus einem Schlüsseltresor als Parameter während der Bereitstellung."
services: azure-resource-manager,key-vault
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: c582c144-4760-49d3-b793-a3e1e89128e2
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: e841c21a15c47108cbea356172bffe766003a145
ms.openlocfilehash: fa688748be96aa614b46a218e19a0e771b908baf


---
# <a name="pass-secure-values-during-deployment"></a>Übergeben sicherer Werte während der Bereitstellung

Wenn Sie während der Bereitstellung einen sicheren Wert (z.B. ein Kennwort) als Parameter übergeben müssen, können Sie diesen Wert als Geheimnis in einem [Azure-Schlüsseltresor](../key-vault/key-vault-whatis.md) speichern und in Ihrer Parameterdatei auf das Geheimnis verweisen. Der Wert wird nie offengelegt, da Sie nur auf die Schlüsseltresor-ID verweisen. Sie müssen den Wert für das Geheimnis nicht jedes Mal manuell eingeben, wenn Sie die Ressourcen bereitstellen.

## <a name="deploy-a-key-vault-and-secret"></a>Bereitstellen eines Schlüsseltresors und eines geheimen Schlüssels

Informationen zum Bereitstellen eines Schlüsseltresors und eines Geheimnisses finden Sie unter [Vorlagenschema für einen Schlüsseltresor](resource-manager-template-keyvault.md) und [Vorlagenschema für ein Geheimnis in einem Schlüsseltresor](resource-manager-template-keyvault-secret.md). Legen Sie beim Erstellen der Schlüsseltresors die **enabledForTemplateDeployment**-Eigenschaft auf **true** fest, damit andere Resource Manager-Vorlagen darauf verweisen können. 

## <a name="reference-a-secret-with-static-id"></a>Verweisen auf einen geheimen Schlüssel mit statischer ID

Sie verweisen über eine Parameterdatei, die Werte an die Vorlage übergibt, auf das Geheimnis. Sie verweisen auf den geheimen Schlüssel, indem Sie den Ressourcenbezeichner des Schlüsseltresors und den Namen des geheimen Schlüssels übergeben. Im folgenden Beispiel muss das Geheimnis für den Schlüsseltresor bereits vorhanden sein, und Sie geben einen statischer Wert für dessen Ressourcen-ID ein.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "sqlsvrAdminLoginPassword": {
        "reference": {
          "keyVault": {
            "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
          },
          "secretName": "adminPassword"
        }
      },
      "sqlsvrAdminLogin": {
        "value": "exampleadmin"
      }
    }
}
```

Der Parameter, der den geheimen Schlüssel annimmt, sollte ein Parameter des Typs **securestring**sein. Das folgende Beispiel zeigt die relevanten Abschnitte einer Vorlage auf, die einen SQL-Server bereitstellt, für den ein Administratorkennwort erforderlich ist.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "sqlsvrAdminLogin": {
            "type": "string",
            "minLength": 4
        },
        "sqlsvrAdminLoginPassword": {
            "type": "securestring"
        },
        ...
    },
    "resources": [
        {
          "name": "[variables('sqlsvrName')]",
          "type": "Microsoft.Sql/servers",
          "location": "[resourceGroup().location]",
          "apiVersion": "2014-04-01-preview",
          "properties": {
              "administratorLogin": "[parameters('sqlsvrAdminLogin')]",
              "administratorLoginPassword": "[parameters('sqlsvrAdminLoginPassword')]"
          },
          ...
        }
    ],
    "variables": {
        "sqlsvrName": "[concat('sqlsvr', uniqueString(resourceGroup().id))]"
    },
    "outputs": { }
}
```

Der Benutzer, der eine Vorlage bereitstellt, die auf ein Geheimnis verweist, muss über die Berechtigung **Microsoft.KeyVault/vaults/deploy/action** für den Schlüsseltresor verfügen. Die Rollen [Besitzer](../active-directory/role-based-access-built-in-roles.md#owner) und [Mitwirkender](../active-directory/role-based-access-built-in-roles.md#contributor) gewähren diesen Zugriff. Sie können auch eine [benutzerdefinierte Rolle](../active-directory/role-based-access-control-custom-roles.md) erstellen, die diese Berechtigung gewährt, und den Benutzer zu dieser Rolle hinzufügen.

## <a name="reference-a-secret-with-dynamic-id"></a>Verweisen auf einen geheimen Schlüssel mit dynamischer ID

Im vorherigen Abschnitt wurde für das Geheimnis des Schlüsseltresors eine statische Ressourcen-ID übergeben. Manchmal muss jedoch auf einen geheimen Schlüsseltresorschlüssel verwiesen werden, der je nach aktueller Bereitstellung variiert. In einem solchen Fall kann die Ressourcen-ID nicht in der Parameterdatei hartcodiert werden. Da in der Parameterdatei keine Vorlagenausdrücke zulässig sind, kann die Ressourcen-ID leider nicht dynamisch in der Parameterdatei generiert werden.

Wenn die Ressourcen-ID für das Geheimnis eines Schlüsseltresors dynamisch generiert werden muss, müssen Sie die Ressource, die das Geheimnis benötigt, in eine geschachtelte Vorlage verschieben. Die geschachtelte Vorlage wird der Mastervorlage hinzugefügt, und es wird ein Parameter mit der dynamisch generierten Ressourcen-ID übergeben.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "vaultName": {
        "type": "string"
      },
      "secretName": {
        "type": "string"
      }
    },
    "resources": [
    {
      "apiVersion": "2015-01-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
              },
              "secretName": "[parameters('secretName')]"
            }
          }
        }
      }
    }],
    "outputs": {}
}
```

## <a name="next-steps"></a>Nächste Schritte
* Allgemeine Informationen zu Schlüsseltresoren finden Sie unter [Erste Schritte mit dem Azure-Schlüsseltresor](../key-vault/key-vault-get-started.md).
* Informationen zur Verwendung eines Schlüsseltresors mit einem virtuellen Computer finden Sie unter [Sicherheitsaspekte für Azure Resource Manager](best-practices-resource-manager-security.md).
* Vollständige Beispiele für Verweise auf geheime Schlüssel finden Sie unter [Key Vault examples](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples)(in englischer Sprache).




<!--HONumber=Nov16_HO3-->


