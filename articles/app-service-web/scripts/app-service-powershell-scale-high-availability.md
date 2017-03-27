---
title: "Azure PowerShell-Skriptbeispiel – weltweites Skalieren einer Web-App mit einer Hochverfügbarkeitsarchitektur | Microsoft-Dokumentation"
description: "Azure PowerShell-Skriptbeispiel – weltweites Skalieren einer Web-App mit einer Hochverfügbarkeitsarchitektur"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 470f0129-1efe-462c-a029-5c66e04158a8
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/08/2017
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: 8e21e2c88578177eb8bc6120ecfcd1222b173ff2
ms.lasthandoff: 03/10/2017

---

# <a name="scale-a-web-app-worldwide-with-a-high-availability-architecture"></a>Weltweites Skalieren einer Web-App mit einer Hochverfügbarkeitsarchitektur

In diesem Szenario erstellen Sie eine Ressourcengruppe, zwei App-Servicepläne, zwei Web-Apps, ein Traffic Manager-Profil und zwei Traffic Manager-Endpunkte. Nach dem Abschluss der Übung verfügen Sie über eine hoch verfügbare Architektur, die – basierend auf der niedrigsten Netzwerklatenz – globale Verfügbarkeit für Ihre Web-App ermöglicht.

Stellen Sie vor dem Ausführen dieses Skripts sicher, dass über das Cmdlet `Login-AzureRmAccount` eine Verbindung mit Azure hergestellt wurde.

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/app-service/scale-geographic/scale-geographic.ps1 "Weltweites Skalieren einer Web-App mit einer Hochverfügbarkeitsarchitektur")]

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
| [New-AzureRMTrafficManagerProfile](https://docs.microsoft.com/powershell/resourcemanager/azurerm.trafficmanager/v2.5.0/new-azurermtrafficmanagerprofile) | Erstellt ein Traffic Manager-Profil. |
| [New-AzureRmAppServicePlan](https://docs.microsoft.com/powershell/resourcemanager/azurerm.websites/v2.5.0/new-azurermappserviceplan) | Erstellt einen App Service-Plan. |
| [New-AzureRmWebApp](https://docs.microsoft.com/powershell/resourcemanager/azurerm.websites/v2.5.0/new-azurermwebapp) | Erstellt die Web-App. |
| [New-AzureRMTrafficManagerEndpoint](https://docs.microsoft.com/powershell/resourcemanager/azurerm.trafficmanager/v2.5.0/new-azurermtrafficmanagerendpoint) | Erstellt einen Endpunkt in einem Traffic Manager-Profil. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/).

Zusätzliche Azure PowerShell-Beispiele für Azure App Service-Web-Apps finden Sie unter [Azure PowerShell-Beispiele](../app-service-powershell-samples.md).

