<properties
   pageTitle="Verwenden eines geheimen Schlüssels aus einem Schlüsseltresor mit Resource Manager-Vorlage | Microsoft Azure"
   description="Informationen zum Übergeben eines geheimen Schlüssels aus einem Schlüsseltresor als Parameter während der Bereitstellung."
   services="azure-resource-manager,key-vault"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# Übergeben sicherer Werte während der Bereitstellung

Wenn Sie während der Bereitstellung einen sicheren Wert (z. B. ein Kennwort) als Parameter übergeben müssen, können Sie diesen Wert als geheimen Schlüssel in einem [Azure-Schlüsseltresor](./key-vault/key-vault-whatis.md) speichern und in anderen Ressourcen-Manager-Vorlagen auf ihn verweisen. In Ihrer Vorlage fügen Sie lediglich einen Verweis auf den geheimen Schlüssel ein, sodass dieser niemals offengelegt wird. Zudem müssen Sie den Wert für den geheimen Schlüssel nicht bei jedem Bereitstellen der Ressourcen manuell eingeben. Sie geben an, welche Benutzer oder Dienstprinzipale auf den geheimen Schlüssel zugreifen können.

## Bereitstellen eines Schlüsseltresors und eines geheimen Schlüssels

Zum Erstellen eines Schlüsseltresors, auf den in anderen Ressourcen-Manager-Vorlagen verwiesen werden kann, müssen Sie die **enabledForTemplateDeployment**-Eigenschaft auf **true** festlegen und Zugriff für den Benutzer oder Dienstprinzipal erteilen, der die Bereitstellung ausführt, in der auf den geheimen Schlüssel verwiesen wird.

Informationen zum Bereitstellen eines Schlüsseltresors und eines geheimen Schlüssels finden Sie unter [Vorlagenschema für einen Schlüsseltresor](resource-manager-template-keyvault.md) und [Vorlagenschema für einen geheimen Schlüssel in einem Schlüsseltresor](resource-manager-template-keyvault-secret.md).

## Verweisen auf einen geheimen Schlüssel mit statischer ID

Sie verweisen auf den geheimen Schlüssel über eine Parameterdatei, die Werte an die Vorlage übergibt. Sie verweisen auf den geheimen Schlüssel, indem Sie den Ressourcenbezeichner des Schlüsseltresors und den Namen des geheimen Schlüssels übergeben. In diesem Beispiel muss der geheime Schlüssel für den Schlüsseltresor bereits vorhanden sein und ein statischer Wert für dessen Ressourcen-ID verwendet werden.

    "parameters": {
      "adminPassword": {
        "reference": {
          "keyVault": {
            "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
          }, 
          "secretName": "sqlAdminPassword" 
        } 
      }
    }

Eine vollständige Parameterdatei kann beispielsweise wie folgt aussehen:

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "sqlsvrAdminLogin": {
          "value": ""
        },
        "sqlsvrAdminLoginPassword": {
          "reference": {
            "keyVault": {
              "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
            },
            "secretName": "adminPassword"
          }
        }
      }
    }

Der Parameter, der den geheimen Schlüssel annimmt, sollte ein Parameter des Typs **securestring** sein. Das folgende Beispiel zeigt die relevanten Abschnitte einer Vorlage auf, die einen SQL-Server bereitstellt, für den ein Administratorkennwort erforderlich ist.

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
        }],
        "variables": {
            "sqlsvrName": "[concat('sqlsvr', uniqueString(resourceGroup().id))]"
        },
        "outputs": { }
    }

## Verweisen auf einen geheimen Schlüssel mit dynamischer ID

Im vorherigen Abschnitt wurde für den geheimen Schlüssel des Schlüsseltresors eine statische Ressourcen-ID übergeben. Manchmal muss jedoch auf einen geheimen Schlüsseltresorschlüssel verwiesen werden, der je nach aktueller Bereitstellung variiert. In einem solchen Fall kann die Ressourcen-ID nicht in der Parameterdatei hartcodiert werden. Da in der Parameterdatei keine Vorlagenausdrücke zulässig sind, kann die Ressourcen-ID leider nicht dynamisch in der Parameterdatei generiert werden.

Wenn die Ressourcen-ID für einen geheimen Schlüsseltresorschlüssel dynamisch generiert werden muss, müssen Sie die Ressource, die den geheimen Schlüssel benötigt, in eine geschachtelte Vorlage verschieben. Die geschachtelte Vorlage wird der Mastervorlage hinzugefügt, und es wird ein Parameter mit der dynamisch generierten Ressourcen-ID übergeben.

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
        }
      ],
      "outputs": {}
    }


## Nächste Schritte

- Allgemeine Informationen zu Schlüsseltresoren finden Sie unter [Erste Schritte mit dem Azure-Schlüsseltresor](./key-vault/key-vault-get-started.md).
- Informationen zur Verwendung eines Schlüsseltresors mit einem virtuellen Computer finden Sie unter [Sicherheitsaspekte für Azure Resource Manager](best-practices-resource-manager-security.md).
- Vollständige Beispiele für Verweise auf geheime Schlüssel finden Sie unter [Key Vault examples](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples) (in englischer Sprache).

<!---HONumber=AcomDC_0629_2016-->