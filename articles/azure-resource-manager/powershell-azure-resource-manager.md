---
title: Azure PowerShell mit Resource Manager | Microsoft Docs
description: "Einführung in das Verwenden von Azure PowerShell, um mehrere Ressourcen als Ressourcengruppe in Azure bereitzustellen."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: b33b7303-3330-4af8-8329-c80ac7e9bc7f
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 08/18/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 109ca4a4672d21969096af26a094390673de25d9
ms.openlocfilehash: 14419f36a9404202d6238d5825fb1ae77d46038a


---
# <a name="using-azure-powershell-with-azure-resource-manager"></a>Verwenden von Windows PowerShell mit dem Azure-Ressourcen-Manager
> [!div class="op_single_selector"]
> * [Portal](resource-group-portal.md)
> * [Azure-Befehlszeilenschnittstelle](xplat-cli-azure-resource-manager.md)
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [REST-API](resource-manager-rest-api.md)

Azure Resource Manager implementiert einen modernen Ansatz für die Steuerung des Azure-Ressourcenlebenszyklus. Anstatt einzelne Ressourcen zu erstellen und zu verwalten, können Sie sich eine Komplettlösung vorstellen, zum Beispiel einen Blog, eine Fotogalerie, ein SharePoint-Portal oder ein Wiki. Mithilfe eine Vorlage (einer deklarativen Darstellung der Lösung) können Sie eine Ressourcengruppe festlegen, die alle Ressourcen enthält, die Sie zum Unterstützen der Lösung brauchen. Anschließend wird diese Ressourcengruppe als logische Einheit bereitgestellt und verwaltet.

In diesem Tutorial erlernen Sie das Verwenden von Azure PowerShell mit dem Azure-Ressourcen-Manager. Sie werden durch die Bereitstellung einer Lösung und die Verwendung der Lösung geführt. Sie verwenden Azure PowerShell und eine Resource Manager-Vorlage, um Folgendes bereitzustellen:

* Instanz von SQL Server zum Hosten der Datenbank
* SQL-Datenbank zum Speichern der Daten
* Firewallregeln, um der Web-App das Verbinden mit der Datenbank zu ermöglichen
* App Service-Plan zum Bestimmen der Funktionen und Kosten der Web-App
* Website zum Ausführen der Web-App
* Webkonfiguration zum Speichern der Verbindungszeichenfolge mit der Datenbank
* Warnungsregeln zum Überwachen der Leistung und von Fehlern
* App Insights für die Einstellungen für die automatische Skalierung

Wie Sie Azure PowerShell erhalten, erfahren Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

## <a name="get-help-for-cmdlets"></a>Hilfe zu Cmdlets
Um detaillierte Hilfe zu einem Cmdlet aus dem Lernprogramm zu erhalten, verwenden Sie das Get-Help-Cmdlet.

    Get-Help <cmdlet-name> -Detailed

Geben Sie beispielsweise Folgendes ein, um Hilfe zum Cmdlet „Get-AzureRmResource“ zu erhalten:

    Get-Help Get-AzureRmResource -Detailed

Geben Sie Folgendes ein, um eine Liste der Cmdlets im Modul „Ressourcen“ mit einer Hilfezusammenfassung zu erhalten:

    Get-Command -Module AzureRM.Resources | Get-Help | Format-Table Name, Synopsis

Die Ausgabe sieht etwa wie der folgende Auszug aus:

    Name                                   Synopsis
    ----                                   --------
    Find-AzureRmResource                   Searches for resources using the specified parameters.
    Find-AzureRmResourceGroup              Searches for resource group using the specified parameters.
    Get-AzureRmADGroup                     Filters active directory groups.
    Get-AzureRmADGroupMember               Get a group members.
    ...

Um die vollständige Hilfe zu einem Cmdlet zu erhalten, geben Sie einen Befehl im folgenden Format ein:

    Get-Help <cmdlet-name> -Full

## <a name="login-to-your-azure-account"></a>Anmelden bei Ihrem Azure-Konto
Bevor Sie an Ihrer Lösung arbeiten, müssen Sie sich bei Ihrem Konto anmelden.

Um sich bei Ihrem Azure-Konto anzumelden, verwenden Sie das **Add-AzureRmAccount** -Cmdlet.

    Add-AzureRmAccount

Sie werden vom Cmdlet zur Eingabe der Anmeldeinformationen für Ihr Azure-Konto aufgefordert. Nach dem Anmelden werden Ihre Kontoeinstellungen heruntergeladen, damit sie Azure PowerShell zur Verfügung stehen.

Die Kontoeinstellungen laufen ab. Sie müssen sie also ab und zu aktualisieren. Um die Kontoeinstellungen zu aktualisieren, führen Sie **Add-AzureRmAccount** erneut aus.

> [!NOTE]
> Für die Resource Manager-Module ist „Add-AzureRmAccount“ erforderlich. Eine Datei mit Veröffentlichungseinstellungen ist nicht ausreichend.
>
>

Wenn Sie über mehrere Abonnements verfügen, geben Sie die Abonnement-ID, die Sie für die Bereitstellung verwenden möchten, mit dem Cmdlet **Set-AzureRmContext** an.

    Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Bevor Sie Ressourcen für Ihr Abonnement bereitstellen, müssen Sie eine Ressourcengruppe erstellen, die die Ressourcen enthält.

Um eine Ressourcengruppe zu erstellen, verwenden Sie das Cmdlet **New-AzureRmResourceGroup** .

Der Befehl verwendet den **Name**-Parameter, um einen Namen für die Ressourcengruppe anzugeben, und den **Location**-Parameter, um ihren Speicherort anzugeben. Basierend auf den Angaben im vorherigen Abschnitt nutzen wir als Standort „USA, Westen“.

    New-AzureRmResourceGroup -Name TestRG1 -Location "West US"

Die Ausgabe ähnelt der folgenden:

    ResourceGroupName : TestRG1
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1

Ihre Ressourcengruppe wurde erfolgreich erstellt.

## <a name="deploy-your-solution"></a>Bereitstellen der Lösung
In diesem Thema wird nicht gezeigt, wie Sie eine Vorlage erstellen, und nicht die Struktur der Vorlage erläutert. Diese Informationen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md) und [Resource Manager-Vorlage – Exemplarische Vorgehensweise](resource-manager-template-walkthrough.md). Sie stellen die vordefinierte Vorlage [Bereitstellen einer Web-App mit einer SQL-Datenbank](https://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) über [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/) bereit.

Sie haben Ihre Ressourcengruppe und Ihre Vorlage, weshalb Sie bereit sind, die in Ihrer Vorlage definierte Infrastruktur in der Ressourcengruppe bereitzustellen. Sie stellen Ressourcen mit dem Cmdlet **New-AzureRmResourceGroupDeployment** bereit. Mit der Vorlage werden viele Standardwerte angegeben, die wir später verwenden. Sie müssen also keine Werte für diese Parameter angeben. Die grundlegende Syntax sieht wie folgt aus:

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -administratorLogin exampleadmin -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json

Sie geben die Ressourcengruppe und den Speicherort der Vorlage an. Wenn Ihre Vorlage eine lokale Datei ist, verwenden Sie den Parameter **-TemplateFile** und geben den Pfad zur Vorlage an. Sie können den **-Mode**-Parameter entweder auf **Incremental** oder **Complete** festlegen. Standardmäßig führt der Resource Manager eine inkrementelle Aktualisierung während der Bereitstellung durch. Daher ist es nicht erforderlich, **-Mode** festzulegen, wenn Sie **Incremental** wünschen.
Informationen zu den Unterschieden bei diesen Bereitstellungsmodi finden Sie unter [Bereitstellen einer Anwendung mit einer Azure Resource Manager-Vorlage](resource-group-template-deploy.md).

### <a name="dynamic-template-parameters"></a>Dynamische Vorlagenparameter
Wenn Sie mit PowerShell vertraut sind, wissen Sie, dass Sie die für ein Cmdlet verfügbaren Parameter durchlaufen können, indem Sie ein Minuszeichen (-) eingeben und die TAB-TASTE drücken. Dieselbe Funktionalität gilt auch für Parameter, die Sie in der Vorlage definieren. Sobald Sie den Vorlagennamen eingeben, ruft das Cmdlet die Vorlage ab, analysiert sie und fügt die Vorlagenparameter dynamisch dem Befehl hinzu. Auf diese Weise wird das Angeben der Vorlagenparameterwerte stark vereinfacht.

Wenn Sie den Befehl eingeben, werden Sie zur Angabe des fehlenden obligatorischen **administratorLoginPassword**-Parameters aufgefordert. Wenn Sie das Kennwort eingeben, wird der Wert der sicheren Zeichenfolge ausgeblendet. Durch diese Strategie wird das Risiko der Angabe eines Kennworts in einfachem Text vermieden.

    cmdlet New-AzureRmResourceGroupDeployment at command pipeline position 1
    Supply values for the following parameters:
    (Type !? for Help.)
    administratorLoginPassword: ********

Wenn die Vorlage einen Parameter mit einem Namen enthält, der einem der Parameter im Befehl zum Bereitstellen der Vorlage entspricht (z.B. einen Parameter namens **ResourceGroupName** in der Vorlage, der mit dem Parameter **ResourceGroupName** im Cmdlet [New-AzureRmResourceGroupDeployment](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.2.0/new-azurermresourcegroupdeployment) identisch ist), werden Sie aufgefordert, einen Wert für einen Parameter mit dem Postfix **FromTemplate** anzugeben (z.B. **ResourceGroupNameFromTemplate**). Im Allgemeinen sollten Sie diese Verwirrung vermeiden, indem Sie Parametern nicht dieselben Namen wie Parametern für Bereitstellungsvorgänge geben.

Der Befehl wird ausgeführt und gibt Meldungen zurück, während die Ressourcen erstellt werden. Schließlich wird das Ergebnis der Bereitstellung angezeigt.

    DeploymentName    : azuredeploy
    ResourceGroupName : TestRG1
    ProvisioningState : Succeeded
    Timestamp         : 4/11/2016 7:26:11 PM
    Mode              : Incremental
    TemplateLink      :
                Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json
                ContentVersion : 1.0.0.0
    Parameters        :
                Name             Type                       Value
                ===============  =========================  ==========
                skuName          String                     F1
                skuCapacity      Int                        1
                administratorLogin  String                  exampleadmin
                administratorLoginPassword  SecureString
                databaseName     String                     sampledb
                collation        String                     SQL_Latin1_General_CP1_CI_AS
                edition          String                     Basic
                maxSizeBytes     String                     1073741824
                requestedServiceObjectiveName  String       Basic

    Outputs           :
                Name             Type                       Value
                ===============  =========================  ==========
                siteUri          String                     websites5wdai7p2k2g4.azurewebsites.net
                sqlSvrFqdn       String                     sqlservers5wdai7p2k2g4.database.windows.net

    DeploymentDebugLogLevel :

In wenigen Schritten haben Sie die Ressourcen, die für eine komplexe Website erforderlich sind, erstellt und bereitgestellt.

### <a name="log-debug-information"></a>Protokollieren von Debuginformationen
Beim Bereitstellen einer Vorlage können Sie zusätzliche Informationen zur Anforderung und Antwort protokollieren, indem Sie den Parameter **-DeploymentDebugLogLevel** beim Ausführen von **New-AzureRmResourceGroupDeployment** angeben. Diese Informationen sind für die Behandlung von Bereitstellungsfehlern hilfreich. Der Standardwert lautet **Keine**. Dies bedeutet, dass keine Inhalte von Anforderungen oder Antworten protokolliert werden. Sie können angeben, dass die Inhalte von Anforderungen, Antworten oder beidem protokolliert werden.  Weitere Informationen zur Problembehandlung für Bereitstellungen und Protokollierung von Debuginformationen finden Sie unter [Problembehandlung beim Bereitstellen von Ressourcengruppen mit Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md). Im folgenden Beispiel werden die Inhalte der Anforderung und der Antwort für die Bereitstellung protokolliert.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -DeploymentDebugLogLevel All -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json

> [!NOTE]
> Überlegen Sie sich beim Festlegen des Parameters DeploymentDebugLogLevel gut, welche Art von Informationen Sie während der Bereitstellung übergeben. Indem Sie Informationen über die Anforderung oder die Antwort protokollieren, machen Sie möglicherweise vertrauliche Daten verfügbar, die durch die Bereitstellungsvorgänge abgerufen werden.

## <a name="get-information-about-your-resource-groups"></a>Abrufen von Informationen zu Ihren Ressourcengruppen
Nach dem Erstellen einer Ressourcengruppe können Sie die Cmdlets im Ressourcen-Manager-Modul verwenden, um Ihre Ressourcengruppen zu verwalten.

* Um eine Ressourcengruppe in Ihrem Abonnement abzurufen, verwenden Sie das Cmdlet **Get-AzureRmResourceGroup** :

        Get-AzureRmResourceGroup -ResourceGroupName TestRG1

    Hiermit werden die folgenden Informationen zurückgegeben:

        ResourceGroupName : TestRG1
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG

        ...

    Wenn Sie keinen Ressourcengruppennamen angeben, gibt das Cmdlet alle Ressourcengruppen im Abonnement zurück.
* Um die Ressourcen in der Ressourcengruppe abzurufen, verwenden Sie das Cmdlet **Find-AzureRmResource** und den zugehörigen **ResourceGroupNameContains**-Parameter. Ohne Parameter ruft „Find-AzureRmResource“ alle Ressourcen in Ihrem Azure-Abonnement ab.

        Find-AzureRmResource -ResourceGroupNameContains TestRG1

     Es wird eine Liste mit Ressourcen zurückgegeben, die wie folgt formatiert sind:

        Name              : sqlservers5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Sql/servers/sqlservers5wdai7p2k2g4
        ResourceName      : sqlservers5wdai7p2k2g4
        ResourceType      : Microsoft.Sql/servers
        Kind              : v2.0
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
        Tags              : {System.Collections.Hashtable}
        ...
* Sie können Tags verwenden, um die Ressourcen in Ihrem Abonnement logisch zu organisieren, und Ressourcen mit den Cmdlets **Find-AzureRmResource** und **Find-AzureRmResourceGroup** abrufen.

        Find-AzureRmResource -TagName displayName -TagValue Website

        Name              : webSites5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Web/sites/webSites5wdai7p2k2g4
        ResourceName      : webSites5wdai7p2k2g4
        ResourceType      : Microsoft.Web/sites
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}

      There is much more you can do with tags. For more information, see [Using tags to organize your Azure resources](resource-group-using-tags.md).

## <a name="add-to-a-resource-group"></a>Hinzufügen zu einer Ressourcengruppe
Um der Ressourcengruppe eine Ressource hinzuzufügen, verwenden Sie das Cmdlet **New-AzureRmResource** . Das Hinzufügen einer Ressource auf diese Weise kann jedoch künftig für Verwirrung sorgen, da die neue Ressource nicht in Ihrer Vorlage vorhanden ist. Wenn Sie die alte Vorlage erneut bereitgestellt hätten, würden Sie eine unvollständige Lösung bereitstellen. Wenn Sie häufig bereitstellen, ist es einfacher und zuverlässiger, die neue Ressource Ihrer Vorlage hinzuzufügen und Sie erneut bereitzustellen.

## <a name="move-a-resource"></a>Verschieben einer Ressource
Sie können vorhandene Ressourcen in eine neue Ressourcengruppe verschieben. Beispiele finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](resource-group-move-resources.md).

## <a name="export-template"></a>Exportieren der Vorlage
Für eine vorhandene Ressourcengruppe (die über PowerShell oder eine andere Methode, z.B. das Portal, bereitgestellt wird), können Sie die Resource Manager-Vorlage für die Ressourcengruppe anzeigen. Das Exportieren der Vorlage hat zwei Vorteile:

1. Sie können zukünftige Bereitstellungen der Lösung leicht automatisieren, da die gesamte Infrastruktur in der Vorlage definiert ist.
2. Sie können sich mit der Vorlagensyntax vertraut machen, indem Sie sich die JavaScript Object Notation (JSON) zu Ihrer Lösung ansehen.

Über PowerShell können Sie entweder eine Vorlage generieren, die den aktuellen Status Ihrer Ressourcengruppe darstellt, oder die Vorlage abrufen, die für eine bestimmte Bereitstellung verwendet wurde.

Das Exportieren der Vorlage für eine Ressourcengruppe ist hilfreich, wenn Sie Änderungen an einer Ressourcengruppe vorgenommen haben und die JSON-Darstellung ihres aktuellen Zustands abrufen müssen. Die generierte Vorlage enthält aber nur eine minimale Anzahl von Parametern und keine Variablen. Die meisten Werte in der Vorlage sind hartcodiert. Vor dem Bereitstellen der generierten Vorlage kann es ratsam sein, weitere Werte in Parameter zu konvertieren, damit Sie die Bereitstellung für unterschiedliche Umgebungen anpassen können.

Das Exportieren der Vorlage für eine bestimmte Bereitstellung ist hilfreich, wenn Sie die eigentliche Vorlage anzeigen möchten, die zum Bereitstellen von Ressourcen verwendet wurde. Die Vorlage enthält alle Parameter und Variablen, die für die ursprüngliche Bereitstellung definiert wurden. Wenn eine Person in Ihrer Organisation aber Änderungen an der Ressourcengruppe vorgenommen hat, die über die Definition in der Vorlage hinausgehen, stellt diese Vorlage nicht den aktuellen Zustand der Ressourcengruppe dar.

> [!NOTE]
> Die Funktion zum Exportieren von Vorlagen befindet sich in der Vorschau, und das Exportieren einer Vorlage wird derzeit nicht für alle Ressourcentypen unterstützt. Wenn Sie versuchen, eine Vorlage zu exportieren, wird ggf. ein Fehler mit dem Hinweis angezeigt, dass einige Ressourcen nicht exportiert wurden. Bei Bedarf können Sie diese Ressourcen nach dem Herunterladen manuell in der Vorlage definieren.


### <a name="export-template-from-resource-group"></a>Exportieren einer Vorlage aus der Ressourcengruppe
Führen Sie zum Anzeigen der Vorlage für eine Ressourcengruppe das Cmdlet **Export-AzureRmResourceGroup** aus.

    Export-AzureRmResourceGroup -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\TestRG1.json

### <a name="download-template-from-deployment"></a>Herunterladen der Vorlage aus einer Bereitstellung
Führen Sie zum Herunterladen der Vorlage, die für eine bestimmte Bereitstellung verwendet wird, das Cmdlet **Save-AzureRmResourceGroupDeploymentTemplate** aus.

    Save-AzureRmResourceGroupDeploymentTemplate -DeploymentName azuredeploy -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\azuredeploy.json

## <a name="delete-resources-or-resource-group"></a>Löschen von Ressourcen oder Ressourcengruppen
* Um eine Ressource aus der Ressourcengruppe zu löschen, verwenden Sie das Cmdlet **Remove-AzureRmResource** . Dieses Cmdlet löscht die Ressource, aber nicht die Ressourcengruppe.

    Dieser Befehl entfernt die Website „TestSite“ aus der Ressourcengruppe „TestRG1“.

        Remove-AzureRmResource -Name TestSite -ResourceGroupName TestRG1 -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01
* Um eine Ressourcengruppe zu löschen, verwenden Sie das Cmdlet **Remove-AzureRmResourceGroup** . Dieses Cmdlet löscht die Ressourcengruppe und ihre Ressourcen.

        Remove-AzureRmResourceGroup -Name TestRG1

    Sie werden aufgefordert, den Löschvorgang zu bestätigen.

        Confirm
        Are you sure you want to remove resource group 'TestRG1'
        [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y

## <a name="deployment-script"></a>Bereitstellungsskript
In den oben stehenden Bereitstellungsbeispielen in diesem Thema wurden nur die individuellen Cmdlets gezeigt, die zum Bereitstellen der Ressourcen für Azure benötigt werden. Im folgenden Beispiel wird ein Bereitstellungsskript gezeigt, mit dem die Ressourcengruppe erstellt wird und die Ressourcen bereitgestellt werden.

    <#
      .SYNOPSIS
      Deploys a template to Azure

      .DESCRIPTION
      Deploys an Azure Resource Manager template

      .PARAMETER subscriptionId
      The subscription id where the template will be deployed.

      .PARAMETER resourceGroupName
      The resource group where the template will be deployed. Can be the name of an existing or a new resource group.

      .PARAMETER resourceGroupLocation
      Optional, a resource group location. If specified, will try to create a new resource group in this location. If not specified, assumes resource group is existing.

      .PARAMETER deploymentName
      The deployment name.

      .PARAMETER templateFilePath
      Optional, path to the template file. Defaults to template.json.

      .PARAMETER parametersFilePath
      Optional, path to the parameters file. Defaults to parameters.json. If file is not found, will prompt for parameter values based on template.
    #>

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
      $templateFilePath = "template.json",

      [string]
      $parametersFilePath = "parameters.json"
    )

    #******************************************************************************
    # Script body
    # Execution begins here
    #******************************************************************************
    $ErrorActionPreference = "Stop"

    # sign in
    Write-Host "Logging in...";
    Add-AzureRmAccount;

    # select subscription
    Write-Host "Selecting subscription '$subscriptionId'";
    Set-AzureRmContext -SubscriptionID $subscriptionId;

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

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Erstellen von Resource Manager-Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Informationen zum Bereitstellen von Vorlagen finden Sie unter [Bereitstellen einer Anwendung mit einer Azure Resource Manager-Vorlage](resource-group-template-deploy.md).
* Ein ausführliches Beispiel für das Bereitstellen eines Projekts finden Sie unter [Vorhersagbares Bereitstellen von Microservices in Azure](../app-service-web/app-service-deploy-complex-application-predictably.md).
* Weitere Informationen zur Problembehandlung bei einer nicht erfolgreichen Bereitstellung finden Sie unter [Problembehandlung beim Bereitstellen von Ressourcengruppen in Azure](resource-manager-troubleshoot-deployments-powershell.md).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).




<!--HONumber=Dec16_HO2-->


