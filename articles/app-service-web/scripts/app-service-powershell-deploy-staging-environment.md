---
title: "Azure PowerShell-Skriptbeispiel – Erstellen einer Web-App und Bereitstellen von Code in einer Stagingumgebung | Microsoft-Dokumentation"
description: "Azure PowerShell-Skriptbeispiel – Erstellen einer Web-App und Bereitstellen von Code in einer Stagingumgebung"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 27cf0680-c3a9-4a58-9f71-6dec09f6b874
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 15ab92ff4983568e6c44eeb77927d9758a501118
ms.lasthandoff: 04/27/2017

---

# <a name="create-a-web-app-and-deploy-code-to-a-staging-environment"></a>Erstellen einer Web-App und Bereitstellen von Code in einer Stagingumgebung

Dieses Beispielskript erstellt eine Web-App in App Service mit einem zusätzlichen Bereitstellungsslot namens „staging“ und stellt dann eine Beispiel-App im „staging“-Slot bereit.

Installieren Sie bei Bedarf Azure PowerShell anhand der Anleitung im [Azure PowerShell-Handbuch](/powershell/azure/overview), und führen Sie dann `Login-AzureRmAccount` aus, um eine Verbindung mit Azure herzustellen.

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.ps1?highlight=1 "Erstellen einer Web-App und Bereitstellen von Code in einer Stagingumgebung")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach dem Ausführen des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, die Web-App und alle zugehörigen Ressourcen entfernt werden.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Erstellt einen App Service-Plan. |
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Erstellt die Web-App. |
| [Set-AzureRmAppServicePlan](/powershell/module/azurerm.websites/set-azurermappserviceplan) | Ändert einen App Service-Plan, um den zugehörigen Tarif zu ändern. |
| [New-AzureRmWebAppSlot](/powershell/module/azurerm.websites/new-azurermwebappslot) | Erstellt einen Bereitstellungsslot für eine Web-App. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) | Ändert eine Ressource in einer Ressourcengruppe. |
| [Swap-AzureRmWebAppSlot](/powershell/module/azurerm.websites/swap-azurermwebappslot) | Ändert den Bereitstellungsslot einer Web-App in eine Produktionsumgebung. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche Azure PowerShell-Beispiele für Azure App Service-Web-Apps finden Sie unter [Azure PowerShell-Beispiele](../app-service-powershell-samples.md).

