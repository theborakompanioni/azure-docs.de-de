---
title: Verwenden von PowerShell zum Verwalten von Azure Event Hubs-Ressourcen | Microsoft-Dokumentation
description: Verwenden des PowerShell-Moduls zum Erstellen und Verwalten von Event Hubs
services: event-hubs
documentationcenter: .NET
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: 2b49c01153b1104612e6ebf9c88566fc40d1f635
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---
# <a name="use-powershell-to-manage-event-hubs-resources"></a>Verwenden von PowerShell zum Verwalten von Event Hubs-Ressourcen

Microsoft Azure PowerShell ist eine Skriptumgebung, mit der Sie die Bereitstellung und Verwaltung von Azure-Diensten steuern und automatisieren können. In diesem Artikel wird beschrieben, wie Sie mithilfe des [Resource Manager-PowerShell-Moduls für Event Hubs](/powershell/module/azurerm.eventhub) Event Hubs-Entitäten (Namespaces, einzelne Event Hubs und Consumergruppen) über eine lokale Azure PowerShell-Konsole oder ein lokales Azure PowerShell-Skript bereitstellen und verwalten.

Sie können zudem Event Hubs-Ressourcen mithilfe von Azure Resource Manager-Vorlagen verwalten. Weitere Informationen finden Sie im Artikel [Erstellen eines Event Hubs-Namespace mit Event Hub und einer Consumergruppe mithilfe einer Azure Resource Manager-Vorlage](event-hubs-resource-manager-namespace-event-hub.md).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, benötigen Sie Folgendes:

* Ein Azure-Abonnement. Weitere Informationen zum Erwerb eines Abonnements finden Sie unter [Azure erwerben][purchase options], [Spezielle Angebote][member offers] oder [Erstellen Sie noch heute Ihr kostenloses Azure-Konto][free account].
* Einen Computer mit Azure PowerShell. Anweisungen finden Sie unter [Erste Schritte mit Azure PowerShell-Cmdlets](/powershell/azure/get-started-azureps).
* Allgemeine Kenntnisse über PowerShell-Skripts, NuGet-Pakete und .NET Framework.

## <a name="get-started"></a>Erste Schritte

Im ersten Schritt verwenden Sie PowerShell zum Anmelden bei Ihrem Azure-Konto und Ihrem Azure-Abonnement. Befolgen Sie die Anweisungen unter [Erste Schritte mit Azure PowerShell-Cmdlets](/powershell/azure/get-started-azureps), um sich bei Ihrem Azure-Konto anzumelden und dann die Ressourcen in Ihrem Azure-Abonnement abzurufen und auf diese zuzugreifen.

## <a name="provision-an-event-hubs-namespace"></a>Bereitstellen eines Event Hubs-Namespace

Beim Arbeiten mit Event Hubs-Namespaces können Sie die Cmdlets [Get-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/get-azurermeventhubnamespace), [New-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/new-azurermeventhubnamespace), [Remove-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/remove-azurermeventhubnamespace) und [Set-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/set-azurermeventhubnamespace) verwenden.

In diesem Beispiel werden einige lokale Variablen im Skript erstellt: `$Namespace` und `$Location`.

* `$Namespace` ist der Name des Event Hubs-Namespaces, mit dem gearbeitet werden soll.
* `$Location` identifiziert das Rechenzentrum, in dem Sie den Namespace bereitstellen.
* `$CurrentNamespace` speichert den Referenznamespace, den Sie abrufen (oder erstellen).

In einem tatsächlichen Skript können `$Namespace` und `$Location` als Parameter übergeben werden.

Dieser Teil des Skripts führt Folgendes aus:

1. Es wird versucht, einen Event Hubs-Namespace mit dem angegebenen Namen zu abzurufen.
2. Wenn der Namespace gefunden wird, erfolgt eine Meldung, was gefunden wurde.
3. Wenn der Namespace nicht gefunden wird, wird der Namespace erstellt und anschließend der neu erstellte Namespace abgerufen.

    ```powershell
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
   
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Host "The namespace $Namespace already exists in the $Location region:"
        # Report what was found
        Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "The $Namespace namespace does not exist."
        Write-Host "Creating the $Namespace namespace in the $Location region..."
        New-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
        Write-Host "The $Namespace namespace in Resource Group $ResGrpName in the $Location region has been successfully created."
    }
    ```

## <a name="create-an-event-hub"></a>Erstellen eines Ereignis-Hubs

Führen Sie zum Erstellen eines Event Hubs mit dem Skript aus dem vorherigen Abschnitt eine Namespaceüberprüfung durch. Erstellen Sie anschließend mit dem Cmdlet [New-AzureRmEventHub](/powershell/module/azurerm.eventhub/new-azurermeventhub) den Event Hub:

```powershell
# Check if event hub already exists
$CurrentEH = Get-AzureRMEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName

if($CurrentEH)
{
    Write-Host "The event hub $EventHubName already exists in the $Location region:"
    # Report what was found
    Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "The $EventHubName event hub does not exist."
    Write-Host "Creating the $EventHubName event hub in the $Location region..."
    New-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -Location $Location -MessageRetentionInDays 3
    $CurrentEH = Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "The $EventHubName event hub in Resource Group $ResGrpName in the $Location region has been successfully created."
}
```

### <a name="create-a-consumer-group"></a>Erstellen einer Consumergruppe

Führen Sie die Namespace- und Event Hub-Prüfungen mithilfe der Skripts im vorherigen Abschnitt durch, um eine Consumergruppe in einem Event Hub zu erstellen. Erstellen Sie die Consumergruppe anschließend mit dem Cmdlet [New-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/new-azurermeventhubconsumergroup) im Event Hub. Beispiel:

```powershell
# Check if consumer group already exists
$CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -ErrorAction Ignore

if($CurrentCG)
{
    Write-Host "The consumer group $ConsumerGroupName in event hub $EventHubName already exists in the $Location region:"
    # Report what was found
    Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "The $ConsumerGroupName consumer group does not exist."
    Write-Host "Creating the $ConsumerGroupName consumer group in the $Location region..."
    New-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
    $CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "The $ConsumerGroupName consumer group in event hub $EventHubName in Resource Group $ResGrpName in the $Location region has been successfully created."
}
```

#### <a name="set-user-metadata"></a>Festlegen von Benutzer-Metadaten

Nach dem Ausführen der Skripts aus dem vorherigen Abschnitt können Sie mit dem Cmdlet [Set-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/set-azurermeventhubconsumergroup) die Eigenschaften einer Consumergruppe aktualisieren, wie im folgenden Beispiel veranschaulicht:

```powershell
# Set some user metadata on the CG
Write-Host "Setting the UserMetadata field to 'Testing'"
Set-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -UserMetadata "Testing"
# Show result
Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
```

## <a name="remove-event-hub"></a>Entfernen von Event Hubs

Zum Entfernen der erstellten Event Hubs können Sie die Cmdlets `Remove-*` verwenden, wie im folgenden Beispiel veranschaulicht:

```powershell
# Clean up
Remove-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
Remove-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
Remove-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
```

## <a name="next-steps"></a>Nächste Schritte

- Die vollständige Dokumentation zum Resource Manager-PowerShell-Modul für Event Hubs finden Sie [hier](/powershell/module/azurerm.eventhub). Auf dieser Seite sind alle verfügbaren Cmdlets aufgeführt.
- Informationen zum Verwenden von Azure Resource Manager-Vorlagen finden Sie im Artikel [Erstellen eines Event Hubs-Namespace mit Event Hub und einer Consumergruppe mithilfe einer Azure Resource Manager-Vorlage](event-hubs-resource-manager-namespace-event-hub.md).
- Informationen zu [Event Hubs .NET-Verwaltungsbibliotheken](event-hubs-management-libraries.md).

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/

