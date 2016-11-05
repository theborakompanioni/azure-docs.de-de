---
title: Ressourcen-Manager-Vorlage für Rollenzuweisungen | Microsoft Docs
description: Zeigt das Ressourcen-Manager-Schema für die Bereitstellung einer Rollenzuweisung über eine Vorlage.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: ''

ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/03/2016
ms.author: tomfitz

---
# <a name="role-assignments-template-schema"></a>Vorlagenschema für Rollenzuweisungen
Weist einen Benutzer, eine Gruppe oder einen Dienstprinzipal einer Rolle in einem angegebenen Bereich zu.

## <a name="resource-format"></a>Ressourcenformat
Fügen Sie zum Erstellen einer Rollenzuweisung das folgende Schema im Ressourcenabschnitt der Vorlage hinzu:

    {
        "type": string,
        "apiVersion": "2015-07-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "roleDefinitionId": string,
            "principalId": string,
            "scope": string
        }
    }

## <a name="values"></a>Werte
In den folgenden Tabellen sind die Werte beschrieben, die Sie im Schema festlegen müssen.

| Name | Erforderlich | Beschreibung |
| --- | --- | --- |
| Typ |Ja |Der zu erstellende Ressourcentyp.<br /><br /> Für die Ressourcengruppe:<br />**Microsoft.Authorization/roleAssignments**<br /><br />Für die Ressource:<br />**{provider-namespace}/{resource-type}/providers/roleAssignments** |
| apiVersion |Ja |Die zum Erstellen der Ressource zu verwendende API-Version.<br /><br /> Verwenden Sie **2015-07-01**. |
| Name |Ja |Ein Globally Unique Identifier (GUID) für die neue Rollenzuweisung. |
| dependsOn |Nein |Ein durch Trennzeichen getrenntes Array von Ressourcennamen oder eindeutigen Ressourcenbezeichnern.<br /><br />Die Auflistung der Ressourcen, von denen diese Rollenzuweisung abhängt. Wenn Sie eine Rolle zuweisen, die auf eine Ressource beschränkt ist, und diese Ressource in derselben Vorlage bereitgestellt wird, nehmen Sie den Ressourcennamen in dieses Element auf, um sicherzustellen, dass die Ressource zuerst bereitgestellt wird. |
| Eigenschaften |Ja |Ein Eigenschaftenobjekt, das die Rollendefinition, den Prinzipal und den Bereich bezeichnet. |

### <a name="properties-object"></a>properties-Objekt
| Name | Erforderlich | Beschreibung |
| --- | --- | --- |
| roleDefinitionId |Ja |Der Bezeichner einer vorhandenen Rollendefinition, der für die Rollenzuweisung verwendet werden soll.<br /><br /> Verwenden Sie das folgende Format:<br /> **/subscriptions/{abonnement-id}/providers/Microsoft.Authorization/roleDefinitions/{rollendefinitions-id}** |
| principalId |Ja |Der Globally Unique Identifier für einen vorhandenen Prinzipal. Dieser Wert wird der ID im Verzeichnis zugeordnet und kann auf einen Benutzer, einen Dienstprinzipal oder eine Sicherheitsgruppe zeigen. |
| Bereich |Nein |Der Bereich, für den diese Rollenzuweisung gilt<br /><br />Verwenden Sie Folgendes für Ressourcengruppen:<br />**/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}**  <br /><br />Verwenden Sie Folgendes für Ressourcen:<br />**/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{provider-namespace}/{resource-type}/{resource-name}** |

## <a name="how-to-use-the-role-assignment-resource"></a>Verwenden der Rollenzuweisungsressource
Sie fügen Ihrer Vorlage eine Rollenzuweisung zu, wenn Sie einer Rolle während der Bereitstellung einen Benutzer, eine Gruppe oder einen Dienstprinzipal hinzufügen müssen. Rollenzuweisungen werden von höheren Bereichsebenen geerbt. Wenn Sie also einer Rolle bereits auf Abonnementebene einen Prinzipal hinzugefügt haben, müssen Sie ihn nicht erneut für die Ressourcengruppe oder die Ressource zuordnen.

Beim Arbeiten mit Rollenzuweisungen gibt es viele Bezeichnerwerte, die Sie bereitstellen müssen. Sie können diese Werte über PowerShell oder die Azure-Befehlszeilenschnittstelle abrufen.

### <a name="powershell"></a>PowerShell
Der Name der Rollenzuweisung erfordert einen Globally Unique Identifier. Sie können wie folgt für **name** einen neuen Bezeichner generieren:

    $name = [System.Guid]::NewGuid().toString()

Sie können den Bezeichner für eine Rollendefinition mit folgendem Befehl abrufen:

    $roledef = (Get-AzureRmRoleDefinition -Name Reader).id

Sie können den Bezeichner des Prinzipals mit einem der folgenden Befehle abrufen.

Für eine Gruppe namens **Auditors**:

    $principal = (Get-AzureRmADGroup -SearchString Auditors).id

Für einen Benutzer namens **exampleperson**:

    $principal = (Get-AzureRmADUser -SearchString exampleperson).id

Für einen Dienstprinzipal namens **exampleapp**:

    $principal = (Get-AzureRmADServicePrincipal -SearchString exampleapp).id 

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Sie können den Bezeichner für eine Rollendefinition mit folgendem Befehl abrufen:

    azure role show Reader --json | jq .[].Id -r

Sie können den Bezeichner des Prinzipals mit einem der folgenden Befehle abrufen.

Für eine Gruppe namens **Auditors**:

    azure ad group show --search Auditors --json | jq .[].objectId -r

Für einen Benutzer namens **exampleperson**:

    azure ad user show --search exampleperson --json | jq .[].objectId -r

Für einen Dienstprinzipal namens **exampleapp**:

    azure ad sp show --search exampleapp --json | jq .[].objectId -r

## <a name="examples"></a>Beispiele
Die folgende Vorlage empfängt einen Bezeichner für eine Rolle und einen Bezeichner für einen Benutzer, eine Gruppe oder einen Dienstprinzipal. Sie weist die Rolle auf Ebene der Ressourcengruppe zu.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "roleDefinitionId": {
                "type": "string"
            },
            "roleAssignmentId": {
                "type": "string"
            },
            "principalId": {
                "type": "string"
            }
        },
        "resources": [
            {
              "type": "Microsoft.Authorization/roleAssignments",
              "apiVersion": "2015-07-01",
              "name": "[parameters('roleAssignmentId')]",
              "properties":
              {
                "roleDefinitionId": "[concat(subscription().id, '/providers/Microsoft.Authorization/roleDefinitions/', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]",
                "scope": "[concat(subscription().id, '/resourceGroups/', resourceGroup().name)]"
              }
            }
        ],
        "outputs": {}
    }



Die nächste Vorlage erstellt ein Speicherkonto und weist diesem die Leserolle zu. Die Bezeichner für zwei Gruppen und die Leserolle wurden der Vorlage hinzugefügt, um die Bereitstellung zu vereinfachen. Diese Werte können zum Zeitpunkt der Bereitstellung im Skript abgerufen und als Parameter übergeben werden.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "roleName": {
          "type": "string"
        },
        "groupToAssign": {
          "type": "string",
          "allowedValues": [
            "Auditors",
            "Limited"
          ]
        }
      },
      "variables": {
        "readerRole": "[concat('/subscriptions/',subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
        "storageName": "[concat('storage', uniqueString(resourceGroup().id))]",
        "scope": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Storage/storageAccounts/', variables('storageName'))]",
        "Auditors": "1c272299-9729-462a-8d52-7efe5ece0c5c",
        "Limited": "7c7250f0-7952-441c-99ce-40de5e3e30b5",
        "fullRoleName": "[concat(variables('storageName'), '/Microsoft.Authorization/', parameters('roleName'))]"
      },
      "resources": [
        {
          "apiVersion": "2016-01-01",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[variables('storageName')]",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "tags": {
            "displayName": "MyStorageAccount"
          },
          "properties": {}
        },
        {
          "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
          "apiVersion": "2015-07-01",
          "name": "[variables('fullRoleName')]",
          "dependsOn": [
            "[variables('storageName')]"
          ],
          "properties": {
            "roleDefinitionId": "[variables('readerRole')]",
            "principalId": "[variables(parameters('groupToAssign'))]"
          }
        }
      ]
    }

## <a name="quickstart-templates"></a>Schnellstartvorlagen
Die folgenden Vorlagen zeigen, wie die Rollenzuweisungsressource verwendet wird:

* [Zuweisen einer integrierten Rolle zu einer Ressourcengruppe](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-resourcegroup)
* [Zuweisen einer integrierten Rolle zu einem vorhandenen virtuellen Computer](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-virtualmachine)
* [Zuweisen einer integrierten Rolle zu mehreren vorhandenen virtuellen Computer](https://azure.microsoft.com/documentation/templates/201-rbac-builtinrole-multipleVMs)

## <a name="next-steps"></a>Nächste Schritte
* Informationen zur Vorlagenstruktur finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](resource-group-authoring-templates.md).
* Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Active Directory](active-directory/role-based-access-control-configure.md).

<!--HONumber=Oct16_HO2-->


