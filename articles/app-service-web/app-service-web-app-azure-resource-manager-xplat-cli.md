---
title: "Azure Resource Manager-basierte plattformübergreifende Befehlszeilentools für die Azure-Web-App | Microsoft Docs"
description: "Es wird beschrieben, wie Sie die neuen Azure Resource Manager-basierten plattformübergreifenden Befehlszeilentools zum Verwalten Ihrer Azure-Web-Apps verwenden."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: d415b195-4262-416f-b59f-7e1aef200054
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2016
ms.author: aelnably
translationtype: Human Translation
ms.sourcegitcommit: 5ea7095e12b6194556d3cd0baa43ccfed1e087ee
ms.openlocfilehash: 7a03e1417617453c43edcc3787da10d171359757
ms.lasthandoff: 02/27/2017


---
# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-app-service"></a>Verwenden der Azure Resource Manager-basierten XPlat-CLI für Azure App Service
> [!div class="op_single_selector"]
> * [Azure-Befehlszeilenschnittstelle](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

Version 0.10.5 der plattformübergreifenden Microsoft Azure-Befehlszeilentools wurden neue Befehle hinzugefügt. Mit diesen Befehlen können Benutzer Azure Resource Manager-basierte PowerShell-Befehle zum Verwalten von App Service verwenden.

Informationen zum Verwalten von Ressourcengruppen finden Sie unter [Verwalten von Azure-Ressourcen und -Ressourcengruppen mithilfe der Azure-Befehlszeilenschnittstelle](../azure-resource-manager/xplat-cli-azure-resource-manager.md). 

> [!NOTE] 
> Probieren Sie auch [Azure CLI 2.0](https://github.com/Azure/azure-cli) aus, eine Befehlszeilenschnittstelle (CLI) der nächsten Generation, die in Python für das Resource Manager-Bereitstellungsmodell geschrieben wurde.
>
>

## <a name="managing-app-service-plans"></a>Verwalten von App Service-Plänen
### <a name="create-an-app-service-plan"></a>Erstellen eines App Service-Plans
Verwenden Sie zum Erstellen eines App Service-Plans den Befehl **azure appserviceplan create**.

Es folgen Beschreibungen der verschiedenen Parameter:

* **--resource-group**: Ressourcengruppe, die den neu erstellten App Service-Plan enthält.
* **--name**: Name des App Service-Plans.
* **--location**: Standort des App Service-Plans.
* **--sku**: Gewünschte Preis-SKU (Optionen: F1 (Free). D1 (Shared). B1 (Basic Small), B2 (Basic Medium) und B3 (Basic Large). S1 (Standard Small), S2 (Standard Medium) und S3 (Standard Large). P1 (Premium Small), P2 (Premium Medium) und P3 (Premium Large).)
* **--instances**: Anzahl von Workern im App Service-Plan (Standardwert: 1).

Beispiel für die Verwendung dieses Cmdlets:

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

#### <a name="create-a-linux-app-service-plan"></a>Erstellen eines Linux-App Service-Plans

Verwenden Sie denselben Befehl **azure appserviceplan create** mit dem zusätzlichen Parameter **--islinux true**. Beachten Sie die Einschränkungen und Regionen, die unter [Introduction to App Service on Linux](app-service-linux-intro.md) (Einführung in App Service unter Linux) angegeben sind.

### <a name="list-existing-app-service-plans"></a>Auflisten vorhandener App Service-Pläne
Verwenden Sie zum Auflisten der vorhandenen App Service-Pläne den Befehl **azure appserviceplan list**.

Verwenden Sie zum Auflisten aller App Service-Pläne in einer bestimmten Ressourcengruppe Folgendes:

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

Verwenden Sie zum Abrufen eines bestimmten App Service-Plans den Befehl **azure appserviceplan show**:

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a>Konfigurieren eines vorhandenen App Service-Plans
Verwenden Sie zum Ändern der Einstellungen eines vorhandenen App Service-Plans den Befehl **azure appserviceplan config**. Sie können die SKU und die Anzahl von Workern ändern. 

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a>Skalieren eines App Service-Plans
Zum Skalieren eines vorhandenen App Service-Plans verwenden Sie Folgendes:

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-the-sku-of-an-app-service-plan"></a>Ändern der SKU eines App Service-Plans
Verwenden Sie zum Ändern der SKU eines vorhandenen App Service-Plans Folgendes:

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a>Löschen eines vorhandenen App Service-Plans
Zum Löschen eines vorhandenen App Service-Plans müssen alle zugewiesenen Apps zuerst verschoben oder gelöscht werden. Anschließend können Sie mit dem Befehl **azure webapp delete** den App Service-Plan löschen.

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-apps"></a>Verwalten von App Service-Apps
### <a name="create-a-web-app"></a>Erstellen einer Web-App
Verwenden Sie zum Erstellen einer Web-App den Befehl **azure webapp create**.

Es folgen Beschreibungen der verschiedenen Parameter:

* **--name**: Name der Web-App.
* **--plan**: Name des Service-Plans, der zum Hosten der Web-App verwendet wird.
* **--resource-group**: Ressourcengruppe, die den App Service-Plan hostet.
* **--location**: Standort der Web-App.

Beispiel für die Verwendung dieses Cmdlets:

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-app"></a>Löschen einer vorhandenen App
Zum Löschen einer vorhandenen App können Sie den Befehl **azure webapp delete** verwenden. Sie müssen den Namen der App und den Namen der Ressourcengruppe angeben.

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-apps"></a>Auflisten von vorhandenen Apps
Verwenden Sie zum Auflisten der Apps den Befehl **azure webapp list**.

Verwenden Sie Folgendes, um alle Apps einer bestimmten Ressourcengruppe aufzulisten:

    azure webapp list --resource-group ContosoAzureResourceGroup

Verwenden Sie zum Abrufen einer bestimmten App den Befehl **azure webapp show**.

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-app"></a>Konfigurieren einer vorhandenen App
Verwenden Sie zum Ändern der Einstellungen und Konfigurationen einer vorhandenen App den Befehl **azure webapp config set**.

Beispiel&1;: Ändern der PHP-Version einer App 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

Beispiel&2;: Hinzufügen oder Ändern von App-Einstellungen

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

Verwenden Sie den Befehl **azure webapp config set -h**, um zu ermitteln, welche anderen Konfigurationselemente geändert werden können.

### <a name="change-the-state-of-an-existing-app"></a>Ändern des Status einer vorhandenen App
#### <a name="restart-an-app"></a>Neustarten einer App
Um eine App neu zu starten, müssen Sie den Namen und die Ressourcengruppe der App angeben.

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-an-app"></a>Beenden einer App
Um eine App zu beenden, müssen Sie den Namen und die Ressourcengruppe der App angeben.

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-an-app"></a>Starten einer App
Um eine App zu starten, müssen Sie den Namen und die Ressourcengruppe der App angeben.

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-publishing-profiles-of-an-app"></a>Verwalten von Veröffentlichungsprofilen einer App
Jede App verfügt über ein Veröffentlichungsprofil, das Sie zum Veröffentlichen Ihres Codes verwenden können.

#### <a name="get-publishing-profile"></a>Abrufen des Veröffentlichungsprofils
Verwenden Sie Folgendes, um das Veröffentlichungsprofil für eine App abzurufen:

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

Mit diesem Befehl werden der Benutzername und das Kennwort für das Veröffentlichungsprofil in der Befehlszeile angegeben.

### <a name="manage-app-hostnames"></a>Verwalten von App-Hostnamen
Verwenden Sie zum Verwalten von Hostnamenbindungen für Ihre App den Befehl **azure webapp config hostnames**.  

#### <a name="list-hostname-bindings"></a>Auflisten von Hostnamenbindungen
Verwenden Sie zum Abrufen der aktuellen Hostnamenbindungen für eine App Folgendes:

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a>Hinzufügen von Hostnamenbindungen
Verwenden Sie zum Hinzufügen von Hostnamenbindungen zu einer App Folgendes:

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a>Löschen von Hostnamenbindungen
Verwenden Sie zum Löschen von Hostnamenbindungen Folgendes:

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

## <a name="next-steps"></a>Nächste Schritte
* Informationen zur Unterstützung der Azure Resource Manager-CLI finden Sie unter [Verwalten von Azure-Ressourcen und -Ressourcengruppen mithilfe der Azure-Befehlszeilenschnittstelle](../azure-resource-manager/xplat-cli-azure-resource-manager.md).
* Informationen zur Verwaltung von App Service per PowerShell finden Sie unter [Verwenden von auf Azure Resource Manager basierenden PowerShell-Befehlen zum Verwalten von Azure-Web-Apps](app-service-web-app-azure-resource-manager-powershell.md).
* Weitere Informationen zu Azure App Service unter Linux finden Sie unter [Introduction to App Service on Linux](app-service-linux-intro.md) (Einführung in App Service unter Linux).

