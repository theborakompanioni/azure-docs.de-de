---
title: High Density-Hosting in Azure App Service | Microsoft-Dokumentation
description: High Density-Hosting in Azure App Service
author: btardif
manager: erikre
editor: 
services: app-service\web
documentationcenter: 
ms.assetid: a903cb78-4927-47b0-8427-56412c4e3e64
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 06/12/2017
ms.author: byvinyal
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: 459a310a719695f6366470976d857ec2f9d6f4a1
ms.contentlocale: de-de
ms.lasthandoff: 06/15/2017


---
# <a name="high-density-hosting-on-azure-app-service"></a>High Density-Hosting in Azure App Service
Wenn Sie App Service verwenden, wird Ihre Anwendung von der ihr zugeordneten Kapazität entkoppelt. Dabei wird auf zwei Konzepte zurückgegriffen:

* **Die Anwendung:** Stellt die App und ihre Laufzeitkonfiguration dar. Sie enthält beispielsweise die Version von .NET, die von der Laufzeit geladen werden soll, und die App-Einstellungen.
* **Der App Service-Plan:** Definiert Eigenschaften wie Kapazität, verfügbarer Funktionsumfang und Standort für die Anwendung. Beispiele für Merkmale wären etwa: großer Computer (vier Kerne), vier Instanzen, Premium-Features in der Region „USA, Osten“.

Eine App ist immer mit einem App Service-Plan verknüpft, aber ein App Service-Plan kann Kapazitäten für eine App oder für mehrere Apps bereitstellen.

Die Plattform bietet daher die Flexibilität, eine einzelne App in einem App Service-Plan zu isolieren oder die Ressourcen eines App Service-Plans für mehrere Apps gemeinsam zu nutzen.

Wenn jedoch mehrere Apps einen App Service-Plan gemeinsam nutzen, wird eine Instanz dieser App auf jeder Instanz des App Service-Plans ausgeführt.

## <a name="per-app-scaling"></a>Skalierung pro App
*Skalierung pro App* handelt es sich um ein Feature, das auf der Ebene des App Service-Plans aktiviert werden und für jede einzelne Anwendung genutzt werden kann.

Bei der Skalierung pro App wird eine App unabhängig von dem App Service-Plan skaliert, von dem sie gehostet wird. Auf diese Weise kann ein App Service-Plan auf 10 Instanzen skaliert werden. Jedoch kann eine App nur auf das Verwenden von fünf Instanzen festgelegt werden.

   >[!NOTE]
   >Eine Skalierung pro App steht nur für **Premium**-SKU-App Service-Pläne zur Verfügung.
   >

### <a name="per-app-scaling-using-powershell"></a>Skalierung pro App mit PowerShell

Sie können einen Plan erstellen, der für eine *Skalierung pro App* konfiguriert ist, indem Sie das Attribut ```-perSiteScaling $true``` an das Cmdlet ```New-AzureRmAppServicePlan``` übergeben.

```
New-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

Wenn Sie einen vorhandenen App Service-Plan für die Verwendung dieses Features aktualisieren möchten: 

- Rufen Sie den Zielplan ab: ```Get-AzureRmAppServicePlan```
- Ändern Sie die Eigenschaft lokal: ```$newASP.PerSiteScaling = $true```
- Veröffentlichen Sie Ihre Änderungen in Azure: ```Set-AzureRmAppServicePlan``` 

```
# Get the new App Service Plan and modify the "PerSiteScaling" property.
$newASP = Get-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan
$newASP

#Modify the local copy to use "PerSiteScaling" property.
$newASP.PerSiteScaling = $true
$newASP
    
#Post updated app service plan back to azure
Set-AzureRmAppServicePlan $newASP
```

Auf App-Ebene müssen wir die Anzahl der Instanzen konfigurieren, die die App im App Service-Plan nutzen kann.

Im folgenden Beispiel ist die App auf zwei Instanzen beschränkt, und zwar unabhängig davon, auf wie viele Instanzen der zugrunde liegenden App Service-Plan horizontal hochskaliert wird.

```
# Get the app we want to configure to use "PerSiteScaling"
$newapp = Get-AzureRmWebApp -ResourceGroupName $ResourceGroup -Name $webapp
    
# Modify the NumberOfWorkers setting to the desired value.
$newapp.SiteConfig.NumberOfWorkers = 2
    
# Post updated app back to azure
Set-AzureRmWebApp $newapp
```

> [!IMPORTANT]
> „$newapp.SiteConfig.NumberOfWorkers“ unterscheidet sich von „$newapp.MaxNumberOfWorkers“. Die Skalierung pro App verwendet „$newapp.SiteConfig.NumberOfWorkers“ zum Ermitteln der Skalierungsmerkmale der App.

### <a name="per-app-scaling-using-azure-resource-manager"></a>Skalierung pro App mit Azure Resource Manager

Die folgende *Azure Resource Manager-Vorlage* erstellt Folgendes:

- Einen App Service-Plan, der bis zu 10 Instanzen horizontal hochskaliert wird
- Eine App, die für die Skalierung auf maximal fünf Instanzen konfiguriert wird

Im App Service-Plan wird die **PerSiteScaling**-Eigenschaft auf TRUE festgelegt (```"perSiteScaling": true```). Für die App wird die **Anzahl der Worker** auf 5 festgelegt (```"properties": { "numberOfWorkers": "5" }```).

```
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
            "name": "P1",
            "tier": "Premium",
            "size": "P1",
            "family": "P",
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
                "properties": { "numberOfWorkers": "5" }
            } ]
        }]
}
```

## <a name="recommended-configuration-for-high-density-hosting"></a>Empfohlene Konfiguration für High Density-Hosting
Bei der Skalierung pro App handelt es sich um ein Feature, das sowohl in globalen Azure-Regionen als auch in App Service-Umgebungen aktiviert ist. Es wird jedoch empfohlen, App Service-Umgebungen mit ihren hoch entwickelten Features und größeren Kapazitätspools zu nutzen.  

Führen Sie zum Konfigurieren des High Density-Hosting für Ihre Apps die folgenden Schritte aus:

1. Konfigurieren Sie die App Service-Umgebung, und wählen Sie einen Workerpool aus, der ausschließlich für das Szenario mit High Density-Hosting genutzt wird.
1. Erstellen Sie einen einzelnen App Service-Plan, und skalieren Sie ihn so, dass die gesamte verfügbare Kapazität für den Workerpool verwendet wird.
1. Legen Sie im App Service-Plan das PerSiteScaling-Flag auf TRUE fest.
1. Neue Apps werden erstellt und diesem App Service-Plan zugewiesen, wobei die **numberOfWorkers**-Eigenschaft auf **1** festgelegt wird. Durch die Konfiguration ergibt sich die höchstmögliche Dichte für diesen Workerpool.
1. Die Anzahl der Worker kann pro App unabhängig konfiguriert werden, um nach Bedarf zusätzliche Ressourcen zur Verfügung zu stellen. Beispiel:
    - Bei einer stark ausgelasteten App kann **numberOfWorkers** auf **3** festgelegt werden, damit diese App über mehr Verarbeitungskapazität verfügt. 
    - Bei selten verwendeten Apps kann **numberOfWorkers** auf **1** festgelegt werden.

## <a name="next-steps"></a>Nächste Schritte

- [Azure App Service-Pläne – Detaillierte Übersicht](azure-web-sites-web-hosting-plans-in-depth-overview.md)
- [Einführung in die App Service-Umgebung](../app-service-web/app-service-app-service-environment-intro.md)
