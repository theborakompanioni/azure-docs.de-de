---
title: "Azure-Bibliotheken für .NET Framework und .NET Core | Microsoft-Dokumentation"
description: "Eine Liste mit allen Azure-Bibliotheken und NuGet-Paketen für .NET Framework und .NET Core."
keywords: Azure .NET, SDK, Azure .NET-API-Referenz, Azure .NET-Klassenbibliothek,Azure NuGet, Azure Core
author: camsoper
manager: douge
ms.author: casoper
ms.date: 04/06/2016
ms.topic: managed-reference
ms.prod: azure
ms.technology: azure
ms.devlang: dotnet
ms.assetid: 
translationtype: Human Translation
ms.sourcegitcommit: 0c4554d6289fb0050998765485d965d1fbc6ab3e
ms.openlocfilehash: fd38e1c9b0ebb89d930274ab92b63f4fb0219ca0
ms.lasthandoff: 04/13/2017

---

# <a name="azure-libraries-for-net"></a>Azure-Bibliotheken für .NET

Verwenden Sie die Bibliotheken im .NET SDK für Azure, um Azure-Dienste in Ihren Anwendungen zu verwalten und zu nutzen.

## <a name="packages"></a>Pakete

> [!TIP]
> [Fluent-Bibliotheken](https://azure.microsoft.com/blog/simpler-azure-management-libraries-for-net/) (mit der Endung **\*.Fluent**) verbessern die Entwicklererfahrung durch eine allgemeinere, objektorientierte API mit optimierter Les- und Schreibbarkeit. Sie machen deutlich, was erforderlich, optional oder nicht änderbar ist. Diese Bibliotheken sind mit Fluent-fremden Bibliotheken kombinierbar, sodass Sie Fluent-Pakete verwenden können, falls Sie diese Syntax bevorzugen. [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent) ist ein Rolluppaket mit allen Fluent-Verwaltungsbibliotheken.

Dienst | Interagieren mit dem Dienst | Verwalten von Dienstressourcen
--------|---------------------------|-------------------------
[Analysis Services](/azure/analysis-services/) | | [Microsoft.Azure.Management.Analysis](https://www.nuget.org/packages/Microsoft.Azure.Management.Analysis)
[ApiManagement](/azure/api-management/) | | [Microsoft.Azure.Management.ApiManagement](https://www.nuget.org/packages/Microsoft.Azure.Management.ApiManagement)
[Autorisierung](/rest/api/authorization) | | [Microsoft.Azure.Management.Authorization](https://www.nuget.org/packages/Microsoft.Azure.Management.Authorization)
[Automation](/azure/automation/) | | [Microsoft.Azure.Management.Automation](https://www.nuget.org/packages/Microsoft.Azure.Management.Automation)
[Azure Active Directory](/azure/active-directory) | [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) | 
[Sicherung](/azure/backup/) | | [Microsoft.Azure.Management.RecoveryServices.Backup](https://www.nuget.org/packages/Microsoft.Azure.Management.RecoveryServices.Backup)
[Batch](/azure/batch/) | [Azure.Batch](https://www.nuget.org/packages/Azure.Batch) | [Microsoft.Azure.Management.Batch.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Batch.Fluent)<br/>[Microsoft.Azure.Management.Batch](https://www.nuget.org/packages/Microsoft.Azure.Management.Batch)
[Abrechnung](/azure/billing/) | | [Microsoft.Azure.Management.Billing](https://www.nuget.org/packages/Microsoft.Azure.Management.Billing)
[CDN](/azure/cdn/) | | [Microsoft.Azure.Management.Cdn.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Cdn.Fluent)<br/>[Microsoft.Azure.Management.Cdn](https://www.nuget.org/packages/Microsoft.Azure.Management.Cdn)
[Cognitive Services](/azure/cognitive-services/) | | [Microsoft.Azure.Management.CognitiveServices](https://www.nuget.org/packages/Microsoft.Azure.Management.CognitiveServices)
[Commerce](/azure/billing/billing-usage-rate-card-overview) | | [Microsoft.Azure.Commerce.UsageAggregates](https://www.nuget.org/packages/Microsoft.Azure.Commerce.UsageAggregates)
[Containerregistrierung](/azure/container-registry) | | [Microsoft.Azure.Management.ContainerRegistry](https://www.nuget.org/packages/Microsoft.Azure.Management.ContainerRegistry)
[Customer Insights](/dynamics365/customer-insights) | | [Microsoft.Azure.Management.CustomerInsights](https://www.nuget.org/packages/Microsoft.Azure.Management.CustomerInsights)
[Data Factory](/azure/data-factory/) | | [Microsoft.Azure.Management.DataFactories](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories)
[Data Lake Analytics](/azure/data-lake-analytics/) | [Microsoft.Azure.Management.DataLake.Analytics](http://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.Analytics) | [Microsoft.Azure.Management.DataLake.Analytics](https://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.Analytics)
[Data Lake Store](/azure/data-lake-store/) | [Microsoft.Azure.Management.DataLake.Store](http://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.Store) | [Microsoft.Azure.Management.DataLake.Store](https://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.Store)<br/>[Microsoft.Azure.Management.DataLake.StoreUploader](https://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.StoreUploader)
[DevTest Labs](/azure/devtest-lab/) | | [Microsoft.Azure.Management.DevTestLabs](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs)
[DNS](/azure/dns/) | | [Microsoft.Azure.Management.Dns.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Dns.Fluent)<br/>[Microsoft.Azure.Management.Dns](https://www.nuget.org/packages/Microsoft.Azure.Management.Dns)
[DocumentDB](/azure/documentdb/) | [Microsoft.Azure.DocumentDB.Core](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core) | 
[Event Hubs](/azure/event-hubs/) | [Microsoft.Azure.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.EventHubs)<br/>[Microsoft.Azure.EventHubs.Processor](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor)
[Graph](/rest/api/graphrbac) | | [Microsoft.Azure.Graph.RBAC](https://www.nuget.org/packages/Microsoft.Azure.Graph.RBAC)
[HDInsight](/azure/hdinsight/) | [Microsoft.Azure.Management.HDInsight.Job](http://www.nuget.org/packages/Microsoft.Azure.Management.HDInsight.Job) | [Microsoft.Azure.Management.HDInsight](https://www.nuget.org/packages/Microsoft.Azure.Management.HDInsight)
[IoT Hub](/azure/iot-hub) | [Microsoft.Azure.Devices](https://www.nuget.org/packages/Microsoft.Azure.Devices)&nbsp;<b>&#42;</b><br/>[Microsoft.Azure.Devices.Client](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client)&nbsp;<b>&#42;</b>
[Schlüsseltresor](/azure/key-vault/) | [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) | [Microsoft.Azure.Management.KeyVault.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.KeyVault.Fluent)<br/>[Microsoft.Azure.Management.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.Management.KeyVault)
[Log Analytics](/azure/log-analytics/) | | [Microsoft.Azure.Management.OperationalInsights](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights)
[Logik-Apps](/azure/logic-apps/) | | [Microsoft.Azure.Management.Logic](https://www.nuget.org/packages/Microsoft.Azure.Management.Logic)
[MachineLearning](/azure/machine-learning/) | | [Microsoft.Azure.Management.MachineLearning](https://www.nuget.org/packages/Microsoft.Azure.Management.MachineLearning)
[Media Services](/azure/media-services/) | [windowsazure.mediaservices.extensions](https://www.nuget.org/packages/windowsazure.mediaservices.extensions)&nbsp;<b>&#42;</b> | [Microsoft.Azure.Management.Media](https://www.nuget.org/packages/Microsoft.Azure.Management.Media)
[Überwachen](/azure/monitoring-and-diagnostics/) | | [Microsoft.Azure.Insights](https://www.nuget.org/packages/Microsoft.Azure.Insights)
[Notification Hubs](/azure/notification-hubs/) | [Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs)&nbsp;<b>&#42;</b><br/>[WindowsAzure.Messaging.Managed](https://www.nuget.org/packages/WindowsAzure.Messaging.Managed)&nbsp;<b>&#42;</b> | [Microsoft.Azure.Management.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.Management.NotificationHubs)
[Power BI Embedded](/azure/power-bi-embedded/) | | [Microsoft.Azure.Management.PowerBIEmbedded](https://www.nuget.org/packages/Microsoft.Azure.Management.PowerBIEmbedded)
[Recovery Services](/azure/site-recovery/) | | [Microsoft.Azure.Management.RecoveryServices](https://www.nuget.org/packages/Microsoft.Azure.Management.RecoveryServices)
[Redis Cache](/azure/redis-cache/) | [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/) | [Microsoft.Azure.Management.Redis.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Redis.Fluent)<br/>[Microsoft.Azure.Management.Redis](https://www.nuget.org/packages/Microsoft.Azure.Management.Redis)
[Ressourcen-Manager](/azure/azure-resource-manager/) | | [Microsoft.Azure.Management.ResourceManager.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager.Fluent)<br/>[Microsoft.Azure.Management.ResourceManager](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)
[Scheduler](/azure/scheduler/) | | [Microsoft.Azure.Management.Scheduler](https://www.nuget.org/packages/Microsoft.Azure.Management.Scheduler)
[Suchen,](/azure/search/) | [Microsoft.Azure.Search](https://www.nuget.org/packages/Microsoft.Azure.Search)&nbsp;<b>&#42;</b> | [Microsoft.Azure.Management.Search](https://www.nuget.org/packages/Microsoft.Azure.Management.Search)
[Service Bus](/azure/service-bus/) | [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus)&nbsp;<b>&#42;</b> | [Microsoft.Azure.Management.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.Management.ServiceBus)
[Service Bus Relay](/azure/service-bus-relay/) | [Microsoft.Azure.Relay](https://www.nuget.org/packages/Microsoft.Azure.Relay)
[Service Fabric](/azure/service-fabric/) | [Microsoft.ServiceFabric](https://www.nuget.org/profiles/servicefabric)&nbsp;<b>&#42;</b> | 
[SQL-Datenbank](/azure/sql-database/) | [System.Data.SqlClient](https://www.nuget.org/packages/System.Data.SqlClient) | [Microsoft.Azure.Management.Sql.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql.Fluent)<br/>[Microsoft.Azure.Management.Sql](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)
[Speicher](/azure/storage/) | [WindowsAzure.Storage](http://www.nuget.org/packages/WindowsAzure.Storage) | [Microsoft.Azure.Management.Storage.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage.Fluent)<br/>[Microsoft.Azure.Management.Storage](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage)<br/>[Microsoft.Azure.Storage.DataMovement](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement)
[Stream Analytics](/azure/stream-analytics/) | | [Microsoft.Azure.Management.StreamAnalytics](https://www.nuget.org/packages/Microsoft.Azure.Management.StreamAnalytics)
[Traffic Manager](/azure/traffic-manager/) | | [Microsoft.Azure.Management.TrafficManager.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.TrafficManager.Fluent)<br/>[Microsoft.Azure.Management.TrafficManager](https://www.nuget.org/packages/Microsoft.Azure.Management.TrafficManager)
[Virtuelle Computer](/azure/virtual-machines/) | | [Microsoft.Azure.Management.Compute.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent)<br/>[Microsoft.Azure.Management.Compute](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute)
[Virtual Network](/azure/virtual-network/) | | [Microsoft.Azure.Management.Network.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Network.Fluent)<br/>[Microsoft.Azure.Management.Network](https://www.nuget.org/packages/Microsoft.Azure.Management.Network)
[Web-Apps](/azure/app-service-web) | | [Microsoft.Azure.Management.AppService.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.AppService.Fluent)<br/>[Microsoft.Azure.Management.Websites](https://www.nuget.org/packages/Microsoft.Azure.Management.Websites)

\* *– Gibt an, dass das Paket derzeit nicht mit .NET Core kompatibel ist.*

## <a name="installation"></a>Installation

### <a name="visual-studio"></a>Visual Studio

Importieren Sie das Paket bei Verwendung von Visual Studio mithilfe der **NuGet-Paket-Manager-Konsole** in Ihr Projekt.

1. Starten Sie bei geöffneter Visual Studio-Projektmappe die Konsole, indem Sie auf **Tools** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole** klicken.  

    ![Paket-Manager-Konsole](media/index/package-manager.png)

2. Verwenden Sie im Konsolenfenster das Cmdlet **Install-Package**, um das NuGet-Paket herunterzuladen und zu installieren.  Verwenden Sie beispielsweise Folgendes, um die neueste Version der [Azure-Ressourcenverwaltungsbibliothek](http://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager) für .NET einzuschließen:

    ```powershell
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre 
    ``` 
    Wenn Sie eine bestimmte Version verwenden möchten, schließen Sie die Versionsnummer mit ein:

    ```powershell
    Install-Package Microsoft.Azure.Management.ResourceMananger -Version 1.4.0-preview
    ``` 

### <a name="other-editors"></a>Andere Editoren

Wenn Sie .NET Core mit Visual Studio Code (oder einen anderen Editor) verwenden, bearbeiten Sie die CSPROJ-Datei, um das Paket als **PackageReference**-Element hinzuzufügen.  In diesem Beispiel wird eine spezifische Version von **Microsoft.Azure.Management.ResourceManager** verwendet. Dies wird zwar empfohlen, es werden jedoch auch [gleitende Versionen](/nuget/consume-packages/package-references-in-project-files#floating-versions) (NuGet) unterstützt.

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.Azure.Management.ResourceManager" Version="1.4.0-preview" />
  </ItemGroup>

</Project>
```

> [!TIP]
> Bei älteren Versionen von .NET Core wurden anstelle von CSPROJ-Dateien Dateien vom Typ „project.json“ verwendet.  Informationen zum Zuordnen von „project.json“ zu „csproj“ finden Sie in [diesem Dokument](/dotnet/articles/core/tools/project-json-to-csproj).

