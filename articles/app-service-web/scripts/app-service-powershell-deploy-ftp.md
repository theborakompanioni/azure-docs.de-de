---
title: "Azure PowerShell-Skriptbeispiel – Hochladen von Dateien in eine Web-App mithilfe von FTP | Microsoft-Dokumentation"
description: "Azure PowerShell-Skriptbeispiel – Hochladen von Dateien in eine Web-App mithilfe von FTP"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: b7d46d6f-44fd-454c-8008-87dab6eefbc1
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: 5f3bf8087fa6601ae410d6db907471fd6faa2742
ms.lasthandoff: 03/10/2017

---

# <a name="upload-files-to-a-web-app-using-ftp"></a>Hochladen von Dateien in eine Web-App mithilfe von FTP

Dieses Skriptbeispiel führt Folgendes über Azure PowerShell aus: 

* Erstellt eine Web-App im Azure App Service der Azure-Region „Europa, Westen“.
* Ruft die FTP-Verbindungsinformationen von einer Web-App ab.
* Stellt Web-App-Code mithilfe von FTP bereit (über [WebClient.UploadFile()](https://msdn.microsoft.com/library/ms144229.aspx)).

## <a name="prerequisites"></a>Voraussetzungen

* Führen Sie `Login-AzureRmAccount` aus, um sich bei Azure anzumelden.

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-ftp/deploy-ftp.ps1?highlight=1 "Hochladen von Dateien in eine Web-App mithilfe von FTP")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, die Web-App und alle zugehörigen Ressourcen entfernt werden.

```powershell
Remove-AzureRmResourceGroup -Name $webappname -Force
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Resources/v3.5.0/new-azurermresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzureRmAppServicePlan](https://docs.microsoft.com/powershell/resourcemanager/azurerm.websites/v2.5.0/new-azurermappserviceplan) | Erstellt einen App Service-Plan. |
| [New-AzureRmWebApp](https://docs.microsoft.com/powershell/resourcemanager/azurerm.websites/v2.5.0/new-azurermwebapp) | Erstellt die Web-App. |
| [Get-AzureRmWebAppPublishingProfile](https://docs.microsoft.com/powershell/resourcemanager/azurerm.websites/v2.5.0/get-azurermwebapppublishingprofile) | Ruft das Veröffentlichungsprofil einer Web-App ab. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/).

Zusätzliche Azure PowerShell-Beispiele für Azure App Service-Web-Apps finden Sie unter [Azure PowerShell-Beispiele](../app-service-powershell-samples.md).

