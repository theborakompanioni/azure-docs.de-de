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
ms.date: 07/25/2017
ms.author: tomfitz
ms.translationtype: HT
ms.sourcegitcommit: 349fe8129b0f98b3ed43da5114b9d8882989c3b2
ms.openlocfilehash: 1ca72599e67e79d42a3d430dbb13e89ea7265334
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---
# <a name="use-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Verwenden von Key Vault zum Übergeben eines sicheren Parameterwerts während der Bereitstellung

Wenn Sie einen zu schützenden Wert (z.B. ein Kennwort) während der Bereitstellung als Parameter übergeben müssen, können Sie den Wert aus einer Instanz von [Azure Key Vault](../key-vault/key-vault-whatis.md) abrufen. Sie rufen den Wert ab, indem Sie den Schlüsseltresor und das Geheimnis in Ihrer Parameterdatei angeben. Der Wert wird nie offengelegt, da Sie nur auf die Schlüsseltresor-ID verweisen. Sie müssen den Wert für das Geheimnis nicht jedes Mal manuell eingeben, wenn Sie die Ressourcen bereitstellen. Der Schlüsseltresor kann unter einem anderen Abonnement als die Ressourcengruppe für die Bereitstellung vorhanden sein. Beim Verweisen auf den Schlüsseltresor geben Sie die Abonnement-ID an.

Legen Sie beim Erstellen des Schlüsseltresors die Eigenschaft *enabledForTemplateDeployment* auf *true* fest. Durch Festlegen des Werts auf „true“ ermöglichen Sie den Zugriff über Resource Manager-Vorlagen während der Bereitstellung.  

## <a name="deploy-a-key-vault-and-secret"></a>Bereitstellen eines Schlüsseltresors und eines geheimen Schlüssels

Verwenden Sie zum Erstellen eines Schlüsseltresors und eines Geheimnisses die Azure CLI oder PowerShell. Beachten Sie, dass der Schlüsseltresor für die Vorlagenbereitstellung aktiviert ist. 

Verwenden Sie für die Azure-Befehlszeilenschnittstelle den folgenden Befehl:

```azurecli
vaultname={your-unique-vault-name}
password={password-value}

az group create --name examplegroup --location 'South Central US'
az keyvault create --name $vaultname --resource-group examplegroup --location 'South Central US' --enabled-for-template-deployment true
az keyvault secret set --vault-name $vaultname --name examplesecret --value $password
```

Verwenden Sie für PowerShell Folgendes:

```powershell
$vaultname = "{your-unique-vault-name}"
$password = "{password-value}"

New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
New-AzureRmKeyVault -VaultName $vaultname -ResourceGroupName examplegroup -Location "South Central US" -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $vaultname -Name "examplesecret" -SecretValue $secretvalue
```

## <a name="enable-access-to-the-secret"></a>Aktivieren des Zugriffs auf das Geheimnis

Stellen Sie unabhängig davon, ob Sie einen neuen oder einen bereits vorhandenen Schlüsseltresor verwenden, Folgendes sicher: Der Benutzer, der die Vorlage bereitstellt, muss auf das Geheimnis zugreifen können. Der Benutzer, von dem eine Vorlage bereitgestellt wird, in der auf ein Geheimnis verwiesen wird, muss über die Berechtigung `Microsoft.KeyVault/vaults/deploy/action` für den Schlüsseltresor verfügen. Die Rollen [Besitzer](../active-directory/role-based-access-built-in-roles.md#owner) und [Mitwirkender](../active-directory/role-based-access-built-in-roles.md#contributor) gewähren diesen Zugriff. Sie können auch eine [benutzerdefinierte Rolle](../active-directory/role-based-access-control-custom-roles.md) erstellen, die diese Berechtigung gewährt, und den Benutzer zu dieser Rolle hinzufügen. Informationen zum Hinzufügen eines Benutzers zu einer Rolle finden Sie unter [Zuweisen eines Benutzers zu Administratorrollen in Azure Active Directory](../active-directory/active-directory-users-assign-role-azure-portal.md).


## <a name="reference-a-secret-with-static-id"></a>Verweisen auf ein Geheimnis mit einer statischen ID

Die Vorlage, die ein Geheimnis für einen Schlüsseltresor empfängt, unterscheidet sich nicht von anderen Vorlagen. Der Grund hierfür ist, dass **Sie auf den Schlüsseltresor in der Parameterdatei verweisen, nicht in der Vorlage.** Die folgende Vorlage stellt z.B. eine SQL-Datenbank bereit, die ein Administratorkennwort enthält. Der Kennwortparameter ist auf eine sichere Zeichenfolge festgelegt. Aber die Vorlage gibt nicht den Ursprung dieses Werts an.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "administratorLogin": {
            "type": "string"
        },
        "administratorLoginPassword": {
            "type": "securestring"
        },
        "collation": {
            "type": "string"
        },
        "databaseName": {
            "type": "string"
        },
        "edition": {
            "type": "string"
        },
        "requestedServiceObjectiveId": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "maxSizeBytes": {
            "type": "string"
        },
        "serverName": {
            "type": "string"
        },
        "sampleName": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
        {
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "name": "[parameters('serverName')]",
            "properties": {
                "administratorLogin": "[parameters('administratorLogin')]",
                "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
                "version": "12.0"
            },
            "resources": [
                {
                    "apiVersion": "2014-04-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Sql/servers/', parameters('serverName'))]"
                    ],
                    "location": "[parameters('location')]",
                    "name": "[parameters('databaseName')]",
                    "properties": {
                        "collation": "[parameters('collation')]",
                        "edition": "[parameters('edition')]",
                        "maxSizeBytes": "[parameters('maxSizeBytes')]",
                        "requestedServiceObjectiveId": "[parameters('requestedServiceObjectiveId')]",
                        "sampleName": "[parameters('sampleName')]"
                    },
                    "type": "databases"
                },
                {
                    "apiVersion": "2014-04-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Sql/servers/', parameters('serverName'))]"
                    ],
                    "location": "[parameters('location')]",
                    "name": "AllowAllWindowsAzureIps",
                    "properties": {
                        "endIpAddress": "0.0.0.0",
                        "startIpAddress": "0.0.0.0"
                    },
                    "type": "firewallrules"
                }
            ],
            "type": "Microsoft.Sql/servers"
        }
    ]
}
```

Erstellen Sie jetzt eine Parameterdatei für der vorherige Vorlage. Geben Sie in der Parameterdatei einen Parameter an, der dem Namen des Parameters in der Vorlage entspricht. Verweisen Sie für den Parameterwert auf das Geheimnis aus dem Schlüsseltresor. Sie verweisen auf den geheimen Schlüssel, indem Sie den Ressourcenbezeichner des Schlüsseltresors und den Namen des geheimen Schlüssels übergeben. Im folgenden Beispiel muss das Geheimnis für den Schlüsseltresor bereits vorhanden sein, und Sie geben einen statischer Wert für dessen Ressourcen-ID ein.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "administratorLogin": {
            "value": "exampleadmin"
        },
        "administratorLoginPassword": {
            "reference": {
              "keyVault": {
                "id": "/subscriptions/{guid}/resourceGroups/examplegroup/providers/Microsoft.KeyVault/vaults/{vault-name}"
              },
              "secretName": "examplesecret"
            }
        },
        "collation": {
            "value": "SQL_Latin1_General_CP1_CI_AS"
        },
        "databaseName": {
            "value": "exampledb"
        },
        "edition": {
            "value": "Standard"
        },
        "location": {
            "value": "southcentralus"
        },
        "maxSizeBytes": {
            "value": "268435456000"
        },
        "requestedServiceObjectiveId": {
            "value": "455330e1-00cd-488b-b5fa-177c226f28b7"
        },
        "sampleName": {
            "value": ""
        },
        "serverName": {
            "value": "exampleserver"
        }
    }
}
```

## <a name="reference-a-secret-with-dynamic-id"></a>Verweisen auf ein Geheimnis mit dynamischer ID

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
* Vollständige Beispiele für Verweise auf geheime Schlüssel finden Sie unter [Key Vault examples](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples)(in englischer Sprache).


