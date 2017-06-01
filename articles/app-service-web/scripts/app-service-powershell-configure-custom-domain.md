---
title: "Azure PowerShell-Skriptbeispiel – Zuweisen einer benutzerdefinierten Domäne an eine Web-App | Microsoft-Dokumentation"
description: "Azure PowerShell-Skriptbeispiel – Zuweisen einer benutzerdefinierten Domäne an eine Web-App"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 356f5af9-f62e-411c-8b24-deba05214103
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: a8a0a8ee9ae03afe27898efd1eb0bd15587f34a3
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017

---

# <a name="assign-a-custom-domain-to-a-web-app"></a>Zuweisen einer benutzerdefinierten Domäne an eine Web-App

Dieses Beispielskript erstellt eine Web-App in App Service mit den zugehörigen Ressourcen und ordnet ihr dann `www.<yourdomain>` zu. 

Installieren Sie bei Bedarf Azure PowerShell anhand der Anleitung im [Azure PowerShell-Handbuch](/powershell/azure/overview), und führen Sie dann `Login-AzureRmAccount` aus, um eine Verbindung mit Azure herzustellen. Sie benötigen außerdem Zugriff auf die Seite für die DNS-Konfiguration der Domänenregistrierungsstelle.

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/app-service/map-custom-domain/map-custom-domain.ps1?highlight=1 "Zuweisen einer benutzerdefinierten Domäne an eine Web-App")]

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
| [Set-AzureRmWebApp](/powershell/module/azurerm.websites/set-azurermwebapp) | Ändert die Konfiguration einer Web-App. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche Azure PowerShell-Beispiele für Azure App Service-Web-Apps finden Sie unter [Azure PowerShell-Beispiele](../app-service-powershell-samples.md).

