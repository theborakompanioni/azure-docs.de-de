---
title: "Resource Manager-Vorlage für Schlüsseltresore | Microsoft Docs"
description: "Zeigt das Ressourcen-Manager-Schema für die Bereitstellung von Schlüsseltresoren über eine Vorlage."
services: azure-resource-manager,key-vault
documentationcenter: na
author: tfitzmac
manager: wpickett
editor: 
ms.assetid: 96433b1a-68ee-4292-85b6-a581618b921b
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/23/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: f2d009477a614c3b2876ce98a355d3775abf772b
ms.openlocfilehash: 04f2d5d8e501ebf41cf95ea925d238f64b096c1d
ms.lasthandoff: 02/27/2017


---
# <a name="key-vault-template-schema"></a>Vorlagenschema für einen Schlüsseltresor
Erstellt einen Schlüsseltresor.

## <a name="schema-format"></a>Schemaformat
Fügen Sie zum Erstellen eines Schlüsseltresors das folgende Schema im Ressourcenabschnitt der Vorlage hinzu.

    {
        "type": "Microsoft.KeyVault/vaults",
        "apiVersion": "2015-06-01",
        "name": string,
        "location": string,
        "properties": {
            "enabledForDeployment": bool,
            "enabledForTemplateDeployment": bool,
            "enabledForDiskEncryption": bool,
            "tenantId": string,
            "accessPolicies": [
                {
                    "tenantId": string,
                    "objectId": string,
                    "permissions": {
                        "keys": [ keys permissions ],
                        "secrets": [ secrets permissions ]
                    }
                }
            ],
            "sku": {
                "name": enum,
                "family": "A"
            }
        },
        "resources": [
             child resources
        ]
    }

## <a name="values"></a>Werte
In den folgenden Tabellen sind die Werte beschrieben, die Sie im Schema festlegen müssen.

| Name | Wert |
| --- | --- |
| type |Enum<br />Erforderlich<br />**Microsoft.KeyVault/vaults**<br /><br />Der zu erstellende Ressourcentyp. |
| apiVersion |Enum<br />Erforderlich<br />**2015-06-01** oder **2014-12-19-preview**<br /><br />Die zum Erstellen der Ressource zu verwendende API-Version. |
| Name |String<br />Erforderlich<br />Ein Name, der innerhalb von Azure eindeutig ist.<br /><br />Der Name des zu erstellenden Schlüsseltresors. Verwenden Sie zum Erstellen eines eindeutigen Namens die Funktion [uniqueString](resource-group-template-functions.md#uniquestring) mit Ihrer Benennungskonvention (wie im Beispiel weiter unten zu sehen). |
| location |String<br />Erforderlich<br />Eine gültige Region für Schlüsseltresore. Informationen zum Bestimmen gültiger Bereiche finden Sie unter [Unterstützte Regionen](resource-manager-supported-services.md#supported-regions).<br /><br />Die Region, in der der Schlüsseltresor gehostet werden soll. |
| properties |Objekt<br />Erforderlich<br />[properties-Objekt](#properties)<br /><br />Ein Objekt, das die Art des zu erstellenden Schlüsseltresors angibt. |
| resources |Array<br />Optional<br />Zulässige Werte: [Geheimnisressourcen für den Schlüsseltresor](resource-manager-template-keyvault-secret.md)<br /><br />Untergeordnete Ressourcen für den Schlüsseltresor. |

<a id="properties" />

### <a name="properties-object"></a>properties-Objekt
| Name | Wert |
| --- | --- |
| enabledForDeployment |Boolean<br />Optional<br />**true** oder **false**<br /><br />Gibt an, ob der Tresor für die Bereitstellung virtueller Computer oder die Service Fabric-Bereitstellung aktiviert ist. |
| enabledForTemplateDeployment |Boolean<br />Optional<br />**true** oder **false**<br /><br />Gibt an, ob der Tresor zur Verwendung in Resource Manager-Vorlagenbereitstellungen aktiviert ist. Weitere Informationen finden Sie unter [Übergeben sicherer Werte während der Bereitstellung](resource-manager-keyvault-parameter.md) |
| enabledForDiskEncryption |Boolean<br />Optional<br />**true** oder **false**<br /><br />Gibt an, ob der Tresor für die Volumeverschlüsselung aktiviert ist. |
| tenantId |String<br />Erforderlich<br />**Globally Unique Identifier (GUID)**<br /><br />Die Mandanten-ID für das Abonnement. Diese ID können Sie mit dem PowerShell-Cmdlet [Get-AzureRmSubscription](https://msdn.microsoft.com/library/azure/mt619284.aspx) oder mit dem Azure-CLI-Befehl **azure account show** abrufen. |
| accessPolicies |Array<br />Erforderlich<br />[accessPolicies-Objekt](#accesspolicies)<br /><br />Ein Array von bis zu 16 Objekten, die die Berechtigungen für den Benutzer oder Dienstprinzipal angeben. |
| sku |Objekt<br />Erforderlich<br />[sku-Objekt](#sku)<br /><br />Die SKU für den Schlüsseltresor. |

<a id="accesspolicies" />

### <a name="propertiesaccesspolicies-object"></a>properties.accessPolicies-Objekt
| Name | Wert |
| --- | --- |
| tenantId |String<br />Erforderlich<br />**Globally Unique Identifier (GUID)**<br /><br />Die Mandanten-ID des Azure Active Directory-Mandanten mit der **objectId** in dieser Zugriffsrichtlinie. |
| objectId |String<br />Erforderlich<br />**Globally Unique Identifier (GUID)**<br /><br />Die Objekt-ID des Azure Active Directory-Benutzers oder -Dienstprinzipals, der Zugriff auf den Tresor hat. Sie können den Wert entweder mithilfe der PowerShell-Cmdlets [Get-AzureRmADUser](https://msdn.microsoft.com/library/azure/mt679001.aspx) oder [Get-AzureRmADServicePrincipal](https://msdn.microsoft.com/library/azure/mt678992.aspx) oder mithilfe des Azure CLI-Befehls **azure ad user** oder **azure ad sp** abrufen. |
| Berechtigungen |Objekt<br />Erforderlich<br />[permissions-Objekt](#permissions)<br /><br />Die Berechtigungen, die in diesem Tresor für das Active Directory-Objekt gewährt werden. |

<a id="permissions" />

### <a name="propertiesaccesspoliciespermissions-object"></a>properties.accessPolicies.permissions-Objekt
| Name | Wert |
| --- | --- |
| keys |Array<br />Erforderlich<br />**all**, **backup**, **create**, **decrypt**, **delete**, **encrypt**, **get**, **import**, **list**, **restore**, **sign**, **unwrapkey**, **update**, **verify**, **wrapkey**<br /><br />Die Berechtigungen, die für Schlüssel in diesem Tresor für das Active Directory-Objekt gewährt werden. Dieser Wert muss als Array von einem oder mehreren zulässigen Werten angegeben werden. |
| secrets |Array<br />Erforderlich<br />**all**, **delete**, **get**, **list**, **set**<br /><br />Die Berechtigungen, die für geheime Schlüssel in diesem Tresor für das Active Directory-Objekt gewährt werden. Dieser Wert muss als Array von einem oder mehreren zulässigen Werten angegeben werden. |

<a id="sku" />

### <a name="propertiessku-object"></a>properties.sku-Objekt
| Name | Wert |
| --- | --- |
| Name |Enum<br />Erforderlich<br />**Standard** oder **Premium** <br /><br />Die Dienstebene des zu verwendenden Schlüsseltresors.  In der Dienstebene „Standard“ werden geheime Schlüssel und geschützte Schlüssel unterstützt.  In der Dienstebene „Premium“ werden zudem HSM-geschützte Schlüssel unterstützt. |
| family |Enum<br />Erforderlich<br />**A** <br /><br />Die zu verwendende SKU-Familie. |

## <a name="examples"></a>Beispiele
Im folgenden Beispiel werden ein Schlüsseltresor und ein geheimer Schlüssel bereitgestellt.

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
            "enableVaultForDiskEncryption": {
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
                "enabledForDiskEncryption": "[parameters('enableVaultForDiskEncryption')]",
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

## <a name="quickstart-templates"></a>Schnellstartvorlagen
Die folgende Schnellstartvorlage stellt einen Schlüsseltresor bereit.

* [Erstellen eines Schlüsseltresors](https://azure.microsoft.com/documentation/templates/101-key-vault-create/)

## <a name="next-steps"></a>Nächste Schritte
* Allgemeine Informationen zu Schlüsseltresoren finden Sie unter [Erste Schritte mit dem Azure-Schlüsseltresor](../key-vault/key-vault-get-started.md).
* Ein Beispiel für das Verweisen auf einen geheimen Schlüssel in einem Schlüsseltresor beim Bereitstellen von Vorlagen finden Sie unter [Übergeben sicherer Werte während der Bereitstellung](resource-manager-keyvault-parameter.md).


