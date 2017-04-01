---
title: PowerShell-Skript zum Erstellen einer Application Insights-Ressource | Microsoft Docs
description: Automatisierung der Erstellung von Application Insights-Ressourcen.
services: application-insights
documentationcenter: windows
author: alancameronwills
manager: douge
ms.assetid: f0082c9b-43ad-4576-a417-4ea8e0daf3d9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/19/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: d84ab993b1d9489ca9d2edaa1cb9672d9bced899
ms.openlocfilehash: 4a7a4b719176a1d10bee2fc4f6b65204cc77bee8
ms.lasthandoff: 11/17/2016


---
# <a name="powershell-script-to-create-an-application-insights-resource"></a>PowerShell-Skript zum Erstellen einer Application Insights-Ressource


Wenn Sie eine neue Anwendung – oder eine neue Version einer Anwendung – mit [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) überwachen möchten, richten Sie eine neue Ressource in Microsoft Azure ein. In dieser Ressource werden die Telemetriedaten aus Ihrer App analysiert und angezeigt. 

Sie können die Erstellung einer neuen Ressource mithilfe von PowerShell automatisieren.

Wenn Sie beispielsweise eine App für mobile Geräte entwickeln, ist es wahrscheinlich, dass irgendwann mehrere veröffentlichte Versionen Ihrer App von Ihren Kunden verwendet werden. Sie möchten vermeiden, dass die Telemetrieergebnisse aus verschiedenen Versionen durcheinander geraten. Daher richten Sie den Buildprozess so ein, dass für jeden Build eine neue Ressource erstellt wird.

> [!NOTE]
> Wenn Sie mehrere Ressourcen gleichzeitig erstellen möchten, sollten Sie [die Ressourcen mithilfe einer Azure-Vorlage erstellen](app-insights-powershell.md).
> 
> 

## <a name="script-to-create-an-application-insights-resource"></a>Skript zum Erstellen einer Application Insights-Ressource
Die wichtigsten Cmdlet-Spezifikationen:

* [New-AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [New-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)

*PowerShell-Skript*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Add-AzureRmAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 

$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

# Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource


$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName Fabrikam `
  -Tag @{ applicationType = "web", applicationName = $applicationTagName} `
  -ResourceType "Microsoft.Insights/components" `
  -Location "East US" `  // or North Europe, West Europe, South Central US
  -PropertyObject @{"Application_Type"="web"} `
  -Force

# Give owner access to the team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


# Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-to-do-with-the-ikey"></a>Zweck des "iKey"
Jede Ressource wird durch ihren Instrumentationsschlüssel (iKey) identifiziert. Der iKey ist eine Ausgabe des Skripts zum Erstellen der Ressource. Ihr Buildskript sollte den iKey dem Application Insights-SDK bereitstellen, das in Ihrer App eingebettet ist.

Es gibt zwei Möglichkeiten, den iKey dem SDK zur Verfügung zu stellen:

* In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md): 
  * `<instrumentationkey>`*ikey*`</instrumentationkey>`
* Oder im [Initialisierungscode](app-insights-api-custom-events-metrics.md): 
  * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`

## <a name="see-also"></a>Siehe auch
* [Erstellen von Application Insights- und Webtestressourcen aus Vorlagen](app-insights-powershell.md)
* [Einrichten einer Überwachung der Azure-Diagnose mit PowerShell](app-insights-powershell-azure-diagnostics.md) 
* [Festlegen von Warnungen mit PowerShell](app-insights-powershell-alerts.md)


