<properties
   pageTitle="Ressourcen-Manager-Vorlage für Ressourcenverknüpfungen | Microsoft Azure"
   description="Zeigt das Ressourcen-Manager-Schema zum Bereitstellen von Links zwischen verwandten Ressourcen über eine Vorlage."
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

# Vorlagenschema für Ressourcenlinks

Erstellt eine Verknüpfung zwischen zwei Ressourcen. Die Verknüpfung gehört zu einer Quellressource genannten Ressource. Die zweite Ressource in der Verknüpfung wird als Zielressource bezeichnet.

## Schemaformat

Fügen Sie zum Erstellen einer Verknüpfung das folgende Schema im Ressourcenabschnitt der Vorlage hinzu.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "targetId": string,
            "notes": string
        }
    }



## Werte

In den folgenden Tabellen sind die Werte beschrieben, die Sie im Schema festlegen müssen.

| Name | Wert |
| ---- | ---- |
| type | Enum<br />Erforderlich<br />**{Namespace}/{Typ}/providers/links**<br /><br />Der zu erstellende Ressourcentyp. Die Werte {Namespace} und {Typ} Werte beziehen sich auf den Anbieternamespace und Ressourcentyp der Quellressource. |
| apiVersion | Enum<br />Erforderlich<br />**2015-01-01**<br /><br />Die zum Erstellen der Ressource zu verwendende API-Version. |  
| name | String<br />Erforderlich<br />**{Ressource}/Microsoft.Resources/{Linkname}**<br /> bis zu 64 Zeichen; darf weder <, > %, &, ? noch irgendwelche Steuerzeichen enthalten.<br /><br />Ein Wert, der sowohl den Namen der Quellressource als auch den Namen für den Link angibt. | 
| dependsOn | Array<br />Optional<br />Eine durch Kommas getrennte Liste von Ressourcennamen oder eindeutigen Ressourcenbezeichnern.<br /><br />Die Ressourcensammlung, von der dieser Link abhängt. Wenn die Ressourcen, die Sie verknüpfen in derselben Vorlage bereitgestellt werden, fügen Sie die Ressourcennamen in diesem Element hinzu, um sicherzustellen, dass sie zuerst bereitgestellt werden. | 
| properties | Object<br />Erforderlich<br />[properties-Objekt](#properties)<br /><br />Ein Objekt, das die Ressource, zu der verknüpft werden soll sowie Informationen zum Link identifiziert. | 

<a id="properties" />
### properties-Objekt

| Name | Wert |
| ------- | ---- |
| targetId | String<br />Erforderlich<br />**{Ressourcen-ID}**<br /><br />Der Bezeichner der Zielressource, zu der verknüpft werden soll. | | notes | String<br />Optional<br />bis zu 512 Zeichen<br /><br />Beschreibung der Sperre. |


## Verwenden der Verknüpfungsressource

Sie stelle eine Verknüpfung zwischen zwei Ressourcen her, wenn die Ressourcen eine Abhängigkeit aufweisen, die nach der Bereitstellung bestehen bleibt. Beispielsweise kann eine App eine Verbindung mit einer Datenbank in einer anderen Ressourcengruppe herstellen. Sie können diese Abhängigkeit definieren, indem Sie eine Verknüpfung der App mit der Datenbank erstellen. Verknüpfungen ermöglichen Ihnen das Dokumentieren der Beziehung zwischen zwei Ressourcen. Später können Sie oder andere Personen in Ihrer Organisation eine Ressource auf Verknüpfungen abfragen, um zu ermitteln, wie die Ressource mit anderen Ressourcen zusammenarbeitet.

Alle verknüpften Ressourcen müssen zum selben Abonnement gehören. Jede Ressource kann mit 50 anderen Ressourcen verknüpft werden. Wenn verknüpfte Ressourcen gelöscht oder verschoben werden, muss der Besitzer des Links den verbleibenden Link bereinigen.

Informationen zum Arbeiten mit Verknüpfungen über REST finden Sie unter [Verknüpfte Ressourcen](https://msdn.microsoft.com/library/azure/mt238499.aspx).

Verwenden Sie den folgenden Azure PowerShell-Befehl, um alle Verknüpfungen in Ihrem Abonnement anzuzeigen. Sie können andere Parameter angeben, um die Ergebnisse einzuschränken.

    Get-AzureRmResource -ResourceType Microsoft.Resources/links -isCollection -ResourceGroupName <YourResourceGroupName>

## Beispiele

Im folgenden Beispiel wird eine Schreibschutzsperre für eine Web-App angewendet.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "hostingPlanName": {
                "type": "string"
            }
        },
        "variables": {
            "siteName": "[concat('site',uniqueString(resourceGroup().id))]"
        },
        "resources": [
            {
                "apiVersion": "2015-08-01",
                "type": "Microsoft.Web/serverfarms",
                "name": "[parameters('hostingPlanName')]",
                "location": "[resourceGroup().location]",
                "sku": {
                    "tier": "Free",
                    "name": "f1",
                    "capacity": 0
                },
                "properties": {
                    "numberOfWorkers": 1
                }
            },
            {
                "apiVersion": "2015-08-01",
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "dependsOn": [ "[parameters('hostingPlanName')]" ],
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                }
            },
            {
                "type": "Microsoft.Web/sites/providers/links",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties": {
                    "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
                    "notes": "This web site uses the storage account to store user information."
                }
    	    }
        ],
        "outputs": {}
    }

## Schnellstartvorlagen

Die folgenden Schnellstartvorlagen stellen Ressourcen mit einem Link bereit.

- [Warnung für Warteschlange mit Logik-App](https://azure.microsoft.com/documentation/templates/201-alert-to-queue-with-logic-app)
- [Warnung für Pufferzeit mit Logik-App](https://azure.microsoft.com/documentation/templates/201-alert-to-slack-with-logic-app)
- [Bereitstellen einer API-App mit einem vorhandenen Gateway](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-existing)
- [Bereitstellen einer API-App mit einem neuen Gateway](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-new)
- [Erstellen einer Logik-App und einer API-App mithilfe einer Vorlage](https://azure.microsoft.com/documentation/templates/201-logic-app-api-app-create)
- [Logik-App, die eine Textnachricht sendet, wenn eine Warnung ausgelöst wird](https://azure.microsoft.com/documentation/templates/201-alert-to-text-message-with-logic-app)


## Nächste Schritte

- Informationen zur Vorlagenstruktur finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](resource-group-authoring-templates.md).

<!----HONumber=AcomDC_0406_2016-->