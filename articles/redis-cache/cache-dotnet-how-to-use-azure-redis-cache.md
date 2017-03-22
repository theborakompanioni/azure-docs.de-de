---
title: Verwenden von Azure Redis Cache | Microsoft Docs
description: Erfahren Sie, wie zur Verbesserung der Leistung Ihrer Azure-Anwendungen mit Azure Redis Cache beitragen.
services: redis-cache,app-service
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: c502f74c-44de-4087-8303-1b1f43da12d5
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: 1d208f931af4704528309780b45e170627f1753c
ms.lasthandoff: 03/09/2017


---
# <a name="how-to-use-azure-redis-cache"></a>Verwenden von Azure Redis Cache
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Dieser Leitfaden beschreibt die ersten Schritte mit **Azure Redis Cache**. Microsoft Azure Redis Cache basiert auf dem beliebten Open Source Redis Cache. Sie erhalten Zugriff auf einen sicheren dedizierten Redis Cache, der von Microsoft verwaltet wird. Ein mit Azure Redis Cache erstellter Cache ist für beliebige Anwendungen in Microsoft Azure erreichbar.

Microsoft Azure Redis Cache ist in den folgenden Ebenen verfügbar:

* **Basic** – Einzelner Knoten. Mehrere Größen bis zu 53 GB.
* **Standard** – Zwei Knoten: Primär/Replikat. Mehrere Größen bis zu 53 GB. 99,9 % SLA
* **Premium** – Zwei Knoten (Primär/Replikat) mit bis zu 10 Shards. Mehrere Größen von 6 GB bis zu 530 GB (für mehr wenden Sie sich an uns). Alle Funktionen der Standard-Ebene und weitere umfassen die Unterstützung für [Redis-Cluster](cache-how-to-premium-clustering.md), [Redis-Persistenz](cache-how-to-premium-persistence.md), und [Azure Virtual Network](cache-how-to-premium-vnet.md). 99,9 % SLA

Diese Ausführungen unterscheiden sich hinsichtlich der Features und des Preises. Weitere Informationen zu Preisen finden Sie unter [Cache – Preisübersicht][Cache Pricing Details].

In dieser Anleitung erfahren Sie, wie Sie den [StackExchange.Redis][StackExchange.Redis]-Client mit C\#-Code verwenden. Die behandelten Szenarien umfassen das **Erstellen und Konfigurieren eines Caches**, das **Konfigurieren von Cacheclients** und das **Hinzufügen und Entfernen von Objekten aus dem Cache**. Weitere Informationen zur Verwendung von Azure Redis Cache finden Sie unter [Nächste Schritte][Next Steps]. Ein Tutorial zur schrittweisen Erstellung einer ASP.NET MVC-Web-App mit Redis Cache finden Sie unter [Gewusst wie: Erstellen einer Web-App mit Redis Cache](cache-web-app-howto.md).

<a name="getting-started-cache-service"></a>

## <a name="get-started-with-azure-redis-cache"></a>Erste Schritte mit Azure Redis Cache
Die ersten Schritte mit Azure Redis Cache sind einfach. Zunächst stellen Sie einen Cache bereit und konfigurieren ihn. Als Nächstes konfigurieren Sie die Cacheclients, sodass von diesen auf den Cache zugegriffen werden kann. Nach der Konfiguration der Cacheclients können Sie mit der Verwendung beginnen.

* [Erstellen des Caches][Create the cache]
* [Konfigurieren der Cacheclients][Configure the cache clients]

<a name="create-cache"></a>

## <a name="create-a-cache"></a>Erstellen eines Caches
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="to-access-your-cache-after-its-created"></a>Zugreifen auf den Cache nach der Erstellung
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Weitere Informationen zum Konfigurieren des Caches finden Sie unter [Gewusst wie: Konfigurieren von Azure Redis Cache](cache-configure.md).

<a name="NuGet"></a>

## <a name="configure-the-cache-clients"></a>Konfigurieren der Cacheclients
[!INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

Nachdem die Zwischenspeicherung im Clientprojekt konfiguriert wurde, können Sie die in den folgenden Abschnitten beschriebenen Methoden verwenden, um mit dem Cache zu arbeiten.

<a name="working-with-caches"></a>

## <a name="working-with-caches"></a>Arbeiten mit Caches
In den Schritten in diesem Abschnitt wird die Ausführung allgemeiner Aufgaben mit dem Cache beschrieben.

* [Herstellen einer Verbindung mit dem Cache][Connect to the cache]
* [Hinzufügen und Abrufen von Objekten zu bzw. aus dem Cache][Add and retrieve objects from the cache]
* [Arbeiten mit .NET-Objekten im Cache](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>

## <a name="connect-to-the-cache"></a>Herstellen einer Verbindung mit dem Cache
Sie benötigen einen Verweis auf den Cache, um programmgesteuert mit dem Cache arbeiten zu können. Fügen Sie die folgende Zeile oben in jeder Datei hinzu, in der Sie mithilfe des „StackExchange.Redis“-Clients auf einen Azure Redis Cache zugreifen möchten:

    using StackExchange.Redis;

> [!NOTE]
> Der StackExchange.Redis-Client benötigt .NET Framework 4 oder höher.
> 
> 

Die Verbindung zum Azure Redis Cache wird von der `ConnectionMultiplexer` -Klasse verwaltet. Diese Klasse sollte in Ihrer gesamten Clientanwendung freigegeben und wiederverwendet werden und muss nicht für jeden Vorgang neu erstellt werden. 

Um eine Verbindung zu einem Azure Redis Cache herzustellen und eine Instanz einer verbundenen `ConnectionMultiplexer`-Klasse zu erhalten, rufen Sie die statische `Connect`-Methode auf und übergeben den Cacheendpunkt und den Schlüssel. Übergeben Sie den im Azure-Portal generierten Schlüssel im Parameter für das Kennwort.

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

> [!IMPORTANT]
> Warnung: Speichern Sie niemals Anmeldeinformationen im Quellcode. Ich zeige diese Daten hier im Code, um dieses Beispiel zu vereinfachen. Informationen zum Speichern von Anmeldeinformationen finden Sie unter [Funktionsweise von Anwendungs- und Verbindungszeichenfolgen][How Application Strings and Connection Strings Work].
> 
> 

Wenn Sie SSL nicht verwenden möchten, legen Sie entweder `ssl=false` fest, oder lassen Sie den `ssl`-Parameter aus.

> [!NOTE]
> Der Nicht-SSL-Port ist für neue Caches standardmäßig deaktiviert. Informationen zum Aktivieren des Nicht-SSL-Ports finden Sie unter [Zugriffsports](cache-configure.md#access-ports).
> 
> 

Ein Ansatz zur Freigabe einer `ConnectionMultiplexer` -Instanz in Ihrer Anwendung ist das Verwenden einer statischen Eigenschaft, die wie im folgenden Beispiel eine verbundene Instanz zurückgibt. Dieser Ansatz ist eine threadsichere Möglichkeit, um nur eine einzelne verbundene `ConnectionMultiplexer`-Instanz zu initialisieren. In diesen Beispielen ist `abortConnect` auf „false“ festgelegt. Dies bedeutet, dass der Aufruf erfolgreich ist, auch wenn keine Verbindung mit Azure Redis Cache hergestellt wird. Eine wichtige Funktion von `ConnectionMultiplexer` ist, dass die Verbindung mit dem Cache automatisch wiederhergestellt wird, sobald das Netzwerkproblem oder andere Ursachen beseitigt wurden.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

Weitere Informationen zu erweiterten Optionen für den Verbindungsaufbau finden Sie unter [StackExchange.Redis-Konfigurationsmodell][StackExchange.Redis configuration model].

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

Nach dem Verbindungsaufbau können Sie einen Verweis auf diese Redis-Cachedatenbank zurückgeben, indem Sie die `ConnectionMultiplexer.GetDatabase` -Methode aufrufen. Das von der `GetDatabase` -Methode zurückgegebene Objekt ist ein einfaches Durchgangsobjekt und muss nicht gespeichert werden.

    // Connection refers to a property that returns a ConnectionMultiplexer
    // as shown in the previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Für Azure Redis-Caches kann eine konfigurierbare Anzahl von Datenbanken (standardmäßig 16) konfiguriert werden, mit denen die Daten innerhalb eines Redis-Caches logisch getrennt werden können. Weitere Informationen finden Sie unter [Was sind Redis-Datenbanken?](cache-faq.md#what-are-redis-databases) und [Standardmäßige Redis-Serverkonfiguration](cache-configure.md#default-redis-server-configuration).

Sie sind nun in der Lage, sich mit einer Azure Redis Cache-Instanz zu verbinden und einen Verweis auf die Cachedatenbank zurückzugeben, und können beginnen, mit dem Cache zu arbeiten.

<a name="add-object"></a>

## <a name="add-and-retrieve-objects-from-the-cache"></a>Hinzufügen zu und Abrufen von Objekten aus dem Cache
Sie können Elemente im Cache speichern und aus dem Cache abrufen, indem Sie die Methoden `StringSet` und `StringGet` verwenden.

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Redis speichert die meisten Daten als Redis-Zeichenfolgen. Diese können jedoch unterschiedliche Datentypen enthalten, inklusive serialisierter Binärdaten, die zum Speichern von .NET-Objekten im Cache verwendet werden können.

Beim Aufruf von `StringGet` wird das Objekt zurückgegeben, sofern es vorhanden ist. Andernfalls wird `null` zurückgegeben. Wenn `null` zurückgegeben wird, können Sie den Wert aus der gewünschten Datenquelle abrufen und für die zukünftige Verwendung im Cache speichern. Dieses Nutzungsmuster wird als „Cache-Aside Pattern“ (Cachefremdes Muster) bezeichnet.

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

Um die Ablaufzeit eines Elements im Cache anzugeben, verwenden Sie den `TimeSpan`-Parameter von `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-the-cache"></a>Arbeiten mit .NET-Objekten im Cache
Azure Redis Cache kann sowohl .NET-Objekte als auch primitive Datentypen zwischenspeichern, aber .NET-Objekte müssen zunächst serialisiert werden. Die Serialisierung des .NET-Objekts ist Aufgabe des Anwendungsentwicklers und überlässt dem Entwickler die Freiheit bei der Wahl des Serialisierers.

Eine einfache Möglichkeit zum Serialisieren von Objekten stellen das Verwenden der `JsonConvert` -Serialisierungsmethoden in [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) und Serialisieren in und aus JSON dar. Das folgende Beispiel zeigt „get“ und „set“ mit einer `Employee` - Objektinstanz.

    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }

        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store to cache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich nun mit den Grundlagen vertraut gemacht haben, lesen Sie die folgenden Artikel zu Azure Redis Cache.

* Überprüfen Sie die ASP.NET-Anbieter für Azure Redis Cache.
  * [Azure Redis-Sitzungszustandsanbieter](cache-aspnet-session-state-provider.md)
  * [Azure Redis Cache ASP.NET-Ausgabecacheanbieter](cache-aspnet-output-cache-provider.md)
* [Aktivieren Sie die Cachediagnose](cache-how-to-monitor.md#enable-cache-diagnostics), damit Sie die Integrität Ihres Caches [überwachen](cache-how-to-monitor.md) können. Sie können die Metriken im Azure-Portal anzeigen und anschließend mit einem Tool Ihrer Wahl [herunterladen und prüfen](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) .
* Lesen Sie die [Dokumentation für den StackExchange.Redis-Cacheclient][StackExchange.Redis cache client documentation].
  * Auf Azure Redis Cache können viele Redis-Clients und Entwicklungssprachen zugreifen. Weitere Informationen finden Sie unter [http://redis.io/clients][http://redis.io/clients].
* Azure Redis Cache kann auch mit Drittanbieterdiensten und -tools wie Redsmin und Redis Desktop Manager verwendet werden.
  * Weitere Informationen zu Redsmin finden Sie unter [Abrufen einer Azure Redis-Verbindungszeichenfolge und ihre Verwendung mit Redsmin][How to retrieve an Azure Redis connection string and use it with Redsmin].
  * Sie können über eine GUI unter Verwendung von [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager)in Azure Redis Cache auf Ihre Daten zugreifen und die Daten anzeigen.
* Lesen Sie außerdem die [Redis][redis]-Dokumentation und die Artikel zu [Redis-Datentypen][redis data types] und die [15-minütige Einführung in Redis-Datentypen][a fifteen minute introduction to Redis data types].

<!-- INTRA-TOPIC LINKS -->
[Next Steps]: #next-steps
[Introduction to Azure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project to Use Azure Caching]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[Create the cache]: #create-cache
[Configure the cache]: #enable-caching
[Configure the cache clients]: #NuGet
[Working with Caches]: #working-with-caches
[Connect to the cache]: #connect-to-cache
[Add and retrieve objects from the cache]: #add-object
[Specify the expiration of an object in the cache]: #specify-expiration
[Store ASP.NET session state in the cache]: #store-session


<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png







<!-- LINKS -->
[http://redis.io/clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[How to retrieve an Azure Redis connection string and use it with Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[StackExchange.Redis configuration model]: http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md

[Work with .NET objects in the cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Cache Pricing Details]: http://www.windowsazure.com/pricing/details/cache/
[Azure portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate to Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups to manage your Azure resources]: ../azure-resource-manager/resource-group-overview.md

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[StackExchange.Redis cache client documentation]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[Redis data types]: http://redis.io/topics/data-types
[a fifteen minute introduction to Redis data types]: http://redis.io/topics/data-types-intro

[How Application Strings and Connection Strings Work]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/



