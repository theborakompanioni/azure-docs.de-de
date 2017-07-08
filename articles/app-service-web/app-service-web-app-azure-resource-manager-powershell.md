---
title: Azure Resource Manager-basierte PowerShell-Befehle zum Verwalten von Azure-Web-Apps | Microsoft Docs
description: Erfahren Sie, wie die neuen Azure Resource Manager-basierten PowerShell-Befehle zum Verwalten Ihrer Azure-Web-Apps verwendet werden.
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: 4231fbba-61e5-4f92-b958-531c066fb87f
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2016
ms.author: aelnably
ms.translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 8d574f051a327ba0409e6f25a5886af673d3d5e0
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
# <a name="using-azure-resource-manager-based-powershell-to-manage-azure-web-apps"></a>Verwenden von auf Azure Resource Manager basierenden PowerShell-Befehlen zum Verwalten von Azure-Web-Apps
> [!div class="op_single_selector"]
> * [Azure-Befehlszeilenschnittstelle](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)
> 
> 

In Version 1.0.0 von Microsoft Azure PowerShell wurden neue Befehle hinzugefügt, die Benutzern die Möglichkeit bieten, Web-Apps mithilfe von PowerShell-Befehlen zu verwalten, die auf Azure Resource Manager basieren.

Informationen zum Verwalten von Ressourcengruppen finden Sie unter [Verwenden von Azure PowerShell mit Azure Resource Manager](../powershell-azure-resource-manager.md). 

Die vollständige Liste von Parametern und Optionen für die PowerShell-Cmdlets finden Sie in der [vollständigen Cmdlet-Referenz für die Azure Resource Manager-basierten PowerShell-Cmdlets für Web-Apps](https://msdn.microsoft.com/library/mt619237.aspx)

## <a name="managing-app-service-plans"></a>Verwalten von App Service-Plänen
### <a name="create-an-app-service-plan"></a>Erstellen eines App Service-Plans
Zum Erstellen eines App Service-Plans verwenden Sie das Cmdlet **New-AzureRmAppServicePlan** .

Es folgen Beschreibungen der verschiedenen Parameter:

* **Name**: Name des App Service-Plans.
* **Location**: Region des App Service-Plans.
* **ResourceGroupName**: Ressourcengruppe, die den neu erstellten App Service-Plan enthält.
* **Tier**: Der gewünschte Tarif (Standardeinstellung ist „Free“, weitere Optionen sind „Shared“, „Basic“, „Standard“ und „Premium“).
* **WorkerSize**: Die Größe der Worker (Standard ist „Small“, wenn der Parameter „Tier“ als „Basic“, „Standard“ oder „Premium“ angegeben wurde. Andere Optionen sind „Medium“ und „Large“.)
* **NumberofWorkers**: Anzahl der Worker im App Service-Plan (Standardwert 1). 

Beispiel für die Verwendung dieses Cmdlets:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a>Erstellen eines App Service-Plans in einer App Service-Umgebung
Zum Erstellen eines App Service-Plans in einer App Service-Umgebung wird derselbe Befehl – **New-AzureRmAppServicePlan** – mit zusätzlichen Parametern zum Angeben des Namens der App Service-Umgebung sowie des Namens ihrer Ressourcengruppe verwendet.

Beispiel für die Verwendung dieses Cmdlets:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

Informationen zur App Service-Umgebung finden Sie unter [Einführung in die App Service-Umgebung](app-service-app-service-environment-intro.md)

### <a name="list-existing-app-service-plans"></a>Auflisten vorhandener App Service-Pläne
Verwenden Sie zum Auflisten vorhandener App Service-Pläne das Cmdlet **Get-AzureRmAppServicePlan** .

Verwenden Sie Folgendes zum Auflisten aller App Service-Pläne in Ihrem Abonnement: 

    Get-AzureRmAppServicePlan

Verwenden Sie zum Auflisten aller App Service-Pläne in einer bestimmten Ressourcengruppe Folgendes:

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

Verwenden Sie zum Abrufen eines bestimmten App Service-Plans Folgendes:

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a>Konfigurieren eines vorhandenen App Service-Plans
Verwenden Sie zum Ändern der Einstellungen eines vorhandenen App Service-Plans das Cmdlet **Set-AzureRmAppServicePlan** . Sie können den Tarif, die Workergröße und Anzahl der Worker ändern. 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a>Skalieren eines App Service-Plans
Zum Skalieren eines vorhandenen App Service-Plans verwenden Sie Folgendes:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-the-worker-size-of-an-app-service-plan"></a>Ändern der Workergröße eines App Service-Plans
Verwenden Sie zum Ändern der Größe der Worker in einem vorhandenen App Service-Plan Folgendes:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-the-tier-of-an-app-service-plan"></a>Ändern des Tarifs eines App Service-Plans
Verwenden Sie zum Ändern des Tarifs eines vorhandenen App Service-Plans Folgendes:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a>Löschen eines vorhandenen App Service-Plans
Zum Löschen eines vorhandenen App Service-Plans müssen alle zugewiesenen Web-Apps zuerst verschoben oder gelöscht werden. Anschließend können Sie mit dem Cmdlet **Remove-AzureRmAppServicePlan** den App Service-Plan löschen.

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a>Verwalten von Azure App Service-Web-Apps
### <a name="create-a-web-app"></a>Erstellen einer Web-App
Verwenden Sie zum Erstellen einer Web-App das Cmdlet **New-AzureRmWebApp**.

Es folgen Beschreibungen der verschiedenen Parameter:

* **Name**: Name der Web-App.
* **AppServicePlan**: Name des Service-Plans, der zum Hosten der Web-App verwendet wird.
* **ResourceGroupName**: Ressourcengruppe, die den App Service-Plan hostet.
* **Location**: Der Speicherort der Web-App.

Beispiel für die Verwendung dieses Cmdlets:

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a>Erstellen einer Web-App in einer App Service-Umgebung
So erstellen Sie eine Web-App in einer App Service-Umgebung (App Service Environment, ASE) Verwenden Sie denselben Befehl – **New-AzureRmWebApp** – mit zusätzlichen Parametern zum Angeben des Namens der App Service-Umgebung und der Ressourcengruppe, der die App Service-Umgebung angehört.

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

Informationen zur App Service-Umgebung finden Sie unter [Einführung in die App Service-Umgebung](app-service-app-service-environment-intro.md)

### <a name="delete-an-existing-web-app"></a>Löschen einer vorhandenen Web-App
Zum Löschen einer vorhandenen Web-App können Sie das Cmdlet **Remove-AzureRmWebApp** verwenden. Sie müssen den Namen der Web-App und der Ressourcengruppe angeben.

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a>Auflisten vorhandener Web-Apps
Verwenden Sie zum Auflisten vorhandener Web-Apps das Cmdlet **Get-AzureRmWebApp** .

Verwenden Sie zum Auflisten aller Web-Apps in Ihrem Abonnement Folgendes:

    Get-AzureRmWebApp

Verwenden Sie zum Auflisten aller Web-Apps in einer bestimmten Ressourcengruppe Folgendes:

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

Verwenden Sie zum Abrufen einer bestimmten Web-App Folgendes:

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a>Konfigurieren einer vorhandenen Web-App
Verwenden Sie zum Ändern der Einstellungen und Konfigurationen einer vorhandenen Web-App das Cmdlet **Set-AzureRmWebApp** . Klicken Sie auf den [Link zur Cmdlet-Referenz](https://msdn.microsoft.com/library/mt652487.aspx)

Beispiel 1: Verwenden Sie dieses Cmdlet zum Ändern von Verbindungszeichenfolgen.

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

Beispiel 2: Hinzufügen oder Ändern von App-Einstellungen

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


Beispiel 3: Festlegen der Web-App auf die Ausführung im 64-Bit-Modus

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-the-state-of-an-existing-web-app"></a>Ändern des Status einer vorhandenen Web-App
#### <a name="restart-a-web-app"></a>Neustarten einer Web-App
Um eine Web-App neu zu starten, müssen Sie den Namen und die Ressourcengruppe der Web-App angeben.

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Beenden einer Web-App
Um eine Web-App zu beenden, müssen Sie den Namen und die Ressourcengruppe der Web-App angeben.

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Starten einer Web-App
Um eine Web-App zu starten, müssen Sie den Namen und die Ressourcengruppe der Web-App angeben.

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Verwalten von Web-App-Veröffentlichungsprofilen
Jede Web-App hat ein Veröffentlichungsprofil, das verwendet werden kann, um Ihre Apps zu veröffentlichen. Eine Reihe von Vorgängen kann auf Veröffentlichungsprofile angewendet werden.

#### <a name="get-publishing-profile"></a>Abrufen des Veröffentlichungsprofils
Um das Veröffentlichungsprofil für eine Web-App abzurufen, verwenden Sie Folgendes:

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

Durch diesen Befehl wird das Veröffentlichungsprofil an der Befehlszeile wiedergegeben und in einer Textdatei ausgegeben.

#### <a name="reset-publishing-profile"></a>Zurücksetzen des Veröffentlichungsprofils
Um sowohl das Veröffentlichungskennwort für die FTP- und Webbereitstellung einer Web-App zurückzusetzen, verwenden Sie Folgendes:

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a>Verwalten von Web-App-Zertifikaten
Informationen zum Verwalten von Web-App-Zertifikaten finden Sie unter [SSL-Zertifikatbindung mithilfe von PowerShell](app-service-web-app-powershell-ssl-binding.md)

### <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zur PowerShell-Unterstützung von Azure Resource Manager finden Sie unter [Verwenden von Azure PowerShell mit Azure Resource Manager](../powershell-azure-resource-manager.md)
* Informationen zu App Service-Umgebungen finden Sie in der [Einführung in die App Service-Umgebung](app-service-app-service-environment-intro.md)
* Informationen zum Verwalten von App Service-SSL-Zertifikaten mit PowerShell finden Sie unter [SSL-Zertifikatbindung mithilfe von PowerShell](app-service-web-app-powershell-ssl-binding.md)
* Die vollständige Liste von Azure Resource Manager-basierten PowerShell-Cmdlets für Azure-Web-Apps finden Sie in der [Azure-Cmdlet-Referenz für die Azure Resource Manager-basierten PowerShell-Cmdlets für Web-Apps](https://msdn.microsoft.com/library/mt619237.aspx)
* * Informationen zum Verwalten von App Service über die Befehlszeilenschnittstelle finden Sie unter [Using Azure Resource Manager-Based XPlat-CLI für Azure-Web-App](app-service-web-app-azure-resource-manager-xplat-cli.md) (Verwenden der Azure Resource Manager-basierten XPlat-CLI für Azure-Web-App).


