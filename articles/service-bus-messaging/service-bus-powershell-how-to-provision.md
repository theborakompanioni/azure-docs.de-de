---
title: Verwalten von Azure Service Bus mit PowerShell | Microsoft Docs
description: Verwalten von Service Bus mit PowerShell-Skripts
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: aff8e2ce-bc8b-489f-aca9-a18782be0375
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2017
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 61f31c8ad0463776937f366d145595f04cc42d2e
ms.openlocfilehash: 24dd8757942488aa8364cc6cf968cfc17299699f


---
# <a name="manage-service-bus-with-powershell"></a>Verwalten von Service Bus mit PowerShell
## <a name="overview"></a>Übersicht
Microsoft Azure PowerShell ist eine Skriptumgebung, mit der Sie die Bereitstellung und Verwaltung Ihrer Workloads in Azure steuern und automatisieren können. In diesem Artikel wird beschrieben, wie Sie mithilfe von PowerShell Service Bus-Entitäten wie Namespaces, Warteschlangen und Event Hubs über eine lokale Azure PowerShell-Konsole bereitstellen und verwalten.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit den Anleitungen in diesem Artikel beginnen, müssen folgende Voraussetzungen erfüllt sein:

* Ein Azure-Abonnement. Azure ist eine abonnementbasierte Plattform. Weitere Informationen zum Erwerb eines Abonnements finden Sie unter [Kaufoptionen][Purchase Options], [Spezielle Angebote][Member Offers] oder [Kostenlose Testversion][Free Trial].
* Einen Computer mit Azure PowerShell. Anweisungen hierzu finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][Install and configure Azure PowerShell].
* Allgemeine Kenntnisse über PowerShell-Skripts, NuGet-Pakete und .NET Framework.

## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Einschließen eines Verweises auf die .NET-Assembly für Service Bus
Es gibt eine begrenzte Anzahl von PowerShell-Cmdlets zur Verwaltung von Service Bus. Für die Bereitstellung von Entitäten, die nicht über die vorhandenen Cmdlets verfügbar gemacht werden, können Sie den .NET-Client für Service Bus im [Service Bus-NuGet-Paket][Service Bus NuGet package] verwenden.

Sie müssen zunächst sicherstellen, dass Ihr Skript die Assembly **Microsoft.ServiceBus.dll** findet, die zusammen mit dem NuGet-Paket installiert wird. Aus Gründen der Flexibilität führt das Skript folgende Schritte aus:

1. Es bestimmt den Pfad, in dem es aufgerufen wurde.
2. Es durchläuft den Pfad, bis es einen Ordner mit dem Namen `packages`gefunden hat. Dieser Ordner wird erstellt, wenn Sie NuGet-Pakete installieren.
3. Es durchsucht den Ordner `packages` rekursiv nach einer Assembly mit der Bezeichnung **Microsoft.ServiceBus.dll**.
4. Es verweist auf die Assembly, sodass die Typen zur späteren Verwendung zur Verfügung stehen.

Diese Schritte werden in einem PowerShell-Skript folgendermaßen implementiert:

```powershell
try
{
    # WARNING: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Host "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Host "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error "Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script."
}
```

## <a name="provision-a-service-bus-namespace"></a>Bereitstellen eines Service Bus-Namespace
Zwei PowerShell-Cmdlets unterstützen Service Bus-Namespacevorgänge. Anstelle der .NET SDK-APIs können Sie [Get-AzureSBNamespace][Get-AzureSBNamespace] und [New-AzureSBNamespace][New-AzureSBNamespace] verwenden.

In diesem Beispiel werden einige lokale Variablen im Skript erstellt: `$Namespace` und `$Location`.

* `$Namespace` ist der Name des Service Bus-Namespace, mit dem Sie arbeiten möchten.
* `$Location` identifiziert das Rechenzentrum, in dem das Skript den Namespace bereitstellt.
* `$CurrentNamespace` speichert den Referenznamespace, den das Skript abruft (oder erstellt).

In einem tatsächlichen Skript können `$Namespace` und `$Location` als Parameter übergeben werden.

Dieser Teil des Skripts führt Folgendes aus:

1. Es wird versucht, einen Service Bus-Namespace mit dem angegebenen Namen zu finden.
2. Wenn der Namespace gefunden wird, erfolgt eine Meldung, was gefunden wurde.
3. Wenn der Namespace nicht gefunden wird, wird der Namespace erstellt und anschließend der neu erstellte Namespace abgerufen.
   
    ```powershell
    $Namespace = "MyServiceBusNS"
    $Location = "West US"
   
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
   
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Host "The namespace [$Namespace] already exists in the [$($CurrentNamespace.Region)] region."
    }
    else
    {
        Write-Host "The [$Namespace] namespace does not exist."
        Write-Host "Creating the [$Namespace] namespace in the [$Location] region..."
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace $false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```

Um weitere Service Bus-Entitäten bereitstellen zu können, erstellen Sie eine Instanz der [NamespaceManager][NamespaceManager]-Klasse aus dem SDK.
Sie können mithilfe des Cmdlets [Get-AzureSBAuthorizationRule][Get-AzureSBAuthorizationRule] eine Autorisierungsregel abrufen, die zur Bereitstellung einer Verbindungszeichenfolge verwendet wird. Sie speichern einen Verweis auf die `NamespaceManager`-Instanz in der `$NamespaceManager`-Variable. Sie werden `$NamespaceManager` später im Skript verwenden, um weitere Entitäten bereitzustellen.

```powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the event hub
Write-Host "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Host "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```

## <a name="provisioning-other-service-bus-entities"></a>Bereitstellen weiterer Service Bus-Entitäten
Um weitere Entitäten wie etwa Warteschlangen, Themen und Event Hubs bereitzustellen, verwenden Sie die [.NET-API für Service Bus][.NET-API für Service Bus]. In diesem Artikel geht es nur um Event Hubs, die Schritte sind für andere Entitäten jedoch ähnlich. Darüber hinaus finden Sie am Ende dieses Artikels Hinweise auf einige detaillierte Beispiele, auch für andere Entitäten.

In diesem Teil des Skripts werden vier weitere lokale Variablen erstellt. Diese Variablen werden zum Instanziieren eines `EventHubDescription` -Objekts verwendet. Das Skript führt Folgendes aus:

1. Überprüfen Sie zunächst mithilfe des `NamespaceManager`-Objekts, ob der durch `$Path` identifizierte Event Hub vorhanden ist.
2. Wenn dieser nicht vorhanden ist, erstellen Sie ein `EventHubDescription`-Objekt und übergeben dieses an die `CreateEventHubIfNotExists`-Methode der `NamespaceManager`-Klasse.
3. Nachdem Sie nun wissen, dass der Event Hub verfügbar ist, erstellen Sie mithilfe von `ConsumerGroupDescription` und `NamespaceManager` eine Verbrauchergruppe.
   
    ```powershell
    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"
   
    # Check to see if the Event Hub already exists
    if ($NamespaceManager.EventHubExists($Path))
    {
        Write-Host "The [$Path] event hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Host "Creating the [$Path] event hub in the [$Namespace] namespace: PartitionCount=[$PartitionCount] MessageRetentionInDays=[$MessageRetentionInDays]..."
        $EventHubDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.EventHubDescription -ArgumentList $Path
        $EventHubDescription.PartitionCount = $PartitionCount
        $EventHubDescription.MessageRetentionInDays = $MessageRetentionInDays
        $EventHubDescription.UserMetadata = $UserMetadata
        $EventHubDescription.Path = $Path
        $NamespaceManager.CreateEventHubIfNotExists($EventHubDescription)
        Write-Host "The [$Path] event hub in the [$Namespace] namespace has been successfully created."
    }
   
    # Create the consumer group if it doesn't exist
    Write-Host "Creating the consumer group [$ConsumerGroupName] for the [$Path] event hub..."
    $ConsumerGroupDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.ConsumerGroupDescription -ArgumentList $Path, $ConsumerGroupName
    $ConsumerGroupDescription.UserMetadata = $ConsumerGroupUserMetadata
    $NamespaceManager.CreateConsumerGroupIfNotExists($ConsumerGroupDescription)
    Write-Host "The consumer group [$ConsumerGroupName] for the [$Path] event hub has been successfully created."
    ```

## <a name="migrate-a-namespace-to-another-azure-subscription"></a>Migrieren eines Namespaces zu einem anderen Azure-Abonnement
Mit der folgenden Befehlssequenz wird ein Namespace aus einem Azure-Abonnement in ein anderes verschoben. Um diesen Vorgang auszuführen, muss der Namespace bereits aktiv sein und der Benutzer, der die PowerShell-Befehle ausführt, muss sowohl im Quell- als auch im Zielabonnement Administratorrechte besitzen.

```powershell
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Nächste Schritte
Dieser Artikel beschreibt die grundlegenden Schritte zur Bereitstellung von Service Bus-Entitäten mithilfe von PowerShell. Alle Aufgaben, die Sie mithilfe der .NET-Clientbibliotheken ausführen können, können Sie auch in einem PowerShell-Skript erledigen.

In den folgenden Blogbeiträgen finden Sie ausführlichere Beispiele:

* [How to create Service Bus queues, topics and subscriptions using a PowerShell script (Erstellen von Service Bus-Warteschlangen, -Themen und -Abonnements mithilfe eines PowerShell-Skripts, in englischer Sprache)](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [How to create a Service Bus Namespace and an Event Hub using a PowerShell script (Erstellen eines Service Bus-Namespace und eines Event Hubs mithilfe eines PowerShell-Skripts, in englischer Sprache)](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Es stehen auch einige einsatzbereite Skripts zum Download zur Verfügung:

* [Service Bus PowerShell-Skripts](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Link references-->
[Purchase Options]: http://azure.microsoft.com/pricing/purchase-options/
[Member Offers]: http://azure.microsoft.com/pricing/member-offers/
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Service Bus NuGet package]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[Get-AzureSBNamespace]: https://docs.microsoft.com/powershell/servicemanagement/azure.compute/v1.6.1/Get-AzureSBNamespace
[New-AzureSBNamespace]: https://docs.microsoft.com/powershell/servicemanagement/azure.compute/v1.6.1/new-azuresbnamespace
[Get-AzureSBAuthorizationRule]: https://docs.microsoft.com/powershell/servicemanagement/azure.compute/v1.6.1/get-azuresbauthorizationrule
[NamespaceManager]: https://docs.microsoft.com/dotnet/api/microsoft.servicebus.namespacemanager



<!--HONumber=Jan17_HO2-->


