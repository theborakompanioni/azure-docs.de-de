<properties
   pageTitle="Ressourcen-Manager-Vorlage für einen geheimen Schlüssel in einem Schlüsseltresor | Microsoft Azure"
   description="Zeigt das Ressourcen-Manager-Schema für die Bereitstellung geheimer Schlüssel in einem Schlüsseltresor über eine Vorlage."
   services="azure-resource-manager,key-vault"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# Vorlagenschema für einen geheimen Schlüssel in einem Schlüsseltresor

Erstellt einen geheimen Schlüssel, der in einem Schlüsseltresor gespeichert wird. Dieser Ressourcentyp wird häufig als untergeordnete Ressource des [Schlüsseltresors](resource-manager-template-keyvault.md) bereitgestellt.

## Schemaformat

Fügen Sie zum Erstellen eines geheimen Schlüssels im Schlüsseltresor das folgende Schema in der Vorlage hinzu. Der geheime Schlüssel kann als untergeordnete Ressource eines Schlüsseltresors oder als Ressource der obersten Ebene definiert werden. Sie können ihn als untergeordnete Ressource definieren, wenn der Schlüsseltresor in der gleichen Vorlage bereitgestellt wird. Sie müssen den geheimen Schlüssel als Ressource der obersten Ebene definieren, wenn der Schlüsseltresor nicht in der gleichen Vorlage bereitgestellt wird oder wenn Sie durch Schleifenbildung des Ressourcentyps mehrere geheime Schlüssel erstellen.

    {
        "type": enum,
        "apiVersion": "2015-06-01",
        "name": string,
        "properties": {
            "value": string
        },
        "dependsOn": [ array values ]
    }

## Werte

In den folgenden Tabellen sind die Werte beschrieben, die Sie im Schema festlegen müssen.

| Name | Wert |
| ---- | ---- | 
| type | Enum<br />Erforderlich<br />**secrets** (wenn als untergeordnete Ressource eines Schlüsseltresors bereitgestellt) oder<br /> **Microsoft.KeyVault/vaults/secrets** (wenn als Ressource der obersten Ebene bereitgestellt)<br /><br />Der zu erstellende Ressourcentyp. |
| "apiVersion": | Enum<br />Erforderlich<br />**2015-06-01** oder **2014-12-19-preview**<br /><br />Die API-Version zum Erstellen der Ressource. | 
| name | String<br />Erforderlich<br />Ein einzelnes Wort, wenn als untergeordnete Ressource eines Schlüsseltresors bereitgestellt, oder im Format **{Schlüsseltresorname}/{Name des geheimen Schlüssels}**, wenn als Ressource der obersten Ebene zum Hinzufügen zu einem vorhandenen Schlüsseltresor bereitgestellt.<br /><br />Der Name des zu erstellenden geheimen Schlüssels. |
| Eigenschaften | Object<br />Erforderlich<br />[properties-Objekt](#properties)<br /><br />Ein Objekt, das den Wert des zu erstellenden geheimen Schlüssels angibt. |
| dependsOn | Array<br />Optional<br />Eine durch Kommas getrennte Liste mit Ressourcennamen oder eindeutigen Ressourcenbezeichnern.<br /><br />Die Sammlung von Ressourcen, von denen dieser Link abhängt. Wenn der Schlüsseltresor für den geheimen Schlüssel in der gleichen Vorlage bereitgestellt wird, fügen Sie den Namen des Schlüsseltresors in diesem Element ein, um sicherzustellen, dass der Tresor zuerst bereitgestellt wird. |

<a id="properties" />
### properties-Objekt

| Name | Wert |
| ---- | ---- | 
| value | String<br />Erforderlich<br /><br />Der im Schlüsseltresor zu speichernde Wert des geheimen Schlüssels. Wenn Sie einen Wert für diese Eigenschaft übergeben, verwenden Sie einen Parameter des Typs **securestring**. |

	
## Beispiele

Im ersten Beispiel wird ein geheimer Schlüssel als untergeordnete Ressource eines Schlüsseltresors bereitgestellt.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the vault"
                }
            },
            "tenantId": {
                "type": "string",
                "metadata": {
                   "description": "Tenant ID for the subscription and use assigned access to the vault. Available from the Get-AzureRmSubscription PowerShell cmdlet"
                }
            },
            "objectId": {
                "type": "string",
                "metadata": {
                    "description": "Object ID of the AAD user or service principal that will have access to the vault. Available from the Get-AzureRmADUser or the Get-AzureRmADServicePrincipal cmdlets"
                }
            },
            "keysPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to keys in the vault. Valid values are: all, create, import, update, get, list, delete, backup, restore, encrypt, decrypt, wrapkey, unwrapkey, sign, and verify."
                }
            },
            "secretsPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to secrets in the vault. Valid values are: all, get, set, list, and delete."
                }
            },
            "vaultSku": {
                "type": "string",
                "defaultValue": "Standard",
                "allowedValues": [
                    "Standard",
                    "Premium"
                ],
                "metadata": {
                    "description": "SKU for the vault"
                }
            },
            "enabledForDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for VM or Service Fabric deployment"
                }
            },
            "enabledForTemplateDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for ARM template deployment"
                }
            },
            "enableVaultForVolumeEncryption": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for volume encryption"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[parameters('keyVaultName')]",
            "apiVersion": "2015-06-01",
            "location": "[resourceGroup().location]",
            "tags": {
                "displayName": "KeyVault"
            },
            "properties": {
                "enabledForDeployment": "[parameters('enabledForDeployment')]",
                "enabledForTemplateDeployment": "[parameters('enabledForTemplateDeployment')]",
                "enabledForVolumeEncryption": "[parameters('enableVaultForVolumeEncryption')]",
                "tenantId": "[parameters('tenantId')]",
                "accessPolicies": [
                {
                    "tenantId": "[parameters('tenantId')]",
                    "objectId": "[parameters('objectId')]",
                    "permissions": {
                        "keys": "[parameters('keysPermissions')]",
                        "secrets": "[parameters('secretsPermissions')]"
                    }
                }],
                "sku": {
                    "name": "[parameters('vaultSku')]",
                    "family": "A"
                }
            },
            "resources": [
            {
                "type": "secrets",
                "name": "[parameters('secretName')]",
                "apiVersion": "2015-06-01",
                "properties": {
                    "value": "[parameters('secretValue')]"
                },
                "dependsOn": [
                    "[concat('Microsoft.KeyVault/vaults/', parameters('keyVaultName'))]"
                ]
            }]
        }]
    }

Im zweiten Beispiel wird der geheime Schlüssel als Ressource der obersten Ebene bereitgestellt, die in einem vorhandenen Schlüsseltresor gespeichert wird.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the existing vault"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.KeyVault/vaults/secrets",
                "apiVersion": "2015-06-01",
                "name": "[concat(parameters('keyVaultName'), '/', parameters('secretName'))]",
                "properties": {
                    "value": "[parameters('secretValue')]"
                }
            }
        ],
        "outputs": {}
    }


## Nächste Schritte

- Allgemeine Informationen zu Schlüsseltresoren finden Sie unter [Erste Schritte mit dem Azure-Schlüsseltresor](./key-vault/key-vault-get-started.md).
- Ein Beispiel für das Verweisen auf einen geheimen Schlüssel in einem Schlüsseltresor beim Bereitstellen von Vorlagen finden Sie unter [Übergeben sicherer Werte während der Bereitstellung](resource-manager-keyvault-parameter.md).

<!---HONumber=AcomDC_0629_2016-->