---
title: "Azure PowerShell-Skriptbeispiel – Erstellen einer Web-App und Bereitstellen von Code über GitHub | Microsoft-Dokumentation"
description: "Azure PowerShell-Skriptbeispiel – Erstellen einer Web-App und Bereitstellen von Code über GitHub"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: 7ee385060bbb3800e8a37f2ae4ba7d06fb31b521
ms.lasthandoff: 03/10/2017

---

# <a name="create-a-web-app-and-deploy-code-from-github"></a>Erstellen einer Web-App und Bereitstellen von Code über GitHub

Dieses Beispielskript erstellt eine Web-App in App Service mit den zugehörigen Ressourcen und stellt dann Ihren Web-App-Code über ein öffentliches GitHub-Repository bereit (ohne Continuous Deployment). Informationen zur GitHub-Bereitstellung mit Continuous Deployment finden Sie unter [Erstellen einer Web-App mit Continuous Deployment über GitHub](app-service-powershell-continuous-deployment-github.md).

Stellen Sie vor dem Ausführen dieses Skripts sicher, dass über das Cmdlet `Login-AzureRmAccount` eine Verbindung mit Azure hergestellt wurde und dass Sie über die URL eines GitHub-Repositorys verfügen, über das die Bereitstellung erfolgen soll.

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-github/deploy-github.ps1?highlight=1-2 "Erstellen einer Web-App und Bereitstellen von Code über GitHub")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, die Web-App und alle zugehörigen Ressourcen entfernt werden.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Resources/v3.5.0/new-azurermresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzureRmAppServicePlan](https://docs.microsoft.com/powershell/resourcemanager/azurerm.websites/v2.5.0/new-azurermappserviceplan) | Erstellt einen App Service-Plan. |
| [New-AzureRmWebApp](https://docs.microsoft.com/powershell/resourcemanager/azurerm.websites/v2.5.0/new-azurermwebapp) | Erstellt die Web-App. |
| [Set-AzureRmResource](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/set-azurermresource) | Ändert eine Ressource in einer Ressourcengruppe. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/).

Zusätzliche Azure PowerShell-Beispiele für Azure App Service-Web-Apps finden Sie unter [Azure PowerShell-Beispiele](../app-service-powershell-samples.md).

