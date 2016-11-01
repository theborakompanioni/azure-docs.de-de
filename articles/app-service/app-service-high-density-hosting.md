<properties
	pageTitle="High Density-Hosting in Azure App Service | Microsoft Azure"
	description="High Density-Hosting in Azure App Service"
	authors="btardif"
	manager="wpickett"
	editor=""
	services="app-service\web"
	documentationCenter=""/>

<tags
	ms.service="app-service-web"
	ms.workload="web"
	ms.tgt_pltfrm="na"
	ms.devlang="multiple"
	ms.topic="article"
	ms.date="08/07/2016"
	ms.author="byvinyal"/>

# High Density-Hosting in Azure App Service#

Wenn Sie App Service verwenden, wird Ihre Anwendung von der ihr zugeordneten Kapazität entkoppelt. Dabei wird auf zwei Konzepte zurückgegriffen:

- **Die Anwendung:** Stellt die App und ihre Laufzeitkonfiguration dar. Sie enthält beispielsweise die Version von .NET, die von der Laufzeit geladen werden soll, die App-Einstellungen usw.

- **Der App Service-Plan:** Definiert Eigenschaften wie Kapazität, verfügbarer Funktionsumfang und Standort für die Anwendung. Beispiele für Merkmale wären etwa: großer Computer (vier Kerne), vier Instanzen, Premium-Features in der Region „USA, Osten“.

Eine App ist immer mit einem App Service-Plan verknüpft, aber ein App Service-Plan kann Kapazitäten für eine App oder für mehrere Apps bereitstellen.

Das bedeutet: Die Plattform bietet die Flexibilität, eine einzelne App in einem App Service-Plan zu isolieren oder die Ressourcen eines App Service-Plans für mehrere Apps gemeinsam zu nutzen.

Wenn jedoch mehrere Apps einen App Service-Plan gemeinsam nutzen, wird eine Instanz dieser App auf jeder Instanz des App Service-Plans ausgeführt.

## Skalierung pro App##
Bei der *Skalierung pro App* handelt es sich um ein Feature, das auf der Ebene des App Service-Plans aktiviert werden und für jede einzelne Anwendung genutzt werden kann.

Bei der Skalierung pro App wird eine App unabhängig von dem App Service-Plan skaliert, von dem sie gehostet wird. Auf diese Weise kann ein App Service-Plan beispielsweise zur Bereitstellung von zehn Instanzen konfiguriert werden, aber für eine einzelne App kann eine Skalierung auf nur fünf dieser Instanzen festgelegt werden.

Mit der folgenden Azure Resource Manager-Vorlage werden ein App Service-Plan, der auf zehn Instanzen horizontal hochskaliert wird, und eine App erstellt, die für die Verwendung der Skalierung pro App konfiguriert und auf nur fünf Instanzen skaliert wird.

Zu diesem Zweck wird im App Service-Plan die Eigenschaft für die **Skalierung pro Standort** auf TRUE festgelegt (`"perSiteScaling": true`), und für die App wird die **Anzahl der Worker** auf 1 festgelegt (`"properties": { "numberOfWorkers": "1" }`).

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters":{
            "appServicePlanName": { "type": "string" },
            "appName": { "type": "string" }
         },
        "resources": [
        {
            "comments": "App Service Plan with per site perSiteScaling = true",
            "type": "Microsoft.Web/serverFarms",
            "sku": {
                "name": "S1",
                "tier": "Standard",
                "size": "S1",
                "family": "S",
                "capacity": 10
                },
            "name": "[parameters('appServicePlanName')]",
            "apiVersion": "2015-08-01",
            "location": "West US",
            "properties": {
                "name": "[parameters('appServicePlanName')]",
                "perSiteScaling": true
            }
        },
        {
            "type": "Microsoft.Web/sites",
            "name": "[parameters('appName')]",
            "apiVersion": "2015-08-01-preview",
            "location": "West US",
            "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
            "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
            "resources": [ {
                    "comments": "",
                    "type": "config",
                    "name": "web",
                    "apiVersion": "2015-08-01",
                    "location": "West US",
                    "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                    "properties": { "numberOfWorkers": "1" }
             } ]
         }]
    }


## Empfohlene Konfiguration für High Density-Hosting

Bei der Skalierung pro App handelt es sich um ein Feature, das sowohl in öffentlichen Azure-Regionen als auch in App Service-Umgebungen aktiviert ist. Es wird jedoch empfohlen, App Service-Umgebungen mit ihren hoch entwickelten Features und größeren Kapazitätspools zu nutzen.

Führen Sie zum Konfigurieren des High Density-Hosting für Ihre Apps die folgenden Schritte aus:

1. Konfigurieren Sie die App Service-Umgebung, und wählen Sie einen Workerpool aus, der ausschließlich für das Szenario mit High Density-Hosting genutzt wird.

1. Erstellen Sie einen einzelnen App Service-Plan, und skalieren Sie ihn so, dass die gesamte verfügbare Kapazität für den Workerpool verwendet wird.

1. Legen Sie im App Service-Plan das Flag für die Skalierung pro Standort auf TRUE fest.

1. Es werden neue Standorte erstellt und diesem App Service-Plan zugewiesen, wobei die **numberOfWorkers**-Eigenschaft auf **1** festgelegt wird. Dadurch ergibt sich die höchstmögliche Dichte für diesen Workerpool.

1. Die Anzahl der Worker kann pro Standort unabhängig konfiguriert werden, um nach Bedarf zusätzliche Ressourcen zur Verfügung zu stellen. Beispielsweise kann für einen Standort mit hoher Nutzung für **numberOfWorkers** der Wert **3** festgelegt werden, um mehr Verarbeitungskapazität für die App bereitzustellen, während bei weniger stark genutzten Standorten **numberOfWorkers** auf **1** festgelegt wird.

<!---HONumber=AcomDC_0907_2016-->