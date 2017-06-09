---
title: Bereitstellen einer App in einer Azure-VM-Skalierungsgruppe | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine einfache Anwendung zur automatischen Skalierung mithilfe einer Azure Resource Manager-Vorlage in einer VM-Skalierungsgruppe bereitstellen.
services: virtual-machine-scale-sets
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/4/2017
ms.author: ryanwi
ms.translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: 8a903cd870f01f9ca6224efd1386b68c63e3aa98
ms.contentlocale: de-de
ms.lasthandoff: 04/06/2017


---

# <a name="deploy-an-autoscaling-app-using-a-template"></a>Bereitstellen einer App für die automatische Skalierung mithilfe einer Vorlage

[Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) sind eine hervorragende Möglichkeit, Gruppen aufeinander bezogener Ressourcen bereitzustellen. Dieses Tutorial baut auf [Bereitstellen einer einfachen Skalierungsgruppe](virtual-machine-scale-sets-mvss-start.md) auf und beschreibt, wie Sie eine einfache Anwendung zur automatischen Skalierung unter Verwendung einer Azure Resource Manager-Vorlage in einer Skalierungsgruppe bereitstellen.  Sie können die automatische Skalierung auch mithilfe von PowerShell, CLI oder über das Portal einrichten. Weitere Informationen finden Sie unter [Übersicht über die automatische Skalierung](virtual-machine-scale-sets-autoscale-overview.md).

## <a name="two-quickstart-templates"></a>Zwei Schnellstartvorlagen
Wenn Sie eine Skalierungsgruppe bereitstellen, können Sie neue Software mithilfe einer [VM-Erweiterung](../virtual-machines/virtual-machines-windows-extensions-features.md) auf einem Plattformimage installieren. Eine VM-Erweiterung ist eine kleine Anwendung, die Konfigurations- und Automatisierungsaufgaben auf virtuellen Azure-Computern nach der Bereitstellung ermöglicht. In [Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) sind zwei unterschiedliche Beispielvorlagen enthalten, die zeigen, wie Sie eine Anwendung zur automatischen Skalierung mithilfe einer VM-Erweiterung in einer Skalierungsgruppe bereitstellen.

### <a name="python-http-server-on-linux"></a>Python-HTTP-Server unter Linux
Mit der Beispielvorlage für einen [Python-HTTP-Server unter Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) wird eine einfache Anwendung zur automatischen Skalierung in einer Linux-Skalierungsgruppe bereitgestellt.  Bei diesem Beispiel werden mithilfe einer benutzerdefinierten Skript-VM-Erweiterung [Bottle](http://bottlepy.org/docs/dev/), ein Python-Framework, und ein einfacher HTTP-Server in jeder VM der Skalierungsgruppe bereitgestellt. Die Skalierungsgruppe wird zentral hochskaliert, wenn die durchschnittliche CPU-Auslastung VM-übergreifend auf über 60 % ansteigt. Sinkt die durchschnittliche CPU-Auslastung unter 30 %, wird zentral herunterskaliert.

Zusätzlich zur Skalierungsgruppenressource deklariert die Beispielvorlage *azuredeploy.json* auch Ressourcen für ein virtuelles Netzwerk, eine öffentliche IP-Adresse, einen Load Balancer sowie Einstellungen für die automatische Skalierung.  Weitere Informationen zum Erstellen dieser Ressourcen in einer Vorlage finden Sie unter [Linux-Skalierungsgruppe mit automatischer Skalierung](virtual-machine-scale-sets-linux-autoscale.md).

In der Vorlage *azuredeploy.json* gibt die `extensionProfile`-Eigenschaft der Ressource `Microsoft.Compute/virtualMachineScaleSets` eine benutzerdefinierte Skripterweiterung an. Mit `fileUris` wird der Skriptspeicherort angegeben. In diesem Fall handelt es sich um zwei Dateien: *workserver.py* definiert einen einfachen HTTP-Server, *installserver.sh* installiert Bottle und startet den HTTP-Server. `commandToExecute` gibt den Befehl an, der nach dem Bereitstellen der Skalierungsgruppe ausgeführt werden soll.

```json
          "extensionProfile": {
            "extensions": [
              {
                "name": "lapextension",
                "properties": {
                  "publisher": "Microsoft.Azure.Extensions",
                  "type": "CustomScript",
                  "typeHandlerVersion": "2.0",
                  "autoUpgradeMinorVersion": true,
                  "settings": {
                    "fileUris": [
                      "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
                      "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
                    ],
                    "commandToExecute": "bash installserver.sh"
                  }
                }
              }
            ]
          }
```

### <a name="aspnet-mvc-application-on-windows"></a>ASP.NET MVC-Anwendung unter Windows
Die Beispielvorlage für eine [ASP.NET MVC-Anwendung unter Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) stellt eine einfache ASP.NET MVC-App bereit, die in IIS in einer Windows-Skalierungsgruppe ausgeführt wird.  IIS und die MVC-App werden mithilfe der VM-Erweiterung [PowerShell Desired State Configuration (DSC)](virtual-machine-scale-sets-dsc.md) bereitgestellt.  Die Skalierungsgruppe wird (jeweils eine VM-Instanz) zentral hochskaliert, wenn die CPU-Auslastung für 5 Minuten über 50 % liegt. 

Zusätzlich zur Skalierungsgruppenressource deklariert die Beispielvorlage *azuredeploy.json* auch Ressourcen für ein virtuelles Netzwerk, eine öffentliche IP-Adresse, einen Load Balancer sowie Einstellungen für die automatische Skalierung. Diese Vorlage demonstriert außerdem ein Anwendungsupgrade.  Weitere Informationen zum Erstellen dieser Ressourcen in einer Vorlage finden Sie unter [Windows-Skalierungsgruppe mit automatischer Skalierung](virtual-machine-scale-sets-windows-autoscale.md).

In der Vorlage *azuredeploy.json* gibt die `extensionProfile`-Eigenschaft der Ressource `Microsoft.Compute/virtualMachineScaleSets` eine [DSC](virtual-machine-scale-sets-dsc.md)-Erweiterung (Desired State Configuration) an, die IIS und eine standardmäßige Web-App aus einem WebDeploy-Paket installiert.  Das im Ordner *DSC* enthaltene Skript *IISInstall.ps1* installiert IIS auf dem virtuellen Computer.  Die MVC-Web-App befindet sich im Ordner *WebDeploy*.  Die Pfade zum Installationsskript und der Web-App werden in den Parametern `powershelldscZip` und `webDeployPackage` der Datei *zuredeploy.parameters.json* definiert. 

```json
          "extensionProfile": {
            "extensions": [
              {
                "name": "Microsoft.Powershell.DSC",
                "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.9",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('powershelldscUpdateTagVersion')]",
                  "settings": {
                    "configuration": {
                      "url": "[variables('powershelldscZipFullPath')]",
                      "script": "IISInstall.ps1",
                      "function": "InstallIIS"
                    },
                    "configurationArguments": {
                      "nodeName": "localhost",
                      "WebDeployPackagePath": "[variables('webDeployPackageFullPath')]"
                    }
                  }
                }
              }
            ]
          }
```

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage
Die einfachste Methode zum Bereitstellen der Vorlagen für den [Python-HTTP-Server unter Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) oder die [ASP.NET MVC-Anwendung unter Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) besteht darin, die Schaltfläche zur **Bereitstellung in Azure** in den Infodateien in GitHub zu verwenden.  Sie können auch PowerShell oder die Azure CLI zum Bereitstellen der Beispielvorlagen verwenden.

### <a name="powershell"></a>PowerShell
Kopieren Sie die Dateien für den [Python-HTTP-Server unter Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) oder die [ASP.NET MVC-Anwendung unter Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) aus dem GitHub-Repository in einen Ordner auf Ihrem lokalen Computer.  Öffnen Sie die Datei *azuredeploy.parameters.json*, und aktualisieren Sie die Standardwerte der Parameter `vmssName`, `adminUsername` und `adminPassword`. Speichern Sie das folgende PowerShell-Skript in *deploy.ps1* im selben Ordner wie die Vorlage *azuredeploy.json*. Um die Beispielvorlage bereitzustellen, führen Sie das Skript *deploy.ps1* über ein PowerShell-Befehlsfenster aus.

```powershell
param(
 [Parameter(Mandatory=$True)]
 [string]
 $subscriptionId,

 [Parameter(Mandatory=$True)]
 [string]
 $resourceGroupName,

 [string]
 $resourceGroupLocation,

 [Parameter(Mandatory=$True)]
 [string]
 $deploymentName,

 [string]
 $templateFilePath = "azuredeploy.json",

 [string]
 $parametersFilePath = "azuredeploy.parameters.json"
)

<#
.SYNOPSIS
    Registers RPs
#>
Function RegisterRP {
    Param(
        [string]$ResourceProviderNamespace
    )

    Write-Host "Registering resource provider '$ResourceProviderNamespace'";
    Register-AzureRmResourceProvider -ProviderNamespace $ResourceProviderNamespace;
}

#******************************************************************************
# Script body
# Execution begins here
#******************************************************************************
$ErrorActionPreference = "Stop"

# sign in
Write-Host "Logging in...";
Login-AzureRmAccount;

# select subscription
Write-Host "Selecting subscription '$subscriptionId'";
Select-AzureRmSubscription -SubscriptionID $subscriptionId;

# Register RPs
$resourceProviders = @("microsoft.resources");
if($resourceProviders.length) {
    Write-Host "Registering resource providers"
    foreach($resourceProvider in $resourceProviders) {
        RegisterRP($resourceProvider);
    }
}

#Create or check for existing resource group
$resourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ErrorAction SilentlyContinue
if(!$resourceGroup)
{
    Write-Host "Resource group '$resourceGroupName' does not exist. To create a new resource group, please enter a location.";
    if(!$resourceGroupLocation) {
        $resourceGroupLocation = Read-Host "resourceGroupLocation";
    }
    Write-Host "Creating resource group '$resourceGroupName' in location '$resourceGroupLocation'";
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else{
    Write-Host "Using existing resource group '$resourceGroupName'";
}

# Start the deployment
Write-Host "Starting deployment...";
if(Test-Path $parametersFilePath) {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -TemplateParameterFile $parametersFilePath;
} else {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath;
}
```

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]

