<properties
   pageTitle="Ressourcen-Manager-Vorlage für Rollenzuweisungen | Microsoft Azure"
   description="Zeigt das Ressourcen-Manager-Schema für die Bereitstellung einer Rollenzuweisung über eine Vorlage."
   services="azure-resource-manager"
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
   ms.date="04/05/2016"
   ms.author="tomfitz"/>

# Vorlagenschema für Rollenzuweisungen

Weist einen Benutzer, eine Gruppe oder einen Dienstprinzipal einer Rolle in einem angegebenen Bereich zu.

## Schemaformat

Fügen Sie zum Erstellen einer Rollenzuweisung das folgende Schema im Ressourcenabschnitt der Vorlage hinzu:
    
    {
        "type": "Microsoft.Authorization/roleAssignments",
        "apiVersion": "2014-10-01-preview",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "roleDefinitionId": string,
            "principalId": string,
            "scope": string
        }
    }

## Werte

In den folgenden Tabellen sind die Werte beschrieben, die Sie im Schema festlegen müssen.

| Name | Wert |
| ---- | ---- |
| type | Enum<br />Erforderlich<br />**Microsoft.Authorization/roleAssignments**<br /><br />Der zu erstellende Ressourcentyp. |
| apiVersion | Enum<br />Erforderlich<br />**2014-10-01-preview**<br /><br />Die zum Erstellen der Ressource zu verwendende API-Version. |  
| name | String<br />Erforderlich<br />**Globally Unique Identifier**<br /><br />Ein Bezeichner für die neue Rollenzuweisung. |
| dependsOn | Array<br />Optional<br />Eine durch Kommas getrennte Liste von Ressourcennamen oder eindeutigen Ressourcenbezeichnern.<br /><br />Die Sammlung von Ressourcen, von denen diese Rollenzuweisung abhängt. Wenn Sie eine Rolle zuweisen, die auf eine Ressource beschränkt ist, und diese Ressource in derselben Vorlage bereitgestellt wird, nehmen Sie den Ressourcennamen in dieses Element auf, um sicherzustellen, dass die Ressource zuerst bereitgestellt wird. | 
| Eigenschaften | Object<br />Erforderlich<br />[properties-Objekt](#properties)<br /><br />Ein Objekt, das die Rollendefinition, den Prinzipal und den Bereich identifiziert. |  

<a id="properties" />
### properties-Objekt

| Name | Wert |
| ------- | ---- |
| roleDefinitionId | String<br />Erforderlich<br /> **/subscriptions/{Abonnement-ID}/providers/Microsoft.Authorization/roleDefinitions/{Rollendefinitions-ID}**<br /><br />Der in der Rollenzuweisung zu verwendende Bezeichner einer vorhandenen Rollendefinition. |
| principalId | String<br />Erforderlich<br />**Globally Unique Identifier**<br /><br />Der Bezeichner eines vorhandenen Prinzipals. Dieser Bezeichner wird der ID im Verzeichnis zugeordnet. Er kann auf einen Benutzer, Dienstprinzipal oder eine Sicherheitsgruppe verweisen. |
| Bereich | String<br />Erforderlich<br />**/subscriptions/{Abonnement-ID}/resourceGroups/{Ressourcengruppenname}** (für Ressourcengruppen) oder<br />**/subscriptions/{Abonnement-ID}/resourceGroups/{Ressourcengruppenname}/providers/{Anbieter-Namespace}/{Ressourcentyp}/{Ressourcenname}** (für Ressourcen)<br /><br />Der Bereich, für den diese Rollenzuweisung gilt. |


## Verwenden der Rollenzuweisungsressource

Sie fügen Ihrer Vorlage eine Rollenzuweisung zu, wenn Sie einer Rolle während der Bereitstellung einen Benutzer, eine Gruppe oder einen Dienstprinzipal hinzufügen müssen. Rollenzuweisungen werden von höheren Bereichsebenen geerbt. Wenn Sie also einer Rolle bereits auf Abonnementebene einen Prinzipal hinzugefügt haben, müssen Sie ihn nicht erneut für die Ressourcengruppe oder die Ressource zuordnen.

Sie können wie folgt für **name** einen neuen Bezeichner generieren:

    PS C:\> $name = [System.Guid]::NewGuid().toString()

Sie können die GUID für eine Rollendefinition mit folgendem Befehl abrufen:

    PS C:\> $roledef = (Get-AzureRmRoleDefinition -Name Reader).id

Sie können den Bezeichner des Prinzipals mit einem der folgenden Befehle abrufen.

Für eine Gruppe namens **Auditors**:

    PS C:\> $principal = (Get-AzureRmADGroup -SearchString Auditors).id

Für einen Benutzer namens **exampleperson**:

    PS C:\> $principal = (Get-AzureRmADUser -SearchString exampleperson).id

Für einen Dienstprinzipal namens **exampleapp**:

    PS C:\> $principal = (Get-AzureRmADServicePrincipal -SearchString exampleapp).id 
 

## Beispiele

Das folgende Beispiel weist eine Gruppe zu einer Rolle für die Ressourcengruppe zu.

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

## Schnellstartvorlagen

Die folgenden Vorlagen zeigen, wie die Rollenzuweisungsressource verwendet wird:

- [Zuweisen einer integrierten Rolle zu einer Ressourcengruppe](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-resourcegroup)
- [Zuweisen einer integrierten Rolle zu einem vorhandenen virtuellen Computer](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-virtualmachine)
- [Zuweisen einer integrierten Rolle zu mehreren vorhandenen virtuellen Computer](https://azure.microsoft.com/documentation/templates/201-rbac-builtinrole-multipleVMs)

## Nächste Schritte

- Informationen zur Vorlagenstruktur finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](resource-group-authoring-templates.md).
- Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Active Directory](active-directory/role-based-access-control-configure.md).

<!---HONumber=AcomDC_0406_2016-->