---
title: Verwalten von Azure Search mit PowerShell-Skripts | Microsoft Docs
description: "Verwalten Sie den Azure Search-Dienst mit PowerShell-Skripts. Erstellen oder Aktualisieren eines Azure Search-Diensts und Verwalten von Azure Search-Administratorschlüsseln"
services: search
documentationcenter: 
author: seansaleh
manager: mblythe
editor: 
tags: azure-resource-manager
ms.assetid: 9b3dc1f2-3619-4235-ba1f-d2d6f5c45dd5
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: powershell
ms.date: 08/15/2016
ms.author: seasa
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: a635b214fcc9b37ac426df227420b643e2a02cc7
ms.lasthandoff: 04/27/2017


---
# <a name="manage-your-azure-search-service-with-powershell"></a>Verwalten des Azure Search-Diensts mit PowerShell
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> 
> 

In diesem Thema werden die PowerShell-Befehle zur Durchführung vieler Verwaltungsaufgaben für Azure Search-Dienste beschrieben. Wir führen Sie schrittweise durch das Erstellen und Skalieren eines Search-Diensts und die Verwaltung der zugehörigen API-Schlüssel.
Diese Befehle stehen parallel zu den Verwaltungsoptionen der [Azure Search Management REST API](http://msdn.microsoft.com/library/dn832684.aspx)zur Verfügung.

## <a name="prerequisites"></a>Voraussetzungen
* Azure PowerShell 1.0 oder höher muss installiert sein. Anweisungen hierzu finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).
* Sie müssen in PowerShell wie im Folgenden beschrieben bei Ihrem Azure-Abonnement angemeldet sein.

Zunächst müssen Sie sich mit dem folgenden Befehl in Azure anmelden:

    Login-AzureRmAccount

Geben Sie die E-Mail-Adresse Ihres Azure-Kontos und das Kennwort in das Anmeldedialogfeld von Microsoft Azure ein.

Als Alternative steht die [Nicht interaktive Anmeldung mit einem Dienstprinzipal](../azure-resource-manager/resource-group-authenticate-service-principal.md)zur Verfügung.

Wenn Sie über mehrere Azure-Abonnements verfügen, müssen Sie Ihr Azure-Abonnement festlegen. Führen Sie den folgenden Befehl aus, um eine Liste Ihrer aktuellen Abonnements anzuzeigen.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Führen Sie den folgenden Befehl aus, um das Abonnement anzugeben. Im folgenden Beispiel ist der Name des Abonnements `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

## <a name="commands-to-help-you-get-started"></a>Befehle als Starthilfe
    $serviceName = "your-service-name-lowercase-with-dashes"
    $sku = "free" # or "basic" or "standard" for paid services
    $location = "West US"
    # You can get a list of potential locations with
    # (Get-AzureRmResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Search'}).Locations
    $resourceGroupName = "YourResourceGroup" 
    # If you don't already have this resource group, you can create it with 
    # New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Register the ARM provider idempotently. This must be done once per subscription
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Search" -Force

    # Create a new search service
    # This command will return once the service is fully created
    New-AzureRmResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateUri "https://gallery.azure.com/artifact/20151001/Microsoft.Search.1.0.9/DeploymentTemplates/searchServiceDefaultTemplate.json" `
        -NameFromTemplate $serviceName `
        -Sku $sku `
        -Location $location `
        -PartitionCount 1 `
        -ReplicaCount 1

    # Get information about your new service and store it in $resource
    $resource = Get-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19

    # View your resource
    $resource

    # Get the primary admin API key
    $primaryKey = (Invoke-AzureRmResourceAction `
        -Action listAdminKeys `
        -ResourceId $resource.ResourceId `
        -ApiVersion 2015-08-19).PrimaryKey

    # Regenerate the secondary admin API Key
    $secondaryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/regenerateAdminKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action secondary).SecondaryKey

    # Create a query key for read only access to your indexes
    $queryKeyDescription = "query-key-created-from-powershell"
    $queryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/createQueryKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action $queryKeyDescription).Key

    # View your query key
    $queryKey

    # Delete query key
    Remove-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices/deleteQueryKey/$($queryKey)" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19

    # Scale your service up
    # Note that this will only work if you made a non "free" service
    # This command will not return until the operation is finished
    # It can take 15 minutes or more to provision the additional resources
    $resource.Properties.ReplicaCount = 2
    $resource | Set-AzureRmResource

    # Delete your service
    # Deleting your service will delete all indexes and data in the service
    $resource | Remove-AzureRmResource

## <a name="next-steps"></a>Nächste Schritte
Nachdem der Dienst erstellt wurde, können Sie die nächsten Schritte ausführen: Erstellen eines [Index](search-what-is-an-index.md), [Abfragen eines Index](search-query-overview.md) und schließlich Erstellen und Verwalten Ihrer eigenen Suchanwendung, die Azure Search verwendet.

* [Erstellen eines Azure Search-Index im Azure-Portal](search-create-index-portal.md)
* [Abfragen eines Azure Search-Index per Suchexplorer im Azure-Portal](search-explorer.md)
* [Einrichten eines Indexers zum Laden von Daten anderer Dienste](search-indexer-overview.md)
* [Verwenden von Azure Search aus einer .NET-Anwendung](search-howto-dotnet-sdk.md)
* [Analysieren Ihres Azure Search-Datenverkehrs](search-traffic-analytics.md)


