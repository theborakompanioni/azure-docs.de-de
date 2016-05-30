<properties 
	pageTitle="High Density-Hosting in Azure App Service" 
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
	ms.date="05/17/2016" 
	ms.author="byvinyal"/>

#High Density-Hosting in Azure App Service#

##Grundlegendes zur App-Skalierung##

Wenn Sie App Service verwenden, wird Ihre Anwendung von der ihr zugeordneten Kapazität entkoppelt. Dabei wird auf zwei Konzepte zurückgegriffen:
 
>**Die Anwendung:** Stellt die App und ihre Laufzeitkonfiguration dar. Zum Beispiel: Welche Version von .NET sollte die Runtime laden? Welche App-Einstellungen gelten?

>**Der App Service-Plan:** Definiert Eigenschaften wie Kapazität, verfügbarer Funktionsumfang und Standort für die Anwendung. Zum Beispiel ein Computer mit folgender Bemessung: Groß (4 Kerne), 4 Instanzen, Premium-Features, Region „USA, Osten“.

Eine App ist immer mit einem **App Service-Plan** verknüpft, aber ein **App Service-Plan** kann Kapazitäten für eine App oder für mehrere Apps bereitstellen.

Das bedeutet: Die Plattform bietet die Flexibilität, eine einzelne App in einem **App Service-Plan** zu isolieren oder die Ressourcen eines App Service-Plans für mehrere Apps gemeinsam zu nutzen.

Wenn jedoch mehrere Apps einen **App Service-Plan** gemeinsam nutzen, gibt es eine Instanz dieser App, die auf jeder Instanz des **App Service-Plans** ausgeführt wird.

##Skalierung pro App##
Bei der **Skalierung pro App** handelt es sich um ein Feature, das auf der Ebene des **App Service-Plans** aktiviert werden und für jede einzelne Anwendung genutzt werden kann.

Mit **Skalierung pro App** können Sie eine App unabhängig von dem **App Service-Plan** skalieren, der zum Hosten dieser App verwendet wird. Das heißt: Ein **App Service-Plan** kann z.B. zur Bereitstellung von 10 Instanzen konfiguriert werden, aber für eine einzelne App kann eine Skalierung auf bis zu 5 dieser Instanzen festgelegt werden.

Mit der folgenden ARM-Vorlage (Azure Resource Manager) wird ein **App Service-Plan** erstellt, der auf 10 Instanzen horizontal skaliert wird. Außerdem wird eine App zur Verwendung von **Skalierung pro App** und zu einer Skalierung auf nur maximal 5 Instanzen konfiguriert.

Zu diesem Zweck wird im App Service-Plan die Eigenschaft für die Skalierung pro Standort auf „true“ festgelegt (`"properties": { "numberOfWorkers": "1" }`), und für die App wird die Anzahl der zu verwendeten Worker auf 1 festgelegt: `"perSiteScaling": true`

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


##Empfohlene Konfiguration für High Density-Hosting##

Die **Skalierung pro App** ist ein Feature, das sowohl in den öffentlichen Azure-Regionen als auch in App Service-Umgebungen aktiviert wird. Es wird jedoch empfohlen, die App Service-Umgebungen zu nutzen und deren hoch entwickelte Features und die größeren Kapazitätspools zu verwenden.

Nutzen Sie die nachfolgend aufgeführten Schritte als Richtlinie zum Konfigurieren von **High Density-Hosting** für Ihre Apps.

1. Konfigurieren Sie die **App Service-Umgebung**, und wählen Sie einen **Workerpool** aus, der ausschließlich für das Szenario mit *High Density-Hosting* genutzt wird.

1. Erstellen Sie einen einzelnen **App Service-Plan** und skalieren Sie ihn so, dass seine gesamte verfügbare Kapazität für den **Workerpool** verwendet wird.

1. Legen Sie im **App Service-Plan** das Flag für die Skalierung pro Standort auf „true“ fest.

1. Es werden neue Standorte erstellt und diesem **App Service-Plan** zugewiesen, wobei die *numberOfWorkers*-Eigenschaft auf *1* festgelegt wird. Dadurch ergibt sich die höchstmögliche Dichte für diesen **Workerpool**.

1. Die Anzahl der Worker kann pro Standort unabhängig konfiguriert werden, um nach Bedarf zusätzliche Ressourcen zur Verfügung zu stellen. Beispielsweise kann für einen Standort mit hoher Nutzung für *numberOfWorkers* der Wert *3* festgelegt werden, während bei weniger stark genutzten Standorten *numberOfWorkers* auf *1* festgelegt wird.

<!---HONumber=AcomDC_0518_2016-->