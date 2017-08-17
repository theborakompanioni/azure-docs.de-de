---
title: Verwalten von Azure Redis Cache mit Azure PowerShell | Microsoft Docs
description: "Erfahren Sie, wie Sie administrative Aufgaben für Azure-Redis-Cache mit Azure PowerShell ausführen."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 1136efe5-1e33-4d91-bb49-c8e2a6dca475
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: sdanie
ms.translationtype: HT
ms.sourcegitcommit: 8021f8641ff3f009104082093143ec8eb087279e
ms.openlocfilehash: 0a5c95eab3fd01f611fc049e80c5c506857e0b81
ms.contentlocale: de-de
ms.lasthandoff: 07/21/2017

---
# <a name="manage-azure-redis-cache-with-azure-powershell"></a>Verwalten von Azure-Redis-Cache mit Azure PowerShell
> [!div class="op_single_selector"]
> * [PowerShell](cache-howto-manage-redis-cache-powershell.md)
> * [Azure-Befehlszeilenschnittstelle](cache-manage-cli.md)
> 
> 

Dieses Thema zeigt, wie Sie gängige Aufgaben wie Erstellen, Aktualisieren und Skalieren Ihrer Azure Redis Cache-Instanzen ausführen, Zugriffsschlüssel neu generieren und Ihre Caches anzeigen. Eine vollständige Liste der Azure Redis Cache-PowerShell-Cmdlets finden Sie unter [Azure Redis Cache-Cmdlets](https://msdn.microsoft.com/library/azure/mt634513.aspx).

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

Weitere Informationen über das klassische Bereitstellungsmodell finden Sie unter [Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung: Grundlegendes zu Bereitstellungsmodellen und zum Status von Ressourcen](../azure-resource-manager/resource-manager-deployment-model.md#classic-deployment-characteristics).

## <a name="prerequisites"></a>Voraussetzungen
Wenn Sie bereits Azure PowerShell installiert haben, benötigen Sie Version 1.0.0 oder höher. Sie können die von Ihnen installierte Azure Power Shell-Version mit diesem Befehl über die Azure PowerShell-Eingabeaufforderung prüfen.

    Get-Module azure | format-table version


Zunächst müssen Sie sich mit dem folgenden Befehl bei Azure anmelden.

    Login-AzureRmAccount

Geben Sie die E-Mail-Adresse Ihres Azure-Kontos und das Kennwort in das Anmeldedialogfeld von Microsoft Azure ein.

Wenn Sie über mehrere Azure-Abonnements verfügen, müssen Sie als Nächstes Ihr Azure-Abonnement festlegen. Führen Sie den folgenden Befehl aus, um eine Liste Ihrer aktuellen Abonnements anzuzeigen.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Führen Sie den folgenden Befehl aus, um das Abonnement anzugeben. Im folgenden Beispiel ist der Name des Abonnements `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

Bevor Sie Windows PowerShell mit dem Azure-Ressourcen-Manager verwenden können, benötigen Sie Folgendes:

* Windows PowerShell, Version 3.0 oder 4.0. Um die Version von Windows PowerShell zu ermitteln, geben Sie `$PSVersionTable` ein. Vergewissern Sie sich, dass der Wert von `PSVersion` 3.0 oder 4.0 ist. Informationen zum Installieren einer kompatiblen Version finden Sie unter [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) oder [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

Um detaillierte Hilfe zu einem Cmdlet aus dem Lernprogramm zu erhalten, verwenden Sie das „Get-Help“-Cmdlet.

    Get-Help <cmdlet-name> -Detailed

Geben Sie beispielsweise Folgendes ein, um Hilfe zum `New-AzureRmRedisCache` -Cmdlet zu erhalten:

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-to-connect-to-other-clouds"></a>Herstellen von Verbindungen mit anderen Clouds
Standardmäßig ist die Azure-Umgebung `AzureCloud` (die globale Azure-Cloudinstanz). Verwenden Sie zum Verbinden mit einer anderen Instanz den `Add-AzureRmAccount`-Befehl mit dem Befehlszeilenschalter `-Environment` oder -`EnvironmentName` mit der gewünschten Umgebung bzw. dem gewünschten Umgebungsnamen.

Führen Sie zum Anzeigen der Liste der verfügbaren Umgebungen das Cmdlet `Get-AzureRmEnvironment` aus.

### <a name="to-connect-to-the-azure-government-cloud"></a>Verbinden mit der Azure Government-Cloud
Verwenden Sie zum Verbinden mit der Azure Government-Cloud einen der folgenden Befehle.

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

oder

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

Verwenden Sie zum Erstellen eines Caches in der Azure Government-Cloud einen der folgenden Standorte.

* US Government, Virginia
* US Government, Iowa

Weitere Informationen zur Azure Government-Cloud finden Sie unter [Microsoft Azure Government](https://azure.microsoft.com/features/gov/) und im [Microsoft Azure Government-Entwicklerhandbuch](../azure-government-developer-guide.md).

### <a name="to-connect-to-the-azure-china-cloud"></a>Verbinden mit der Azure China-Cloud
Verwenden Sie zum Verbinden mit der Azure China-Cloud einen der folgenden Befehle.

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

oder

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

Verwenden Sie zum Erstellen eines Caches in der Azure China-Cloud einen der folgenden Standorte.

* China, Osten
* China, Norden

Weitere Informationen zur Azure China-Cloud finden Sie unter [AzureChinaCloud für Azure wird in China von 21Vianet betrieben](http://www.windowsazure.cn/).

### <a name="to-connect-to-microsoft-azure-germany"></a>So stellen Sie eine Verbindung mit Microsoft Azure Deutschland her
Verwenden Sie zum Herstellen einer Verbindung mit Microsoft Azure Deutschland einen der folgenden Befehle.

    Add-AzureRMAccount -EnvironmentName AzureGermanCloud


or

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureGermanCloud)

Verwenden Sie einen der folgenden Orte, um einen Cache in Microsoft Azure Deutschland zu erstellen.

* Deutschland, Mitte
* Deutschland, Nordosten

Weitere Informationen zu Microsoft Azure Deutschland finden Sie unter [Microsoft Azure Deutschland](https://azure.microsoft.com/overview/clouds/germany/).

### <a name="properties-used-for-azure-redis-cache-powershell"></a>Eigenschaften, die für Azure Redis Cache-PowerShell verwendet werden
Die folgende Tabelle enthält Eigenschaften und Beschreibungen für Parameter, die beim Erstellen und Verwalten Ihrer Azure Redis Cache-Instanzen mithilfe von Azure PowerShell häufig verwendet werden.

| Parameter | Beschreibung | Standard |
| --- | --- | --- |
| Name |Name des Caches | |
| Ort |Ort des Caches | |
| ResourceGroupName |Name der Ressourcengruppe, in der der Cache erstellt werden soll | |
| Größe |Die Größe des Caches. Gültige Werte sind: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250 MB, 1 GB, 2,5 GB, 6 GB, 13 GB, 26 GB, 53 GB |1 GB |
| ShardCount |Die Anzahl der zu erstellenden Shards beim Erstellen eines Premium-Caches mit Clusterunterstützung. Gültige Werte sind: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 | |
| SKU |Gibt die SKU des Caches an. Gültige Werte sind: Basic, Standard, Premium |Standard |
| RedisConfiguration |Gibt die Konfigurationseinstellungen für Redis an. Details zu den einzelnen Einstellungen finden Sie in der Tabelle [RedisConfiguration-Eigenschaften](#redisconfiguration-properties) . | |
| EnableNonSslPort |Gibt an, ob der Nicht-SSL-Port aktiviert ist. |False |
| MaxMemoryPolicy |Dieser Parameter ist veraltet – verwenden Sie stattdessen RedisConfiguration. | |
| StaticIP |Wenn Sie den Cache in einem VNET hosten, geben Sie hiermit eine eindeutige IP-Adresse im Subnetz für den Cache an. Wird keine IP-Adresse angegeben, wird eine für Sie aus dem Subnetz ausgewählt. | |
| Subnetz |Wenn Sie den Cache in einem VNET hosten, geben Sie hiermit den Namen des Subnetzes an, in dem der Cache bereitgestellt wird. | |
| VirtualNetwork |Wenn Sie den Cache in einem VNET hosten, geben Sie hiermit die Ressourcen-ID des VNET an, in dem der Cache bereitgestellt wird. | |
| KeyType |Gibt an, welcher Zugriffsschlüssel beim Erneuern der Zugriffsschlüssel neu generiert werden muss. Gültige Werte sind: Primary, Secondary | |

### <a name="redisconfiguration-properties"></a>RedisConfiguration-Eigenschaften
| Eigenschaft | Beschreibung | Tarife |
| --- | --- | --- |
| rdb-backup-enabled |Gibt an, ob [Redis-Datenpersistenz](cache-how-to-premium-persistence.md) aktiviert ist |Nur Premium |
| rdb-storage-connection-string |Die Verbindungszeichenfolge für das Speicherkonto für [Redis-Datenpersistenz](cache-how-to-premium-persistence.md) |Nur Premium |
| rdb-backup-frequency |Die Sicherungshäufigkeit für [Redis-Datenpersistenz](cache-how-to-premium-persistence.md) |Nur Premium |
| maxmemory-reserved |Konfiguriert den für andere Prozesse als Cacheprozesse [reservierten Speicher](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) |Standard und Premium |
| maxmemory-policy |Konfiguriert die [Entfernungsrichtlinie](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) für den Cache |Alle Tarife |
| notify-keyspace-events |Konfiguriert die [Keyspace-Benachrichtigungen](cache-configure.md#keyspace-notifications-advanced-settings) |Standard und Premium |
| hash-max-ziplist-entries |Konfiguriert die [Speicheroptimierung](http://redis.io/topics/memory-optimization) für kleine Aggregatdatentypen |Standard und Premium |
| hash-max-ziplist-value |Konfiguriert die [Speicheroptimierung](http://redis.io/topics/memory-optimization) für kleine Aggregatdatentypen |Standard und Premium |
| set-max-intset-entries |Konfiguriert die [Speicheroptimierung](http://redis.io/topics/memory-optimization) für kleine Aggregatdatentypen |Standard und Premium |
| zset-max-ziplist-entries |Konfiguriert die [Speicheroptimierung](http://redis.io/topics/memory-optimization) für kleine Aggregatdatentypen |Standard und Premium |
| zset-max-ziplist-value |Konfiguriert die [Speicheroptimierung](http://redis.io/topics/memory-optimization) für kleine Aggregatdatentypen |Standard und Premium |
| Datenbanken |Konfiguriert die Anzahl der Datenbanken. Diese Eigenschaft kann nur bei der Erstellung des Caches konfiguriert werden. |Standard und Premium |

## <a name="to-create-a-redis-cache"></a>So erstellen Sie einen Redis-Cache
Neue Azure Redis Cache-Instanzen werden mit dem [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) -Cmdlet erstellt.

> [!IMPORTANT]
> Beim ersten Erstellen eines Redis-Caches in einem Abonnement mithilfe des Azure-Portals registriert das Portal den `Microsoft.Cache`-Namespace für dieses Abonnement. Wenn Sie versuchen, den ersten Redis-Cache in einem Abonnement mithilfe von PowerShell zu erstellen, müssen Sie zunächst diesen Namespace mit dem folgenden Befehl registrieren; andernfalls tritt beim Ausführen von Cmdlets wie `New-AzureRmRedisCache` und `Get-AzureRmRedisCache` ein Fehler auf.
> 
> `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`
> 
> 

Um eine Liste der verfügbaren Parameter und ihrer Beschreibungen für `New-AzureRmRedisCache`anzuzeigen, führen Sie den folgenden Befehl aus.

    PS C:\> Get-Help New-AzureRmRedisCache -detailed

    NAME
        New-AzureRmRedisCache

    SYNOPSIS
        Creates a new redis cache.


    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]


    DESCRIPTION
        The New-AzureRmRedisCache cmdlet creates a new redis cache.


    PARAMETERS
        -Name <String>
            Name of the redis cache to create.

        -ResourceGroupName <String>
            Name of resource group in which to create the redis cache.

        -Location <String>
            Location in which to create the redis cache.

        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.

        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.

        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.

        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}

        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Um einen Cache mit Standardparametern zu erstellen, führen Sie den folgenden Befehl aus.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name` und `Location` sind erforderliche Parameter, aber die übrigen sind optional und haben Standardwerte. Mit dem vorherigen Befehl erstellen Sie eine Standard-SKU-Azure Redis Cache-Instanz mit Name, Speicherort und Ressourcengruppe gemäß der entsprechenden Angaben und einer Größe von 1 GB, für die der Nicht-SSL-Port deaktiviert ist.

Um einen Premium-Cache zu erstellen, geben Sie eine Größe von P1 (6 bis 60 GB), P2 (13 bis 130 GB), P3 (26 bis 260 GB), oder P4 (53 bis 530 GB) ein. Um Clustering zu aktivieren, geben Sie eine Shardanzahl mit dem Parameter `ShardCount` ein. Das folgende Beispiel erstellt einen P1-Premium-Cache mit 3 Shards. Ein P1-Premium-Cache ist 6 GB groß, und da wir drei Shards angegeben haben, beträgt die Gesamtgröße 18 GB (3 x 6 GB).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

Um Werte für die `RedisConfiguration`-Parameter anzugeben, schließen Sie die Werte als Schlüssel-Wert-Paare in `{}` ein (Beispiel: `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`). Im folgenden Beispiel wird ein standardmäßiger 1 GB-Cache mit `allkeys-random`-maxmemory-Richtlinie und mit `KEA` konfigurierten Keyspace-Benachrichtigungen erstellt. Weitere Informationen finden Sie unter [Keyspacebenachrichtigungen (Erweiterte Einstellungen)](cache-configure.md#keyspace-notifications-advanced-settings) und [Arbeitsspeicherrichtlinien](cache-configure.md#memory-policies).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>

## <a name="to-configure-the-databases-setting-during-cache-creation"></a>So konfigurieren Sie die Einstellung für Datenbanken während der Erstellung des Caches
Die Einstellung `databases` kann nur während der Erstellung des Caches konfiguriert werden. Mit dem folgenden Beispiel wird ein Premium-P3-Cache (26 GB) mit 48 Datenbanken mit dem [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) -Cmdlet erstellt.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

Weitere Informationen zur Eigenschaft `databases` finden Sie unter [Standardmäßige Redis-Serverkonfiguration](cache-configure.md#default-redis-server-configuration). Weitere Informationen zum Erstellen eines Caches mithilfe des [New-AzureRmRedisCache-Cmdlets](https://msdn.microsoft.com/library/azure/mt634517.aspx) finden Sie im vorherigen Abschnitt [So erstellen Sie eine Redis Cache-Instanz](#to-create-a-redis-cache).

## <a name="to-update-a-redis-cache"></a>So aktualisieren Sie einen Redis-Cache
Azure Redis Cache-Instanzen werden mit dem [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) -Cmdlet aktualisiert.

Um eine Liste der verfügbaren Parameter und ihrer Beschreibungen für `Set-AzureRmRedisCache`anzuzeigen, führen Sie den folgenden Befehl aus.

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed

    NAME
        Set-AzureRmRedisCache

    SYNOPSIS
        Set redis cache updatable parameters.

    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]

    DESCRIPTION
        The Set-AzureRmRedisCache cmdlet sets redis cache parameters.

    PARAMETERS
        -Name <String>
            Name of the redis cache to update.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.

        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Das Cmdlet `Set-AzureRmRedisCache` kann verwendet werden, um Eigenschaften wie `Size`, `Sku`, `EnableNonSslPort` und die `RedisConfiguration`-Werte zu aktualisieren. 

Der folgende Befehl aktualisiert die maxmemory-Richtlinie für den Redis-Cache mit dem Namen „myCache“.

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>

## <a name="to-scale-a-redis-cache"></a>Skalieren eines Redis-Cache
`Set-AzureRmRedisCache` kann verwendet werden, um eine Azure Redis Cache-Instanz zu skalieren, wenn die Eigenschaften `Size`, `Sku` oder `ShardCount` geändert werden. 

> [!NOTE]
> Das Skalieren eines Caches mithilfe von PowerShell unterliegt den gleichen Einschränkungen und Richtlinien wie das Skalieren eines Caches über das Azure-Portal. Sie können mit den folgenden Einschränkungen auf eine andere Preisstufe skalieren.
> 
> * Sie können keine Skalierung von einem höheren Tarif auf einen niedrigeren Tarif vornehmen.
> * Eine Skalierung von einem **Premium**-Cache auf einen niedrigeren **Standard**- oder **Basic**-Cache ist nicht möglich.
> * Ein **Standard**-Cache kann nicht auf einen niedrigeren **Basic**-Cache skaliert werden.
> * Ein **Basic**-Cache kann auf einen **Standard**-Cache skaliert werden, die Größe kann jedoch nicht gleichzeitig geändert werden. Wenn Sie eine andere Größe benötigen, können Sie anschließend einen Skalierungsvorgang auf die gewünschte Größe durchführen.
> * Ein **Basic**-Cache kann nicht direkt auf einen **Premium**-Cache skaliert werden. Stattdessen müssen Sie in einem ersten Skalierungsvorgang von **Basic** auf **Standard** skalieren und dann in einem anschließenden Skalierungsvorgang von **Standard** auf **Premium**.
> * Von einer größeren Größe kann nicht auf **C0 (250 MB)** herunterskaliert werden.
> 
> Weitere Informationen finden Sie unter [Skalieren von Azure Redis Cache](cache-how-to-scale.md).
> 
> 

Das folgende Beispiel veranschaulicht, wie ein Cache mit Namen `myCache` zu einem 2,5-GB-Cache skaliert wird. Beachten Sie, dass dieser Befehl sowohl für einen Basic- als auch für einen Standard-Cache verwendet werden kann.

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Nach Eingabe dieses Befehls wird der Status des Caches zurückgegeben (ähnlich wie beim Aufruf von `Get-AzureRmRedisCache`). Beachten Sie, dass der `ProvisioningState` `Scaling` ist.

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB


    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

Nach Abschluss des Skalierungsvorgangs ändert sich `ProvisioningState` in `Succeeded`. Wenn Sie weitere Skalierungen vornehmen möchten (etwa einen Wechsel von Basic zu Standard oder eine Änderung der Größe), müssen Sie warten, bis der vorherige Vorgang abgeschlossen ist. Andernfalls wird eine Fehlermeldung wie die folgende angezeigt.

    Set-AzureRmRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-a-redis-cache"></a>Abrufen von Informationen zu einem Redis-Cache
Sie können Informationen zu einem Cache mithilfe des [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) -Cmdlets abrufen.

Um eine Liste der verfügbaren Parameter und ihrer Beschreibungen für `Get-AzureRmRedisCache`anzuzeigen, führen Sie den folgenden Befehl aus.

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed

    NAME
        Get-AzureRmRedisCache

    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.

    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]

    DESCRIPTION
        The Get-AzureRmRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.

        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.

        If no parameters are given than it will return details about all caches the current subscription.

    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns the details for the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Um Informationen über alle Caches im aktuellen Abonnement zurückzugeben, führen Sie `Get-AzureRmRedisCache` ohne Parameter aus.

    Get-AzureRmRedisCache

Um Informationen über alle Caches in einer bestimmten Ressourcengruppe zurückzugeben, führen Sie `Get-AzureRmRedisCache` mit dem `ResourceGroupName`-Parameter aus.

    Get-AzureRmRedisCache -ResourceGroupName myGroup

Um Informationen zu einem bestimmten Cache zurückzugeben, führen Sie `Get-AzureRmRedisCache` mit dem `Name`-Parameter aus, der den Namen des Caches enthält, und den `ResourceGroupName`-Parameter mit der Ressourcengruppe, die diesen Cache enthält.

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="to-retrieve-the-access-keys-for-a-redis-cache"></a>Abrufen der Zugriffsschlüssel für einen Redis-Cache
Um die Zugriffsschlüssel für Ihren Cache abzurufen, können Sie das [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) -Cmdlet verwenden.

Um eine Liste der verfügbaren Parameter und ihrer Beschreibungen für `Get-AzureRmRedisCacheKey`anzuzeigen, führen Sie den folgenden Befehl aus.

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed

    NAME
        Get-AzureRmRedisCacheKey

    SYNOPSIS
        Gets the accesskeys for the specified redis cache.


    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]

    DESCRIPTION
        The Get-AzureRmRedisCacheKey cmdlet gets the access keys for the specified cache.

    PARAMETERS
        -Name <String>
            Name of the redis cache.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Um die Schlüssel für Ihren Cache abzurufen, rufen Sie das `Get-AzureRmRedisCacheKey` -Cmdlet auf und übergeben Sie im Namen Ihres Caches den Namen der Ressourcengruppe, die den Cache enthält.

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup

    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-redis-cache"></a>Abrufen der Zugriffsschlüssel für Ihren Redis-Cache
Um die Zugriffsschlüssel für Ihren Cache abzurufen, können Sie das [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) -Cmdlet verwenden.

Um eine Liste der verfügbaren Parameter und ihrer Beschreibungen für `New-AzureRmRedisCacheKey`anzuzeigen, führen Sie den folgenden Befehl aus.

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed

    NAME
        New-AzureRmRedisCacheKey

    SYNOPSIS
        Regenerates the access key of a redis cache.

    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]

    DESCRIPTION
        The New-AzureRmRedisCacheKey cmdlet regenerate the access key of a redis cache.

    PARAMETERS
        -Name <String>
            Name of the redis cache.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        -KeyType <String>
            Specifies whether to regenerate the primary or secondary access key. Possible values are Primary or Secondary.

        -Force
            When the Force parameter is provided, the specified access key is regenerated without any confirmation prompts.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Um den primären oder sekundären Schlüssel für Ihren Cache erneut zu generieren, rufen Sie das `New-AzureRmRedisCacheKey`-Cmdlet auf und übergeben Sie den Namen und die Ressourcengruppe. Geben Sie entweder `Primary` oder `Secondary` für den `KeyType`-Parameter an. Im folgenden Beispiel wird der sekundäre Zugriffsschlüssel für einen Cache erneut generiert.

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary

    Confirm
    Are you sure you want to regenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-a-redis-cache"></a>Löschen eines Redis-Caches
Um einen Redis-Cache zu löschen, verwenden Sie das [Remove-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) -Cmdlet.

Um eine Liste der verfügbaren Parameter und ihrer Beschreibungen für `Remove-AzureRmRedisCache`anzuzeigen, führen Sie den folgenden Befehl aus.

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed

    NAME
        Remove-AzureRmRedisCache

    SYNOPSIS
        Remove redis cache if exists.

    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>

    DESCRIPTION
        The Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.

    PARAMETERS
        -Name <String>
            Name of the redis cache to remove.

        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.

        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.

        -PassThru
            By default Remove-AzureRmRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating the success of the operatio

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Im folgenden Beispiel wird der Cache mit dem Namen `myCache` entfernt.

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Confirm
    Are you sure you want to remove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-a-redis-cache"></a>So importieren Sie einen Redis-Cache
Sie können mit dem Cmdlet `Import-AzureRmRedisCache` Daten in eine Azure Redis Cache-Instanz importieren.

> [!IMPORTANT]
> Import/Export ist nur für Caches im [Premium-Tarif](cache-premium-tier-intro.md) verfügbar. Weitere Informationen zum Importieren/Exportieren finden Sie unter [Importieren und Exportieren von Daten in Azure Redis Cache](cache-how-to-import-export-data.md).
> 
> 

Um eine Liste der verfügbaren Parameter und ihrer Beschreibungen für `Import-AzureRmRedisCache`anzuzeigen, führen Sie den folgenden Befehl aus.

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed

    NAME
        Import-AzureRmRedisCache

    SYNOPSIS
        Import data from blobs to Azure Redis Cache.


    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Import-AzureRmRedisCache cmdlet imports data from the specified blobs into Azure Redis Cache.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -Files <String[]>
            SAS urls of blobs whose content should be imported into the cache.

        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.

        -Force
            When the Force parameter is provided, import will be performed without any confirmation prompts.

        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If the PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating the success of the
            operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


Der folgende Befehl importiert Daten aus dem vom SAS-URI angegebenen Blob in Azure Redis Cache.

    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-a-redis-cache"></a>So exportieren Sie einen Redis-Cache
Sie können mit dem Cmdlet `Export-AzureRmRedisCache` Daten aus einer Azure Redis Cache-Instanz exportieren.

> [!IMPORTANT]
> Import/Export ist nur für Caches im [Premium-Tarif](cache-premium-tier-intro.md) verfügbar. Weitere Informationen zum Importieren/Exportieren finden Sie unter [Importieren und Exportieren von Daten in Azure Redis Cache](cache-how-to-import-export-data.md).
> 
> 

Um eine Liste der verfügbaren Parameter und ihrer Beschreibungen für `Export-AzureRmRedisCache`anzuzeigen, führen Sie den folgenden Befehl aus.

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed

    NAME
        Export-AzureRmRedisCache

    SYNOPSIS
        Exports data from Azure Redis Cache to a specified container.


    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache to a specified container.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -Prefix <String>
            Prefix to use for blob names.

        -Container <String>
            SAS url of container where data should be exported.

        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.

        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating the success of the operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


Der folgende Befehl exportiert Daten aus einer Azure Redis Cache-Instanz in den vom SAS-URI angegebenen Container.

        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-a-redis-cache"></a>So starten Sie einen Redis-Cache neu
Sie können mit dem Cmdlet `Reset-AzureRmRedisCache` die Azure Redis Cache-Instanz neu starten.

> [!IMPORTANT]
> Ein Neustart ist nur für Caches im [Premium-Tarif](cache-premium-tier-intro.md) verfügbar. Weitere Informationen zum Neustarten Ihres Cache finden Sie unter [Verwalten von Azure Redis Cache – Neustart](cache-administration.md#reboot).
> 
> 

Um eine Liste der verfügbaren Parameter und ihrer Beschreibungen für `Reset-AzureRmRedisCache`anzuzeigen, führen Sie den folgenden Befehl aus.

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed

    NAME
        Reset-AzureRmRedisCache

    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.


    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Reset-AzureRmRedisCache cmdlet reboots the specified node(s) of an Azure Redis Cache instance.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -RebootType <String>
            Which node to reboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".

        -ShardId <Integer>
            Which shard to reboot when rebooting a premium cache with clustering enabled.

        -Force
            When the Force parameter is provided, reset will be performed without any confirmation prompts.

        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating the success of the operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


Der folgende Befehl startet beide Knoten des angegebenen Cache neu.

        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Verwendung von Windows PowerShell mit Azure finden Sie in den folgenden Ressourcen:

* [Dokumentation zu den Azure Redis Cache-Cmdlets auf MSDN](https://msdn.microsoft.com/library/azure/mt634513.aspx)
* [Azure Resource Manager-Cmdlets](http://go.microsoft.com/fwlink/?LinkID=394765): Hier erfahren Sie, wie Sie die Cmdlets im Azure Resource Manager-Modul verwenden.
* [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-template-deploy-portal.md): Hier erfahren Sie, wie Sie Ressourcengruppen im Azure-Portal erstellen und verwalten.
* [Azure-Blog](http://blogs.msdn.com/windowsazure): Informationen zu neuen Funktionen in Azure.
* [Windows PowerShell-Blog](http://blogs.msdn.com/powershell): Informationen zu neuen Funktionen in Windows PowerShell.
* ["Hey, Scripting Guy!"- Blog](http://blogs.technet.com/b/heyscriptingguy/): Praktische Tipps und Tricks aus der Windows PowerShell-Community.


